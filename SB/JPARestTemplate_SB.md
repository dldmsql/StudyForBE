# JPA

## ORM(Object-Relational Mapping)

- 애플리케이션 class와 RDB(Relational Database)의 테이블을 매핑한다는 뜻
- 애플리케이션 객체를 RDB 테이블에 자동으로 영속화
- SQL문이 아닌 Method를 통해 DB 조작 가능 → 비즈니스 로직 구성에만 집중
- 객체지향적인 코드 작성 가능 → 생산성 증가
- 복잡하고 무거운 Query는 SQL문을 사용해야 할 수도 있음

## JPA(Java Persistence API)

- ORM 기술 표준으로 사용하는 인터페이스 모음
- Java에서 RDB 사용방식을 정의한 인터페이스
- Hibernate, OpenJPA 등으로 구현함
- 반복적인 CRUD SQL 처리 가능



# REST

## HTTP(Hypertext Transfer Protocol)

- 웹 상에서 브라우저와 서버가 데이터를 주고 받을 때 사용하는 프로토콜
- 클라이언트의 요청과 서버의 응답으로 이루어짐
- HTML 문서 뿐만 아니라 단순 텍스트나 이미지, 오디오 등의 미디어 데이터 전송
- `Connectionless`(비연결성) : 한 가지 요청에 대한 응답을 받으면 연결 해제
- `Stateless`(무상태) : 서버는 클라이언트 식별X, 요청은 독립적임

### HTTP Method

| 이름 | 역할 |
| --- | --- |
| GET | 서버에게 데이터를 달라는 요청(열람)할 때 사용 |
| HEAD | GET과 같지만 서버가 응답할 때 Body 없이 Header만 리턴 |
| POST | 서버에게 데이터를 전송하는 요청할 때 사용 |
| PUT | 서버에서 요청 URI의 데이터를 수정하거나 새로 추가하도록 요청할 때 사용 |
| PATCH | 서버의 데이터를 일부 수정할 때 사용 |
| DELETE | 서버에서 요청 URI​​의 데이터를 삭제하도록 요청할 때 사용 |
| TRACE | 클라이언트로부터 수신한 요청을 응답에 포함시켜서 전달(디버깅용) |
| OPTIONS | 서버에서 특정 데이터가 어떤 Method를 지원하는지 알아볼 때 사용 |

### HTTP Status code

- 서버가 응답을 전송할 때 같이 전송하는 코드

### 1XX - 정보 응답

- 100 Continue : 요청 진행중, 문제 X

### 2XX - 성공 응답

- 200 OK : 요청 성공적으로 완료
- 201 Created : 요청 성공적 완료, 새로운 리소스 생성 - POST, PUT

### 3XX - 리다이렉션 메시지

- 300 Multiple Choice : 요청에 대해 하나 이상의 응답 가능
- 301 Moved Permanetly : 요청한 리소스의 URI 변경

### 4XX - 클라이언트 에러 응답

- 400 Bad Request : 잘못된 문법으로 서버가 요청 이해 X
- 401 Unauthorized : 요청 보낸 클라이언트 인증 X
- 403 Forbidden : 요청 보낸 클라이언트가 리소스에 접근할 권리 X
- 404 Not Found : 서버가 요청 받은 리소스 찾을 수 없음
- 408 Request Timeout : 요청 중 시간 초과

### 5XX - 서버 에러 응답

- 500 Internal Server Error : 서버에 문제가 있지만 처리 방법 모름
- 502 Bad Gateway : 서버가 게이트웨이로부터 잘못된 응답 받음
- 503 Service Temporarily Unavailable : 일시적으로 서버 사용 불가
    - 유지보수로 인한 서버 중단 또는 과부하
- 504 Gateway Timeout : 서버가 게이트웨이 역할, 다른 서버로부터 적시에 응답 받지 못함

## REST(Representational State Transfer)

- 애플리케이션 개발 아키텍처
- 웹 애플리케이션 상에 존재하는 모든 리소스에 대해 고유 URI 부여함
- HTTP Method를 이용하여 리소스에 대해 CRUD 명령 적용
- URI가 부여된 리소스의 상태를 응답(JSON, XML 등)으로 전송함

## REST 구성 요소

1. 리소스(Resource)
    - 서버에 존재하는 데이터의 총칭
    - 모든 리소스는 고유의 URI를 가짐
    - ex. /resource/1
2. 행위(Verb)
    - 클라이언트가 HTTP Method를 이용하여 자원을 조작하는 것
3. 표현(Representation)
    - 클라이언트의 행위에 대한 서버의 응답


## REST 특징

- 서버-클라이언트 구조(Server-Client Architecture)
    - 서버는 API 제공, 클라이언트는 유저에 대한 처리
    - 서버와 클라이언트의 명확한 역할 구분 가능
- 무상태성(Stateless)
    - 각각의 요청에 대한 정보 저장 X 별개의 요청으로 처리

      → 구현 쉽고 서버 부담⬇️

- 캐시 가능(Cacheable)
    - 캐시 기능을 이용해 같은 URI에 대한 반복된 요청을 효율적으로 처리 가능
- 일관된 인터페이스(Uniform Interface)
    - 플랫폼에 독립적
    - 리소스 타입에 상관 없이 같은 형태의 요청으로 처리
- 자체적인 표현 구조(Self-Descriptiveness)
    - JSON, XML 등을 이용하는 메세지 구조로 해당 메세지가 무엇을, 어떤 행위를 의미하는지 직관적으로 이해 가능
- 계층 구조(Layered System)
    - 클라이언트와 서버 통신 사이에 보안이나 로드 밸런싱 등을 위한 중간 계층 추가 가능

## REST API(=RESTful API)

- REST의 규칙을 지키면서 만든 API

## REST Template

- Spring 3.0부터 지원, REST API 호출 이후 응답을 받을 때까지 기다리는 동기 방식
- 스프링에서 제공하는 HTTP 통신에 유용하게 쓸 수 있는 템플릿
- HTTP 서버와의 통신을 단순화하고 RESTful 원칙을 지킴

### RestTemplate Method

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ba39b3f4-2301-4850-ab94-69050d030e63/Untitled.png)

### RestTemplate 동작 원리

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a20a9012-788c-4a78-9fe2-e18266fd6c18/Untitled.png)

1. 애플리케이션이 `RestTemplate`을 생성하고 URI, HTTP method 등의 헤더를 담아 요청함
2. `RestTemplate`는 `HttpMessageConverter`를 사용하여 `requestEntity`를 요청 메세지로 변환함
3. `RestTemplate`는 `ClientHttpRequestFactory`로 부터 `ClientHttpRequest`를 가져와서 요청을 보냄
4. `ClientHttpRequest`는 요청메세지를 만들어 `HTTP 프로토콜`을 통해 서버와 통신함
5. `RestTemplate`는 `ResponseErrorHandler`로 오류를 확인하고 있다면 처리로직을 태움
6. `ResponseErrorHandler`는 오류가 있다면 `ClientHttpResponse`에서 응답데이터를 가져와서 처리
7. `RestTemplate`는 `HttpMessageConverter`를 이용해서 응답메세지를 java object(Class responseType) 로 변환
8. 애플리케이션에 반환

[https://dbjh.tistory.com/77](https://dbjh.tistory.com/77)

[https://tibetsandfox.tistory.com/18](https://tibetsandfox.tistory.com/18)

[https://tibetsandfox.tistory.com/19](https://tibetsandfox.tistory.com/19)

[https://velog.io/@soosungp33/스프링-RestTemplate-정리요청-함](https://velog.io/@soosungp33/%EC%8A%A4%ED%94%84%EB%A7%81-RestTemplate-%EC%A0%95%EB%A6%AC%EC%9A%94%EC%B2%AD-%ED%95%A8)