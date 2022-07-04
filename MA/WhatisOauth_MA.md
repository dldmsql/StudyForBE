# What is OAuth?

### OAuth이란?
 >사용자가 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준 
 
 >사용 이유? 사용자가 안전하게 다른 서비스의 정보를 우리 서비스에 건네주기 위한 방법 ... 예를 들어 네이버 로그인 위해서 사용자의 네이버 아이디, 비번 가져오는거 말안됨

 >OAuth 2.0 부터는 반드시 HTTPS 사용, 웹이 아닌 애플리케이션도 지원, Access token 만료 시간 설정 등이 추가

### OAuth 2.0 용어
- Resource Owner : 자원의 소유자,  
실제 사용자
- Client Application : 사용자가 사용하는 서비스 애플리케이션. 
- Resource Server : OAuth를 통해 인증, 인가를 제공해주는 서버. 자원 서버. 자원(이름, 이메일, 프로필 사진 등)을 제공(ex. github, naver, kakao, google)
- Authorization Server : OAuth를 통해 인증, 인가를 제공해주는 서버. 인증 서버. 토큰을 발급해준다.(ex. github, naver, kakao, google)

Resource Server와 Authorization Server는 자원과 인증으로 분리되어 있긴 하지만, 둘 다 같은 소속

### OAuth 2.0 인증 방식
1. Authorization Code Grant </br>
 -> 앱/웹에서 가장 많이 사용 </br>
 OAuth 서버에서 client Application에게 access token이 아닌 Authorization code를 넘겨주고, client Application은 Authorization code를 통해 access token을 발급, access token으로 허가된 리소스 요청을 하는 방식
2. Implicit Grant </br>
암시적 승인 ... 바로 access token을 발급받는 방법
3. Resource Owner Password Credentials Grant </br>
클라이언트가 암호를 사용하여 엑세스 토큰에 대한 사용자의 자격 증명
4. Client Credentials Grant </br>
클라이언트가 컨텍스트 외부에서 액세스 토큰을 얻어 특정 리소스에 접근을 요청할 때 사용하는 방식

### Authorization Code Grant Flow
1. 클라이언트가 파리미터러 클라이언트 ID, 리다이렉트 URI, 응답 타입을 code로 지정하여 권한 서버에 전달 </br>
정상적으로 인증이 되면 권한 코드 부여 코드를 클라이언트에게 보냄
응답 타입은 code, token 이 사용 가능(
응답 타입이 token 일 경우 암시적 승인 타입에 해당)
2. 성공적으로 권한 부여 코드를 받은 클라이언트는 권한 부여 코드를 사용하여 엑세스 토큰을 권한 서버에 추가로 요청</br> 이때 필요한 파라미터는 클라이언트 ID, 클라이언트 비밀번호, 리다이렉트 URI, 인증 타입 
3. 마지막으로 받은 엑세스 토큰을 사용하여 리소스 서버에 사용자의 데이터 전

OAuth를 통해 SNS 로그인 기능을 구현했어도 실제 개발할 애플리케이션의 인증 방법은 별도로 필요 -> 세션/쿠키나 토큰을 사용한 별도의 인증 작업 필요

### OAuth 흐름 _ [EB](https://github.com/dldmsql)
* 프론트 + 백 혼합으로 인증 과정 수행
이 방식이 REST API에 적합한 방식이다.

[FLOW]
1. User가 웹 브라우저에서 간편 로그인 버튼을 누른다. 
2. Front-End는 버튼에 해당하는 GET 요청을 Spring API에 보낸다.
3. Spring API는 (구글, 네이버, 카카오)에 맞는 Redirect URL을 Front-End에 응답한다.
4. Front-End는 그에 맞는 웹 페이지를 User에게 보여준다.
5. User는 해당 페이지에 로그인을 위한 입력을 한다.
6. Front-End는 인가 코드와 Spring API에서 넘겨 받은 Redirect URL을 Authorization Server에 요청 보낸다.
7. Authorization Server는 인가 코드를 Spring API에 응답한다.
8. Spring API는 인가 코드와 함께 Access Token을 Authorization Server에 요청한다.
9. Authorization Server는 인가 코드를 확인 후, Access Token을 응답한다.
10. Spring API는 Resource Server에 Access Token과 함께 user info를 요청한다.
11. Resourece Server는 Access Token을 확인 후, user info를 응답한다.
12. Spring API는 user info를 DB에 저장 후, JWT token과 Refresh Token을 만들어, refresh token을 DB에 저장한다.
13. 그 후, refresh token을 cookie에 담고, redirect url에 token을 닮도록 Front-End에 응답한다.
14. Front-End는 브라우저의 쿠키 혹은 local storage에 Spring API로 부터 받은 token을 저장한다.
15. Front-End는 jwt token을 가지고 Spring API에 user info를 get 요청한다.
16. Spring API는 token 검증 후, user info를 응답한다.
17. Front-end는 user info를 front단에 저장 후, User에게 보여준다.
[FLOW] - Exception ( about expired token )
1. User가 어떤 행위를 한다.
2. Fornt-End는 그 행위에 따른 Get 요청을 Spring API에 보낸다.
3. Spring API는 token이 만료되었음을 에러 코드와 함께 Front-End에 보낸다.
4. Front-end는 만료된 token과 refresh token을 함께 Spring API에 보낸다.
5. Spring API는 만료된 JWT token과 refresh token을 검증하고, 새로운 jwt token과 refresh token을 만들어 refresh token을 DB에 저장한다.
6. Spring API는 새로운 jwt token을 응답하고, 쿠키에 refresh token을 담는다.
### Spring boot flow
위의 과정은 spring security가 관여한다.
1. OAuth2LoginAuthenticationFilter에서 OAuth2 로그인 과정이 수행된다.
2. OAuth2 Filter 단에서 직접 customizing 한 oauth2 service의 `lodUser`메소드가 실행된다.
3. 로그인을 성공하게 되면, success handler의 `onAuthenticationSuccess`메소드가 실행된다.
4. success handler에서 최초 로그인 확인 및 jwt 생성, 응답 과정이 실행된다.

### Spring Security란?
>Spring 기반의 애플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크
- '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리
Filter는 Dispatcher Servlet으로 가기 전에 적용되므로 가장 먼저 URL 요청을 받지만, Interceptor는 Dispatcher와 Controller사이에 위치한다는 점에서 적용 시기의 차이가 있다. 
- Spring Security는 보안과 관련해서 체계적으로 많은 옵션을 제공해주기 때문에 하나하나 보안관련 로직을 작성하지 않아도 된다.

### JWT
>JWT(JSON Web Token)
유저를 인증, 식별하기 위한 토큰 기반 인증

- 세션과 달리 서버가 아닌 클라이언트에 저장되어 서버의 부담이 줄어든다.

- JWT 자체에 사용자의 권한 정보나 서비스를 사용하기 위한 정보가 포함된다.

- 세션과 달리 확장성이 좋다.
  만약 여러 서버가 돌아가는 상황이라면, 각 서버마다 세션 저장소를 두거나, 공통 세션 저장소를 만들어야하는데, 이 또한 비용이다. 반면에 토큰은 RESTful 같은 stateless(무상태) 환경에서 사용자 데이터를 주고받을 수 있다.

- 토큰은 한 번 발급되면 강제로 만료시킬 수 없다는 단점이 있다. 따라서 만료 시간이 짧은 access token과 만료 시간이 긴 refresh token을 나눠서 사용한다.

- 사용 순서</br>
    1. 클라이언트 사용자가 아이디, 패스워드 통해 웹서비스 인증
    2. 서버에서 서명된 JWT 생성하여 클라이언트에 응답으로 돌려주기
    3. 클라이언트가 서버에 데이터를 추가적으로 요구할 때 JWT를 HTTP 헤더에 첨부
    4. 서버에서 클라이언트로부터 온 JWT 검증

### JWT 구조/구성
>각각 .으로 구분되는 Header, Payload, Signature로 구성
- Header : JWT에서 사용할 타입과 해시 알고리즘의 종류
- Payload : 서버에서 첨부한 사용자 권한 정보와 데이터
- Signature : 위 내용을 Base64 URL-safe Encode 한 후 해시함수 적용, private key로 서명한 전자서명한 내용

## 참고자료
https://velog.io/@max9106/OAuth
https://cheese10yun.github.io/oauth2/
https://pronist.dev/143
https://mangkyu.tistory.com/76
https://github.com/dldmsql/StudyForBE
 
