# 🍃Spring Security

- 스프링 기반 애플리케이션의 보안(`인증`, `인가` 등)을 담당하는 스프링 하위 프레임워크

## 관련 보안 용어

- `인증`(Authentication) : 보호된 리소스에 접근한 대상에 대해 유저가 누구인지, 유저가 작업을 수행해도 되는 주체인지 확인하는 과정
- `인가`(Authorize) : 인증 이후, 해당 유저가 요청한 리소스에 대해 접근 가능한 권한을 가지고 있는지 확인하는 과정
- `권한` : 모든 리소스는 접근 제어 권한이 걸려있음. 권한은 어떠한 리소스에 대한 접근 제한
- `접근 주체`(Principal) : 보호된 리소스에 접근하는 대상
- `비밀번호`(Credential) : 리소스에 접근하는 대상의 비밀번호

## 특징

<aside>
📄 웹 `인증`과 `인가`를 편하게 구현하기 위한 특징들

</aside>

1. 서블릿 API 통합
2. Spring Web MVC와의 선택적 통합
3. `인증`과 `권한` 부여를 모두 포괄적이고 확장 가능한 지원
4. `세션 고정`, `ClickJacking`, `사이트 간 요청 위조` 등과 같은 공격으로부터 보호
    1. `세션 고정` : 사용자 로그인 시 항상 일정하게 고정된 세션 ID 값을 사용하는 취약점
    2. `ClickJacking` : 사용자가 클릭하고 있다고 인지하는 것과 다른 어떤 것을 클릭하게 속이는 악의적인 기법
    3. `사이트 간 요청 위조` : `csrf`, 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위를 특정 웹사이트에 요청하게 하는 공격 기법


## 요청 처리

![스크린샷 2022-07-02 오후 5.01.45.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/974f0b9f-6789-4abd-b698-9686ff261ed9/스크린샷_2022-07-02_오후_5.01.45.png)

- `서블릿 필터 체인`을 자동으로 구성하고 요청을 거치게 함
- 필요한 `필터`만 구현해서 사용

<aside>
📄 `Servlet` : 자바를 사용하여 웹페이지를 동적으로 생성하는 서버 측 프로그램
`Filter` : 요청과 응답에 대한 정보들을 변경할 수 있게 개발자들에게 제공하는 서블릿 컨테이너
`FilterChain` : `Filter`가 여러 개 모여서 하나의 체인을 형성한 것

</aside>

## 인증 구조(feat. 로그인)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/93da36ab-9b93-4183-b5eb-9eda83346eb3/Untitled.png)

<aside>
📄 Authentication
1. 사용자가 로그인 정보와 함께 인증 요청 `HttpRequest`
2.  `AuthenticationFilter`가 요청을 가로채고, 가로챈 정보를 통해 `UsernamePasswordAuthenticationToken` 객체 생성 (아직 미검증 `Authentication` 상태)
3. ProviderManager에게 `UsernamePasswordAuthenticationToken` 객체 전달
4. `AuthenticationProvider`에 `UsernamePasswordAuthenticationToken` 객체 전달
5. 실제 DB로부터 사용자 인증 정보 가져오는 `UserDetailsService`에 사용자 정보 넘겨줌
6. 넘겨받은 정보를 통해 DB에서 찾은 사용자 정보인 `UserDetails` 객체 생성
7. `AuthenticationProvider`는 `UserDetails`를 넘겨 받고 사용자 정보 비교
8. 인증이 완료되면 사용자 정보를 담은 `Authentication` 객체 반환
9. 최초의 `AuthenticationFilter`에 `Authentication` 객체 반환됨
10. `Authentication` 객체를 `SecurityContext`에 저장

</aside>

- 로그인 방식이 OAuth 2.0 로그인일 경우, `UsernamePasswordAuthenticationFilter` 대신 `Oauth2LoginAuthenticationFilter` 호출

# JWT(JSON WEB TOKEN)

- 유저 인증과 식별에 필요한 정보들을 `Token`에 담아 암호화시켜 사용하는 `토큰`
- 토큰 자체에 **사용자의 권한 정보**나 **서비스를 사용하기 위한 정보**가 포함됨
- 세션과는 달리 **서버가 아닌 클라이언트에 저장**됨
    - 세션 사용시 쿠키 등을 통해 식별하고 서버에 저장
    - `JWT`와 같은 토큰을 클라이언트에 저장하고 요청 시 **`HTTP 헤더`에 `토큰을` 첨부**하는 것만으로도 단순하게 데이터를 요청하고 응답받을 수 있음
        - **무상태(Stateless)** 환경에서 사용자 데이터 주고받을 수 있음
- JSON 데이터를 `Base64 URL -safe Encode` 를 통해 인코딩하여 직렬화함
    - `Base64 URL -safe Encode` : `A–Z, a–z, 0–9, -,_`
      와 같은 64개의 문자를 사용해서 ASCII코드를 문자열(텍스트)로 인코딩하는 방식
- 토큰 내부에 위변조 방지를 위한 개인키를 통한 `전자서명`이 있음
    - 사용자가 JWT를 서버로 전송하면 서버는 서명을 검증하는 과정을 거치고, 검증이 완료되면 요청한 응답을 돌려줌

## JWT 구조

- `Header`, `Payload`, `Signature` 로 구성되며 각 요소는 `.` 으로 구분됨
    - `Header` : `JWT`에서 사용할 `타입`과 `해시 알고리즘`의 종류가 담겨있음
    - `Payload` : 서버에서 첨부한 `사용자 권한 정보`와 `데이터`가 담겨있음
    - `Signature` : `Header`, `Payload`를 encoding한 이후 `Header`에 명시된 `해시함수`를 적용하고, 개인키로 서명한 `전자서명`이 담겨 있음. `전자서명`으로 `Header`, `Payload`가 변조되었는지 확인할 수 있음

      → **신뢰할 수 있는 토큰**으로 사용할 수 있는 근거

- `전자서명`은 비대칭 암호화 알고리즘을 사용하므로 암호화를 위한 키와 복호화를 위한 키가 다름

  → `암호화`(전자서명)에는 `개인키`, `복호화`(검증)에는 `공개키` 사용


## JWT 사용 절차

1. 클라이언트 사용자가 아이디, 패스워드를 통해 웹서비스 `인증`
2. 서버에서 서명된(Signed) `JWT`를 생성하여 클라이언트에 응답
3. 클라이언트가 서버에 데이터를 추가적으로 요구할 때 `JWT`를 `HTTP Header`에 첨부
4. 서버에서 클라이언트로부터 온 `JWT` 검증

# Oauth (OpenID Authentication)

- 타사의 사이트에 대한 접근 권한을 얻고, 그 권한을 이용하여 개발할 수 있도록 도와주는 프레임워크
    - `구글`, `카카오`, `네이버` 등에서 로그인하면 직접 구현한 사이트에서도 로그인 인증을 받을 수 있게 되는 구조
- 사용자 입장에서는 **여러 서비스들을 하나의 계정으로 관리**할 수 있게 되어 편해진다는 장점, 개발자 입장에서는 **민감한 사용자 정보를 다루지 않아 위험 부담이 줄고** 서비스 제공자로부터 **사용자 정보를 활용**할 수 있다는 장점이 있음
- `Access Token`을 발급 받고, 토큰을 기반으로 원하는 기능 구현 가능
    - `Access Token` : 리소스(사용자 정보)에 직접 접근할 수 있도록 해주는 토큰, 유효 기간이 있음
    - `Refresh Token` : 새로운 `Access Token`을 발급받기 위한 정보를 담고 있는 토큰, `Access Token` 보다 유효기간이 훨씬 긺
- `Oauth`에 `JWT` 기반 인증을 도입하면 **비용 절감** + **Stateless 아키텍처** 구성

## 주요 용어

- `Resource Owner` : 개인 정보의 소유자, ex. 유저 me
- `Client` : 제 3의 서비스로부터 인증 받고자하는 서버 ex. 직접 구현한 사이트 s
- `Resource Server` : 개인 정보를 저장하고 있는 서버 ex. 구글, 카카오
- `Client ID` : Resource Server에서 발급해주는 ID ex. s에 구글이 할당한 ID
- `Client Secret` : Resource Server에서 발급해주는 PW ex. s에 구글이 할당한 PW
- `Authorized Redirect Uri` : Client 측에서 등록하는 Url, 이 Uri로부터 인증을 요구하는 것이 아니라면 Resource Server는 해당 요청 무시함

## Kakao Oauth 인증 과정

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8e18da9b-d280-45f1-b1cf-b3b35f36c984/Untitled.png)

## 🍫 Kakaoauth (Live Coding 준비)

[https://loosie.tistory.com/302](https://loosie.tistory.com/302)

요기 참고 !

# ETC.

### Spring이 싱글 스레드일 경우?

최초 사용자가 보낸 요청에 응답하기 전까지 다른 사용자는 해당 서비스를 이용할 수 없음

→ spring이 멀티 쓰레드를 지원하는 이유

- client의 요청이 있을 때마다 스레드를 생성하여 해당 요청을 처리함.
- 요청이 많아질 수록 오버헤드 발생할 확률 증가 -> 스레드풀로 미리 스레드들을 만들어두어 단점 개선 but 얘도 적정량이 아니라면 메모리 낭비의 가능성

### singleton 패턴

특정 클래스의 객체가 오직 하나만 존재하도록 함

## spring은 왜 bean 객체를 싱글톤으로 관리하는가?

- 사용자들의 요청마다 멀티 스레드들이 그 과정마다 필요한 객체를 생성해야 함

→ 성능 저하 및 메모리 낭비 가능성 증가함

- 싱글톤 객체로 생성하여 모든 사용자들의 스레드가 공유할 수 있게 함

## 왜 node.js는 싱글 스레드?

- 작업이 완료되는 대로 결과를 넘겨주는 비동기 방식
- 논블로킹 방식으로 다른 작업이 들어와도 스레드가 멈춰있지 않음
- 갑자기 요청이 많아지거나 복잡한 요청이 들어오면 서버 반응이 느려짐
- 간단한 요구사항이 많을 경우 사용하기 좋음 ex. 채팅, crud 기반 서비스

↔ spring : 복잡한 요구사항이 많을 경우 사용하기 좋음 (실시간 게임, 영상처리 등)

[https://velog.io/@tmdgh0221/Spring-Security-와-OAuth-2.0-와-JWT-의-콜라보](https://velog.io/@tmdgh0221/Spring-Security-%EC%99%80-OAuth-2.0-%EC%99%80-JWT-%EC%9D%98-%EC%BD%9C%EB%9D%BC%EB%B3%B4)

[https://www.bottlehs.com/springboot/스프링-부트-spring-security를-활용한-인증-및-권한부여/](https://www.bottlehs.com/springboot/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-spring-security%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-%EC%9D%B8%EC%A6%9D-%EB%B0%8F-%EA%B6%8C%ED%95%9C%EB%B6%80%EC%97%AC/)

[https://youtu.be/aEk-7RjBKwQ](https://youtu.be/aEk-7RjBKwQ)

[https://brunch.co.kr/@jinyoungchoi95/1](https://brunch.co.kr/@jinyoungchoi95/1)

[https://pronist.dev/143](https://pronist.dev/143)

[https://fbtmdwhd33.tistory.com/m/256](https://fbtmdwhd33.tistory.com/m/256)

[https://haksae.tistory.com/m/45](https://haksae.tistory.com/m/45)