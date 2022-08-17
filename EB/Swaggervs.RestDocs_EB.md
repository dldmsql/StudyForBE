# What is difference between swagger and restdocs

## What is swagger

Swagger는 REST API를 설계, 빌드, 문서화 및 사용하는 데 도움이 되는 OpenAPI 사양을 중심으로 구축된 오픈 소스 도구 세트이다.

## Why to use

* 적용이 쉽다

RestDocs 문서화 도구와 달리 테스트 자동화 적용이 쉽다.

* 테스트 할 수 있는 UI를 제공한다.

웹 브라우저에서 `localhost:8080/swagger-ui.html`로 접속하면 테스트 가능한 UI가 보여진다.

## How to use

1. `build.gradle`에 의존성을 추가해준다.

```` bash
plugins {
    id 'org.springframework.boot' version '2.7.1'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id "org.springdoc.openapi-gradle-plugin" version "1.3.4" // 이 코드 추가
    id 'java'
}

...
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-mongodb-reactive'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-webflux'


    implementation 'org.springdoc:springdoc-openapi-webflux-ui:1.6.9' // 이 코드 추가
    compileOnly 'org.projectlombok:lombok' // 이 코드 추가
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    annotationProcessor 'org.projectlombok:lombok'  // 이 코드 추가
...
}

...

````

2. YAML 파일 설정 추가하기

```` bash
com:
  example:
    backend_webflux:
      springdoc:
        api:
          router:
            separete:
              enabled: false
            common:
              enabled: true
````

> router - handler 방식의 endpoint를 인식하도록 하기 위해서이다.

3. Router 클래스에 어노테이션으로 swagger 설정 추가하기

```` bash

@Configuration
public class UserRouter {

  @Bean
  @GetAllUserApiInfo
  public RouterFunction<ServerResponse> getAllUserRouter(UserHandler userHandler) {
    return RouterFunctions
        .route(GET("/api/user")
            , userHandler::getAllUser)
        ;
  }

  @Bean
  @GetUserByIdApiInfo
  public RouterFunction<ServerResponse> getUserRouter(UserHandler userHandler) {
    return RouterFunctions
        .route(GET("/api/user/{id}")
            , userHandler::getUser);
  }

  @Bean
  @CreateUserApiInfo
  public RouterFunction<ServerResponse> createUserRouter(UserHandler userHandler) {
    return RouterFunctions
        .route(POST("api/user")
            , userHandler::createUser);
  }

  @Bean
  @ModifyUserByIdApiInfo
  public RouterFunction<ServerResponse> modifyUserRouter(UserHandler userHandler) {
    return RouterFunctions
        .route(PUT("api/user/{id}")
            , userHandler::modifyUser);
  }

  @Bean
  @DeleteUserByIdApiInfo
  public RouterFunction<ServerResponse> deleteUserRouter(UserHandler userHandler) {
    return RouterFunctions
        .route(DELETE("api/user/{id}")
            , userHandler::deleteUser);
  }

}

````

`@Bean` 어노테이션 아래에 커스터마이징 한 어노테이션이 Swagger 설정을 위한 어노테이션이다. router 단의 코드에서 가독성을 높이기 위해 어노테이션으로 설정을 추가하는 방식으로 구현하였다.

4. 어노테이션 커스터마이징 하기

```` bash
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.METHOD, ElementType.TYPE})
@RouterOperations({
    @RouterOperation(
        method = RequestMethod.GET,
        operation =
        @Operation(
            description = "Get all users ",
            operationId = "getAllUser",
            tags = "users",
            responses = {
                @ApiResponse(
                    responseCode = "200",
                    description = "Get all users endpoint",
                    content = {
                        @Content(
                            mediaType = MediaType.APPLICATION_JSON_VALUE,
                            array = @ArraySchema(schema = @Schema(implementation = User.class)))
                    }),
                @ApiResponse(
                    responseCode = "400",
                    description = "Not found response",
                    content = {
                        @Content(
                            mediaType = MediaType.APPLICATION_JSON_VALUE,
                            schema = @Schema(implementation = ExceptionResponse.class))
                    })
            }))
})
public @interface GetAllUserApiInfo {

}
````

* @Retention

어노테이션이 적용되는 범위를 설정한다.

RetentionPolicy.RUNTIME : 컴파일 이후에도 JVM에 의해서 참조가 가능하다.

* @Target

어노테이션이 사용될 위치를 설정한다.

ElementType.METHOD : 메소드 선언 시 사용

ElementType.TYPE : 타입 선언 시 사용

* @RouterOperations

spring-webflux with functional endpoints 방식에서 각 라우터마다 swagger 설정을 하기 위한 어노테이션이다.

* @Operation

실제로 코드상에서 라우터로 요청이 들어올 때, 실행되는 함수에 대한 설정이다.

함수명을 동일하게 작성해줘야 한다.

* @ApiResponse

응답 시, 상태 코드와 그에 대한 설명, 에러가 있다면 에러를 지정할 수 있다.

## What is RestDocs

RETful 서비스의 문서화를 도와주는 도구이다. Sprnig REST Docs는 기본적으로 Asciidotor를 사용하며, 이것을 사용해 HTML을 생성한다. 필요한 경우, 마크다운을 사용하도록 변경할 수 있다.

SpringMVC의 테스트 프레임워크로, Spring Webflux WebTestClient 또는 RestAssured 3로 작성된 테스트 코드에서 생성된 Snippet을 사용한다. 테스트 기반 접근 방식은 서비스 문서의 정확성을 보장해준다. snippet이 올바르지 않을 경우, 테스트가 실패하기 때문이다.

## Whyt to use

* 코드의 추가 및 수정이 없다.

Spring REST Docs는 서비스 내부 코드에 추가 또는 수정과 같이 영향을 주지 않고도 별도의 문서화 작업을 진행할 수 있게 된다. 

* 테스트 코드 작성 강제화

테스트 코드 작성이 필요하며, 테스트 성공 시 문서가 생성된다.

* 버전 변화에 유연

문서를 재작업하지 않고도 버전에 따라 변경할 수 있다. 

## How to use

1. `build.gradle`에 의존성 추가하기

```` bash

plugins {
	id 'org.springframework.boot' version '2.7.0'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
	id 'org.asciidoctor.jvm.convert' version '3.3.2' // 이 코드 추가
	id 'checkstyle'
}

... 

dependencies { 
    ... 

    asciidoctorExt 'org.springframework.restdocs:spring-restdocs-asciidoctor:2.0.6.RELEASE'
	testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc:2.0.6.RELEASE'

    ...

    compileOnly 'org.projectlombok:lombok:1.18.24'
	annotationProcessor 'org.projectlombok:lombok:1.18.24'
	testCompileOnly 'org.projectlombok:lombok:1.18.24'
	testAnnotationProcessor 'org.projectlombok:lombok:1.18.24'

    ...
}

ext {
	snippetsDir = file('build/generated-snippets') // snippet 생성 위치 지정
}

test { // Snippets 디렉토리를 출력으로 작업하도록 설정
	outputs.dir snippetsDir
	useJUnitPlatform()
}

asciidoctor {
    // Snippets 디렉토리를 Input 디렉토리로 설정
	inputs.dir snippetsDir
	configurations 'asciidoctorExt'
    // 문서 생성 전 테스트가 실행되도록 test 에 종속 설정
	dependsOn test
}

asciidoctor.doFirst {
	delete file('build/docs/asciidoc')
	delete file('src/main/resources/static/docs')
}

task copyDocument(type: Copy) { // 이게 문서 패키징
    // 빌드 전 문서 생성 확인
	dependsOn asciidoctor
    // 생성된 문서를 src/main/resources/static/docs 에 복사
	from file("build/docs/asciidoc")
	into file("src/main/resources/static/docs")
}

...

````

생성된 API 문서를 jar 또는 war 에 패키징하기 위한 설정을 추가해야 한다. 이는 spring boot에 의해 정적 콘텐츠로 제공된다.

> 정적 콘텐츠를 제공하는 서버는 web server이다.

jar 가 빌드 되기 전에 문서가 생성되고, 이 문서는 jar에 포함된다.

2. snippet 생성하기

* Junit 5 테스트 설정

@ExtendWith 어노테이션 작성하기

MockMVC 객체 생성하기

setUp() 함수 작성하기

* 실제 요청 테스트 코드 작성하기

컨트롤러 단위 테스트 코드이기 때문에, UserController에 Get 요청하는 함수를 테스트할 것이다. 테스트를 위해 UserService와 그에 필요한 다른 service, util 클래스들을 @MockBean으로 생성한다. 

```` bash

@ActiveProfiles("test")
@ExtendWith({RestDocumentationExtension.class, SpringExtension.class}) // JUnit 5 테스트 설정 코드 추가하기
@WebMvcTest(UserController.class)
public class UserDocumentationTests {

  @Autowired
  private MockMvc mockMvc; // 가짜 객체 생성하기
  @MockBean
  private UserService userService;
  @MockBean
  private UserInterceptor userInterceptor;
  @MockBean
  private JwtProvider jwtProvider;
  @MockBean
  private PasswordEncoder passwordEncoder;
  @Autowired
  private ObjectMapper objectMapper;
  @MockBean
  private Session session;
  @MockBean
  private Authentication authentication;
  @MockBean
  private SecurityContext securityContext;

  //DTO
  private final ProfileDto.Request request =
      ProfileDto.Request.builder()
          .address("인천 계양구")
          .imageUrl("이미지")
          .name("김영희")
          .nickname("groot")
          .phone("01012345678")
          .build();
  private final ProfileDto.Response response =
      ProfileDto.Response.builder()
          .address("인천 계양구")
          .email("groot@example.com")
          .nickname("groot")
          .name("김영희")
          .imageUrl("이미지")
          .phone("01012345678")
          .build();
  private final UserDto.PasswordRequest passwordRequest =
      UserDto.PasswordRequest.builder()
          .currentPassword("groot1234*")
          .newPassword("abcd1234!")
          .build();

  @BeforeEach
  void setUp(WebApplicationContext webApplicationContext,
      RestDocumentationContextProvider restDocumentation) { // 문서화를 위한 설정과 mock 객체 초기화
    mockMvc = MockMvcBuilders.webAppContextSetup(webApplicationContext).apply(
        documentationConfiguration(restDocumentation).operationPreprocessors()
            .withRequestDefaults(prettyPrint()).withResponseDefaults(prettyPrint())).build();
  }

  @Test
  @DisplayName("사용자 프로필 조회")
  public void getUserProfile() throws Exception {

    // JWT 토큰 기반 인증 방식이기에 필요한 코드이다.
    when(securityContext.getAuthentication()).thenReturn(authentication);
    SecurityContextHolder.setContext(securityContext);
    when(SecurityContextHolder.getContext().getAuthentication().getPrincipal()).thenReturn(session);
    
    // 요청이 들어오면 실행되는 서비스 로직을 호출한다.
    given(userService.getUserProfile(session.getId())).willReturn(response);

    // RESTful 하게 들어오는 요청과 응답에 대한 정보를 정의한다.
    ResultActions resultActions = mockMvc.perform(
        RestDocumentationRequestBuilders
            .get("/user/profile")
            .characterEncoding("utf-8")
            .accept(MediaType.APPLICATION_JSON));

    // 실제 snippet으로 출력할 정보들을 여기에 작성한다.
    resultActions.andExpect(status().isOk())
        .andDo(print())
        .andDo(
            document("profile-get", getDocumentRequest(), getDocumentResponse(),
                responseFields(
                    fieldWithPath("status").description("결과 코드"),
                    fieldWithPath("message").description("응답 메세지"),
                    fieldWithPath("data.nickname").description("닉네임"),
                    fieldWithPath("data.email").description("이메일"),
                    fieldWithPath("data.name").description("이름"),
                    fieldWithPath("data.phone").description("핸드폰 번호"),
                    fieldWithPath("data.address").description("주소"),
                    fieldWithPath("data.imageUrl").description("이미지 url")
                )));
  }
}

````

3. build 하기

결과물로 snippet이 만들어졌다면, target 폴더에서 확인한 후, static/html/docs 하위에 html이 생성된 것을 확인할 수 있다. 

이 html 파일에 커스터마이징 하면 최종적으로 완성된 문서를 확인할 수 있다.

## Review about 2 docs tool

두 자동화 도구를 사용해봤다. **개인적으로 RestDocs가 좀 더 안정적이라고 느꼈다.** swagger로 UI를 통해 테스트해 볼 수 있음과 다른 팀원들과 공유하기에도 편하다는 것은 인정한다. 또한, 문서 자동화를 위한 설정이 어렵지 않음 역시 매력적이다. 하지만, REST Docs는 테스트 코드 작성을 강제화 하여 테스트를 성공해야만 문서가 만들어진다는 점에서 안정적이라는 느낌을 받았다. 물론 코드 작성이 귀찮을 수 있지만, 실제 서비스 로직에 어떠한 추가를 가하지 않아도 된다는 점이 매력적이다.
