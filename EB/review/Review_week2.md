# keyword
> JWT, Oauth, Access token, Refresh token, Http, stateless, connectionless, 
> Node.js, spring bean, multi-thread, singleton

# next week
1. node.js 멀티 쓰레드 
2. refesh token 을 어떤 식으로 저장하고 사용하는가? access token도 저장하나 ?

> 목차
> 1. 쿠키와 세션은 무엇인가 ( + HTTP )
> 2. Oauth는 무엇인가
> 3. Spring boot를 이용한 Oauth 개발 Flow
> 4. Spring에서 Bean 객체를 관리하는 방법
> 5. Node.js와 Spring의 차이

## 1. 쿠키와 세션은 무엇인가

* 쿠키
쿠키는 사용자가 방문한 웹 사이트 서버가 사용자 컴퓨터에 저장하는 작은 기록 정보 파일을 말한다. 
<br/>
쿠키는 아래와 같은 특징을 갖는다.

1. 쿠키 저장 시, 설정한 만료 시점까지 삭제 불가능하다.
2. 쿠키 정보는 노출되고 수정될 수 있는 위험이 있다.
3. 쿠키는 key, value 형태로 이루어져 있다.
4. 쿠키가 필요한 이유는 HTTP의 특성( connectionless & stateless ) 때문이다.
> HTTP는 상태 정보를 유지하지 않기 때문에 누가 요청을 보냈는지 모른다. 이 상태를 유지하기 위해 등장한 게, 쿠키이다. 

* 세션
세션은 일정 시간동안 같은 사용자로부터 들어오는 일련의 요청을 하나의 상태로 보고, 그 상태를 일정하게 유지시키는 기술이다.
<br/>
세션은 아래와 같은 특징을 갖는다.

1. 세션 정보는 웹 서버에 저장한다.
2. 브라우저 종료 시 삭제되기 때문에 쿠키보다 안전하다.
3. 중요한 정보는 서버에서 관리하고 클라이언트에는 세션 키만 제공한다.
4. 서버에 세션 객체 생성 후, 세션 키를 만들어 속성명으로 사용한다.
5. 속성 값에 정보를 저장하고, 세션 키를 클라이언트에 보낸다.

### HTTP에 대해 알아보자.
HTTP는 Hypertext Transfer Protocol의 약자로, 인터넷 상 통신 규약을 말한다. <br/>

* HTTP는 클라이언트-서버 구조로 이루어져 있다.
* Statless 하다.
> HTTP는 서버가 클라이언트의 상태를 보존하지 않는 구조이다. 서버가 연결되는 모든 클라이언트의 상태를 보존하지 않아도 되기 때문에 여러 작업을 처리하는 데 용이해서 서버 확장성이 높다. 단, 동일한 클라이언트가 추가로 데이터를 전송함으로써 서버에게 자신을 알려야 한다.
* Connectionless
> 서버가 클라이언트의 요청에 대한 응답 처리를 완료한 즉시 클라이언트와의 연결을 끊는다. 이는 불특정 다수의 클라이언트와의 연결을 지속하지 않아도 된다는 점에서 리소스 낭비를 줄일 수 있으나, 동일한 클라이언트에 대한 모든 요청에 대해 매번 새로운 연결을 시도/해제 해아 하므로 이에 대한 오버헤드가 상당하다.
* status
> 1xx : 요청을 받았으며 프로세스를 계속해서 진행한다.
> 2xx : 요청을 받았고, 인식했으며 프로세스를 처리한다. ( 성공적 응답 )
> 3xx : 요청에 대한 추가적인 처리가 필요하다 ( 리다이렉션 )
> 4xx : 요청이 잘못되었거나, 해당 요청을 처리할 수 없음
> 5xx : 요청을 받았으나 서버에서 처리할 수 없음

### TCP에 대해 알아보자.
HTTP는 IP/TCP 기반에서 동작한다. <br/>
TCP는 OSI 7 계층에서 전송 계층에 해당한다. *전송계층*은 application 프로세스들 간의 논리적 통신을 제공한다. 즉, 네트워크 양 끝단에서 통신을 수행하는 당사자 간의 단대단 연결을 제공한다. <br/>
전송계층은 아래와 같은 기능을 제공한다.
* 흐름 제어
* 오류 제어
* 분할과 병합
* 서비스 프리미티브

**TCP** 는 IP 프로토콜 위에서 연결형 서비스를 지원하는 전송계층 프로토콜이며, 데이터 단위는 세그먼트이다.

* 특징
1. 연결형 서비스를 지원한다.
2. 전이중 방식의 양방향 가상 회선을 제공한다.
3. 신뢰성 있는 데이터 전송을 보장한다.

* 3 way hand shake
1. CLIENT -SYN--> SERVER
2. CLINET <--ACK,SYN- SERVER
3. CLIENT -ACK--> SERVER

* 4 way hand-shake
1. CLIENT -SYN, FIN--> SERVER
2. CLIENT <--ACK- SERVER
3. CLIENT <--SYN,FIN- SERVER
4. CLIENT -ACK--> SERVE

## 2. Oauth는 무엇인가
회원에 관한 모든 동작을 타 서비스에 의탁하는 것을 말한다.

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

## 3. Spring boot를 이용한 Oauth 개발 Flow

1. Client -- "localhost:8080/oauth2/authorize/naver?redirect_uri=localhost:8080/api/main"-> Spring
2. Spring ---> Srping Security Oauth2 Client
3. Srping Security Oauth2 Client --Rediret Naver authorization URL -> Client
4. Client -- Enter info -> Authorization Server
5. Authorization Server -- allow with **authorization code** -> Spring Security
6. Spring Security ---> CustomOauth2UserService ( Create or Update User info in DB )
7. CustomOauth2UserService -- Create **access token** with authorization code -> Oauth2AuthenticationSuccessHandler
8. Oauth2AuthenticationSuccessHandler -- Create JWT token and Add header and Redirect localhost:8080/api/main -> Client

* Error Flow
1. Client -- "localhost:8080/oauth2/authorize/naver?redirect_uri=localhost:8080/api/main"-> Spring
2. Spring ---> Srping Security Oauth2 Client
3. Srping Security Oauth2 Client --Rediret Naver authorization URL -> Client
4. Client -- Enter info -> Authorization Server
5. Authorization Server -- deny with **error code** and Callback URL Redirect -> Spring Security
> http://localhost:8080/oauth2/callback/naver
6. Spring Security ----> OAuth2AuthenticationFailureHandler

## 4. Spring에서 Bean 객체를 관리하는 방법
Spring은 Bean 객체를 singleton으로 관리하고, multi-thread 환경에서 동작한다.
### singleton
어플리케이션이 실행될 때, 어떤 클래스가 최초 한 번만 메모리를 할당하고 그 메모리에 인스턴스를 만들어 사용하는 디자인 패턴을 말한다. 인스턴스가 1개만 생성되기에 하나의 인스턴스를 메모리에 등록해서 여러 thread가 동시에 해당 인스턴스를 공유하여 사용할 수 있게끔 할 수 있다. 이는 요청이 많은 곳에서 사용하면 효율을 높일 수 있다.
### thread
Thread는 process 내에서 실제로 작업을 수행하는 주체를 말한다. Thread는 순서와 상관없이 실행되며, Thread를 생성하는 비용은 많이 든다. 
> 왜 많이 들까?
> Thread는 고유의 register와 stack 영역을 갖고 있다. 따라서, Thread의 생성을 위해서는 메모리의 할당이라는 비용이 발생하게 된다. JAVA 11에서는 Thread에 기본적으로 11MB의 메모리를 예약할당한다. Thread를 너무 많이 생성하게 되면, 메모리 할당 비용이 그만큼 증가하게 된다.
Spring에서는 Thread Pool을 사용하여 Thread를 미리 만들어 놓고, 요청 시 사용하고 반납하도록 한다.
### REST API에 빗대어 이해해보자.
우리가 REST API를 개발할 때, 전역에 어떤 변수들을 두고 사용했는지 떠올려보자. <br/>
`@Controller`, `@Service`, `@Repository` 등 어노테이션이 붙은 Bean 객체를 전역 변수로 두고, `DTO`, `VO` 같은 변수들은 지역 변수로 사용하였다. 그 이유가 무엇일까?

하나의 공유자원을 놓고 여러 개의 Thread가 읽기/쓰기를 하면서 데이터 조작 중에 문제가 발생하게 될 수 있다. 이를 Race Condition이라 한다. REST API에서 전역 변수로 DTO를 두었다고 가정해보자. DTO는 값이 계속 변하는 가변 객체로, 이를 여러 Thread가 동시에 사용한다고 하면 ? Race Condition 문제가 발생할 것이다. 이러한 상황을 Thread-safe하지 못하다고 한다. 

JVM에서 각각의 Thread는 고유의 stack 영역을 가지고 있지만, Heap 영역은 Thread들 간에 공유하고 있다. 왜? Process가 메모리 영역 중 code 세그먼트, data 세그먼트, Heap 영역을 갖기 때문이다. Process 안에는 최소 1개의 Thread가 존재해야 하며 여러 개의 Thread들은 Process의 메모리 자원을 공유한다. 

따라서 !! REST API를 개발할 때에는 전역에 Singleton으로 만들어진 Bean 객체인 불변객체를 사용하고, VO와 DTO 같이 값이 자주 변하는 가변 객체는 사용하지 않는다.

## 5. Node.js와 Spring의 차이
Spring은 multi-thread 환경이다. 여러 Client로부터 요청이 들어올 때, 그 요청들이 원활히 처리될 수 있었던 이유가 바로 이 때문이다. 만약, single thread 환경이라면, 하나의 요청이 처리되기 전까지 다른 요청에 대한 처리가 이루어지지 않는다.

그렇다면, Node.js는 어떻게 그 많은 요청을 처리하면서도 Spring과 성능 차이가 많이 나지 않는 걸까?

Node.js도 Multi-thread이지만 사용자가 직접 다룰 수 있는 thread는 한 개뿐이어서 single thread 환경이라고 한다. 데이터 읽고/쓰기 혹은 네트워크 요청과 같은 작업들은 background에서 Event-loop에 의해 Thread pool에서 처리된다. 

기본적으로 Node.js는 non-blocking, event driven 방식이기 때문에 이벤트가 발생했을 때 일어날 일을 미리 정의해둔다. 이벤트가 발생하면, 콜스택에 함수들이 쌓이고, 이를 event loop이 thread pool에 작업 할당한다. thread pool에서 작업이 완료되면, 미리 정의된 callback 함수를 task queue로 옮기고 event loop이 task queue에 있는 callback 함수들을 콜스택으로 올려 처리한다. 이와 같은 흐름으로 동작하기 때문에 single thread이지만 성능면에서 spring과 큰 차이가 없다.  단, 하나의 요청이 너무 많은 작업을 수행해야 하거나 오류가 발생하는 경우, 서버가 다운 될 수 있다.

[여기 블로그 보면 좋아요](https://fbtmdwhd33.tistory.com/256)