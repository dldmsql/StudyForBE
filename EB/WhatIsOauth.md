# What is OAuth2
> 목차
> * What is Oauth
> * How to develop
> * What is cookie and session

## What is Oauth
회원에 관한 모든 동작을 타 서비스에 의탁하는 것이다.
> 동작에는 회원의 ID 관리, 비밀번호 관리 등이 있다.

Oauth2는 회원의 계정 정보를 다른 서비스에 의탁하기 위한 프로토콜이다. 대표적으로 kakao, google, naver 등이 있다.

Oauth2 서버 구조에 따른 방식
oauth2는 서비스의 서버 구조마다 다른 흐름으로 동작한다.
대표적으로 3가지 방식이 있다.
1. 프론트에서 모든 인증 과정 수행
2. 백에서 모든 인증 과정 수행
3. 프론트 + 백 혼합으로 인증 과정 수행

* 프론트에서 모든 인증 과정 수행
이 방식은 보통 프론트로만 서비스가 수행될 경우이다. 즉, React에서 제공하는 Next-Auth라는 라이브러리르 사용해서 백엔드에 어떠한 요청을 보내지 않는 경우이다.

* 백에서 모든 인증 과정 수행
이 방식은 백에서 페이지 관리까지 수행할 경우에 사용한다. 이는, REST API에는 부적합하다.
> REST API는 자원 중심인데, 페이지에 의존하게 되면 RESTful 하다고 할 수 없다.

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

## Spring boot flow
위의 과정은 spring security가 관여한다.
1. OAuth2LoginAuthenticationFilter에서 OAuth2 로그인 과정이 수행된다.
2. OAuth2 Filter 단에서 직접 customizing 한 oauth2 service의 `lodUser`메소드가 실행된다.
3. 로그인을 성공하게 되면, success handler의 `onAuthenticationSuccess`메소드가 실행된다.
4. success handler에서 최초 로그인 확인 및 jwt 생성, 응답 과정이 실행된다.

## 참고자료
[springboot-oauth2-jwt](https://velog.io/@jkijki12/Spring-Boot-OAuth2-JWT-%EC%A0%81%EC%9A%A9%ED%95%B4%EB%B3%B4%EB%A6%AC%EA%B8%B0)

## What is cookie and session
* cookie
사용자가 방문한 웹 사이트 서버가 사용자 컴퓨터에 저장하는 작은 기록 정보 파일을 말한다. <br/>
클라이언트 컴퓨터에 저장했다가 필요 시 참조하거나 재사용 가능한다.
<br/>
cookie 저장 시, 설정한 만료 시점까지 삭제 불가능하다.
<br/>
cookie 정보는 노출되고 수정될 수 있는 위험이 있다.
<br/>
cookie는 key, value 형태로 이루어져 있다. 
<br/>

cookie가 필요한 이유는 **HTTP는 Connectionless하면서 Stateless하기** 때문이다. 
> HTTP는 상태 정보를 유지하지 않는다. <br/> 누가 요청을 보냈는지 모르고 IP 주소와 브라우저 정보 정도만을 알고 있다. <br/> 로그인 상태를 유지하려면 어떻게 해야 할까 ? <br/> 그래서 등장한 게 cookie이다.
cookie를 통해 서버와 클라이언트 간 접속 상태 정보가 있는 세션을 유지할 수 있다. <br/>
매 request마다 서버에 cookie를 동봉해서 보내고, 서버는 cookie를 읽어 request를 보낸 주체를 파악한다. <br/> cookie는 request header에 담긴다.

* session
일정 시간동안 같은 사용자로부터 들어오는 일련의 request를 하나의 상태로 보고, 그 상태를 일정하게 유지시키는 기술이다.
<br/>
session 정보는 웹 서버에 저장한다.
<br/>
브라우저 종료 시 삭제되기 때문에 cookie보다 안전하다.
<br/>
중요한 정보는 서버에서 관리하고 클라이언트에는 session 키만 제공한다.
<br/>
서버에 session 객체 생성 후, uniqueInt(키)를 만들어 속성명으로 사용한다.
<br/>
속성 값에 정보를 저장하고 uniqueInt를 클라이언트에 보낸다.



