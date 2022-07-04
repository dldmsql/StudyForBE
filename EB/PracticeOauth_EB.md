## Naver 간편 로그인 구현하기 (Spring Security   JWT   Naver)

### Spring Boot Flow
  Spring Security Filter 과정에서 모든 것이 수행된다. 
  1. 가장 먼저 OAuth2LoginAuthenticationFilter에서 OAuth2 로그인 과정을 처리한다.
  2. OAuth2 Filter layer에서 `CustomOAuth2UserService`의 `loadUser` 메소드가 실행된다.
  3. 로그인 성공 시, OAuth2SuccessHandler의 `onAuthenticationSuccess` 메소드가 실행된다.
  4. OAuth2SuccessHandler에서 최초 로그인 확인 및 JWT 생성/응답 과정이 실행된다.

### 구현
*build.gradle 설정*
```` bash
dependencies {
// for security
    implementation 'org.springframework.boot:spring-boot-starter-oauth2-client'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
// default
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
// for security
    implementation 'io.jsonwebtoken:jjwt-api:0.11.2'
    runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.11.2'
    runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.11.2'
// default
    implementation 'junit:junit:4.13.1'
    runtimeOnly 'mysql:mysql-connector-java'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
// for security
    testImplementation 'org.springframework.security:spring-security-test'
}
````
*Security Config 설정*
```` bash
@RequiredArgsConstructor
@EnableWebSecurity
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

  private final CustomOAuth2UserService oAuth2UserService;
  private final OAuth2SuccessHandler successHandler;
  private final TokenService tokenService;

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http.httpBasic().disable()
        .csrf().disable()
        .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        .and()
        .authorizeRequests()
        .antMatchers("/**").permitAll()
        .anyRequest().authenticated()
        .and()
        .addFilterBefore(new JwtAuthFilter(tokenService),
            UsernamePasswordAuthenticationFilter.class)
        .oauth2Login().loginPage("/api/main")
        .successHandler(successHandler)
        .userInfoEndpoint().userService(oAuth2UserService);

    http.addFilterBefore(new JwtAuthFilter(tokenService), UsernamePasswordAuthenticationFilter.class);
  }
}
````
* `.antMatchers("/**").permitAll()` <br/>
모든 url에 대해 접근을 허용하겠다는 의미이다. 추후, 로그인 없이 접근 불가능한 url에 대해 block할 예정이다.
* `.oauth2Login().loginPage("/api/main")` <br/>
로그인 페이지 url을 직접 설정해줄 수 있다. front를 구현하지 않았기 때문에, "/api/main"으로 설정했다.
* `.successHandler(successHandler)` <br/>
로그인 성공 시, handler를 설정해준다.
* `.userInfoEndpoint().userService(oAuth2UserService)` <br/>
oauth2 로그인 성공 후, `oAuth2UserService`에서 그 후처리를 하겠다는 의미이다.
* JWTAuthFilter 실행 후, UserNamePasswordAuthenticationFilter가 실행된다.

*CustomOAuth2UserService 생성*
```` bash
@Service
public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {
  @Override
  public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
    //  1
    OAuth2UserService<OAuth2UserRequest, OAuth2User> oAuth2UserService = new DefaultOAuth2UserService();

    //	2
    OAuth2User oAuth2User = oAuth2UserService.loadUser(userRequest);

    //	3
    String registrationId = userRequest.getClientRegistration().getRegistrationId();
    String userNameAttributeName = userRequest.getClientRegistration()
        .getProviderDetails().getUserInfoEndpoint().getUserNameAttributeName();


    // 4
    OAuth2Attribute oAuth2Attribute =
        OAuth2Attribute.of(registrationId, userNameAttributeName, oAuth2User.getAttributes());

    var memberAttribute = oAuth2Attribute.convertToMap();

    return new DefaultOAuth2User(
        Collections.singleton(new SimpleGrantedAuthority("ROLE_USER")),
        memberAttribute, "email");
  }
}
````
* 1. `DefaultOAuth2UserService` 객체를 생성한다.
* 2. `oAuth2UserService.loadUser(userRequest);` 생성된 service로 부터 oauthUser 정보를 받는다.
* 3. userRequest로 부터 registrationId, userNameAttributeName를 얻는다.
     > registrationId는 provider를 말하고, <br/> userNameAttributeName은 attributeKey를 말한다.
* 4. 2번과 3번에서 얻은 정보들을 이용해 `OAuth2Attribte를 생성한다.

*OAuth2Attribute 생성*
```` bash
@Getter
public class OAuthAttributes {
  private Map<String, Object> attributes; // OAuth2 반환하는 유저 정보 Map
  private String attributeKey;
  private String name;
  private String email;
  private String picture;

  @Builder
  public OAuthAttributes(Map<String, Object> attributes, String attributeKey, String name, String email, String picture) {
    this.attributes = attributes;
    this.attributeKey= attributeKey;
    this.name = name;
    this.email = email;
    this.picture = picture;
  }
  public static OAuthAttributes of(String registrationId, String attributeKey, Map<String,Object> attributes){

      return ofNaver(attributeKey,attributes);

  }
  private static OAuthAttributes ofNaver(String attributeKey, Map<String, Object> attributes){
    Map<String, Object> response = (Map<String, Object>) attributes.get("response");

    return OAuthAttributes.builder()
        .name((String) response.get("name"))
        .email((String) response.get("email"))
        .picture((String) response.get("picture"))
        .attributes(response)
        .attributeKey(attributeKey)
        .build();
  }

}
````
* `CustomOAuth2UserService`에서 생성한, OAuth2Attribute는 `of()`를 통해 provider를 구분한다. <br/> 본 예제에서는 **NAVER**만 구현하였다.
* response라는 키워드로 attribute를 가져온다.

*OAuth2 Success Handler 구현*
```` bash
@RequiredArgsConstructor
@Component
public class OAuth2SuccessHandler extends SimpleUrlAuthenticationSuccessHandler {
  private final TokenService tokenService;
  private final UserRequestMapper userRequestMapper;
  private final ObjectMapper objectMapper;

  @Override
  public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication)
      throws IOException, ServletException {
    OAuth2User oAuth2User = (OAuth2User)authentication.getPrincipal();
    UserDto userDto = userRequestMapper.toDto(oAuth2User);

    String targetUrl;
    Token token = tokenService.generateToken(userDto.getEmail(), "USER");

    targetUrl = UriComponentsBuilder.fromUriString("/api/main/signUp")
        .queryParam("token", "token")
        .build().toUriString();
    getRedirectStrategy().sendRedirect(request, response, targetUrl);
  }
}
````
* 로그인 완료 시, 실행되는 클래스이다.
* 최초 로그인 여부와 Access Token, Refresh Token 생성 및 발급, Token을 포함한 리다이렉트를 담당한다.

*Token Service 구현*
```` bash
@Service
public class TokenService{
  private String secretKey = "token-secret-key";

  @PostConstruct
  protected void init() {
    secretKey = Base64.getEncoder().encodeToString(secretKey.getBytes());
  }


  public Token generateToken(String uid, String role) {
    long tokenPeriod = 1000L * 60L * 10L;
    long refreshPeriod = 1000L * 60L * 60L * 24L * 30L * 3L;

    Claims claims = Jwts.claims().setSubject(uid);
    claims.put("role", role);

    Date now = new Date();
    return new Token(
        "accesstoken",
        "refreshToken");
  }


  public boolean verifyToken(String token) {
    try {
      Jws<Claims> claims = Jwts.parser()
          .setSigningKey(secretKey)
          .parseClaimsJws(token);
      return claims.getBody()
          .getExpiration()
          .after(new Date());
    } catch (Exception e) {
      return false;
    }
  }


  public String getUid(String token) {
    return Jwts.parser().setSigningKey(secretKey).parseClaimsJws(token).getBody().getSubject();
  }
}

````
* 토큰을 생성하고, 검증하는 기능을 담당한다.
* secretKey 는 토큰 유효성 검사 시 사용하기 때문에 외부에 노출되지 않도록 해야 한다.
* claim은 payload 부분에 들어갈 정보 조각들이다.
  Registered | Public | Private 이렇게 나눠진다.

*JWT Filter 생성*
```` bash
@RequiredArgsConstructor
class JwtAuthFilter extends GenericFilterBean {
  private final TokenService tokenService;

  @Override
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
      throws IOException, ServletException, IOException {
    String token = ((HttpServletRequest)request).getHeader("Auth");

    if (token != null && tokenService.verifyToken(token)) {
      String email = tokenService.getUid(token);

      // DB연동을 안했으니 이메일 정보로 유저를 만들어주겠습니다
      UserDto userDto = UserDto.builder()
          .email(email)
          .name("이름이에용")
          .picture("프로필 이미지에요").build();

      Authentication auth = getAuthentication(userDto);
      SecurityContextHolder.getContext().setAuthentication(auth);
    }

    chain.doFilter(request, response);
  }

  public Authentication getAuthentication(UserDto member) {
    return new UsernamePasswordAuthenticationToken(member, "",
        Arrays.asList(new SimpleGrantedAuthority("ROLE_USER")));
  }
}
````
* 토큰 검증이 통과되면, 토큰으로부터 고유 식별자 값 ( email )을 받아오고, 이를 새로운 user로 생성한다. <br/> 그 후, securityContextHolder.getContext()에 Authentication 객체를 등록한다.

아래는 데모 화면 캡처이다. <br/>
웹 브라우저와 postman 모두 간편 로그인을 거친 뒤, 토큰을 통해 다른 기능에 접근 가능하다.
![image](https://user-images.githubusercontent.com/61505572/176627565-341fd308-7641-4b9d-b277-fed8977c28f2.png)

[간편로그인](https://velog.io/@jkijki12/Spring-Boot-OAuth2-JWT-%EC%A0%81%EC%9A%A9%ED%95%B4%EB%B3%B4%EB%A6%AC%EA%B8%B0) | [Claim](https://yeon-blog.tistory.com/3) | [Spring security](https://cjw-awdsd.tistory.com/45) 참고