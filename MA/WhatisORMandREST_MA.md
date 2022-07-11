# What is ORM?

### ORM이란?
>Object-Relational Mapping

- ORM이란 객체지향 프로그래밍의 객체(Object)와 관계형 데이터베이스의 데이터를  매핑해주는 것
     - ORM은 객체지향 프로그래밍에서 사용할 수 있는 가상의 객체지향 데이터베이스를 만들어 프로그래밍 코드와 데이터 연결
    - 객체 지향 프로그래밍은 클래스를 관계형 데이터베이스는 테이블을 하여 객체 모델과 관계형 모델 간에 불일치 존재 </br>
-> ORM을 통해 객체 간의 관계를 바탕으로 SQL을 자동으로 생성하여 불일치를 해결

- 객체를 통해 간접적으로 데이터베이스 데이터를 다룬다.

- Persistant API라고도 부른다.

-> ORM으로 생성된 가상의 객체지향 데이터베이스는 프로그래밍 코드 또는 데이터베이스와 독립적이라 재사용, 유지보수 용이

-> SQL 코드를 직접입력하지 않고 선언문, 할당, 종료 등 부수적인 코드가 생략되기 때문에 직관적이고 간단하게 데이터 조작 가능

-> 완벽한 ORM으로만 서비스를 구현하기 어려움

-> 사용하기는 편하지만 설계는 매우 신중하게 해야한다. 
프로젝트의 복잡성이 커질 경우 난이도 또한 올라갈 수 있다.
잘못 구현된 경우에 속도 저하 및 심각할 경우 일관성이 무너지는 문제점이 생길수 있다.


### ORM 프레임워크
> ORM을 구현하기 위한 구조나 구현을 위해 필요한 여러 기능들을 제공하는 소프트웨어 의미
- JAVA : JPA, Hibernate, EclipseLink,,,
- Python : Diango,,,

### Persistence(영속성) 이란?
- 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성
- 영속성을 갖지 않는 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 모두 잃어버리게 된다.

##### Object Persistence(영구적인 객체)
- 메모리 상의 데이터를 파일 시스템, 관계형 데이터베이스 혹은 객체 데이터베이스 등을 활용하여 영구적으로 저장하여 영속성을 부여

- 데이터를 데이터베이스에 저장하는 3가지 방법
    1. JDBC (Java에서 사용)
    2. Spring JDBC (ex. JdbcTemplate)
    3. Persistence Framework (Ex. Hibernate, MyBatis)

### JPA(Java Persistence API)란?
- Java 진영에서 ORM 기술 표준으로 사용하는 인터페이스 모음
- 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스
- 인터페이스이기 때문에 Hibernate, OpenJPA 등이 JPA를 구현함

### JPA 장점

1. 생산성 </br>
반복적인 코드를 개발자가 직접 작성하지 않아도 되며, DDL문도 자동으로 생성해주기 때문에 데이터베이스 설계 중심을 객체 설계 중심으로 변경 가능

2. 유지보수  </br>
필드를 하나만 추가해도 관련된 SQL과 JDBC 코드를 전부 수정해야 했지만 JPA가 이를 대신 처리해주기 때문에 개발자가 유지보수해야하는 코드가 줄어든다.

3. 패러다임의 불일치 해결 </br>
JPA는 연관된 객체를 사용하는 시점에 SQL을 전달할 수 있고, 같은 트랜잭션 내에서 조회할 때 동일성도 보장하기 때문에 다양한 패러다임의 불일치를 해결

4. 성능 </br>
애플리케이션과 데이터베이스 사이에서 성능 최적화 기회를 제공
같은 트랜잭션안에서는 같은 엔티티를 반환하기 때문에 데이터 베이스와의 통신 횟수를 줄일 수 있다.  </br>
또한, 트랜잭션을 commit하기 전까지 메모리에 쌓고 한번에 SQL을 전송한다.

5. 데이터 접근 추상화와 벤더 독립성 </br>
RDB는 같은 기능이라도 벤더마다 사용법이 다르기 때문에 처음 선택한 데이터베이스에 종속되고 변경이 어렵다.  </br>JPA는 애플리케이션과 데이터베이스 사이에서 추상화된 데이터 접근을 제공하기 때문에 종속이 되지 않도록한다.

# What is REST?
### REST란?
>REST란 Representational State Transfer의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미

-> REST란 
HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
HTTP Method(POST, GET, PUT, DELETE)를 통해 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.

### REST 구성 요소
- 자원(Resource) : HTTP URI
- 자원에 대한 행위(Verb) : HTTP Method
- 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load

### REST의 특징
- Server-Client(서버-클라이언트 구조)
- Stateless(무상태)
</BR> : 서버에 세션 상태가 없는것
- Cacheable(캐시 처리 가능)
- Layered System(계층화)
- Uniform Interface(인터페이스 일관성)

### REST API란?
> REST API란 REST의 원리를 따르는 API를 의미
- API(Application Programming Interface)란
데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환가능 하도록 하는 것

### REST API 설계 원칙
1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용
2. 마지막에 슬래시 (/) 포함X
3. 언더바 대신 하이폰을 사용
4. 파일확장자 포함X
5. 행위 포함X

### RESTFUL이란?
> RESTFUL이란 REST의 원리를 따르는 시스템을 의미

REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있다.

### RestTemplate이란?
>Spring에서 제공하는 HTTP Client로 REST API를 호출을 위한 함수를 제공하는 클래스.
API endpont 호출을 간단하게 처리해준다.

- Spring 4.x 부터 지원하는 Spring의 HTTP 통신 템플릿
- HTTP 요청 후 Json, xml, String 과 같은 응답을 받을 수 있는 템플릿
- RESTful 형식을 지원
- 반복적인 코드를 줄여줌
- Blocking I/O 기반의 Synchronous API (비동기 방식은 WebClient 사용)

### RestTemplate의 동작 과정
1. 애플리케이션 내부에서 REST API에 요청하기 위해 RestTemplate의 메서드를 호출한다.
2. RestTemplate은 MessageConverter를 이용해 java object를 request body에 담을 message(JSON,,,)로 변환한다. (메시지 형태는 상황에 따라 다름)
3. ClientHttpRequestFactory에서 ClientHttpRequest을 받아와 요청을 전달한다.
4. 실질적으로 ClientHttpRequest가 HTTP 통신으로 요청을 수행한다.
5. RestTemplate이 에러핸들링을 한다.
6. ClientHttpResponse에서 응답 데이터를 가져와 오류가 있으면 처리한다.
7. MessageConverter를 이용해 response body의 message를 java object로 변환한다.
8. 결과를 애플리케이션에 돌려준다.
 
## 참고자료
https://dbjh.tistory.com/77 </br>
https://velog.io/@dnjscksdn98/Database-ORM%EC%9D%B4%EB%9E%80 </br>
https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80 </br>
https://velog.io/@kho5420/Web-API-%EA%B7%B8%EB%A6%AC%EA%B3%A0-EndPoint </br>
https://e2e2e2.tistory.com/15 