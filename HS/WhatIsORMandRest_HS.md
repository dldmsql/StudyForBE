# What Is ORM and REST
## 목차
1. ORM
2. JPA
3. REST
4. REST API 
5. RestTemplate

## 1. ORM
- ORM(Object Relational Mapping)은 말 그대로 객체-관계 매핑
- 객체지향프로그래밍(OOP)에서 쓰이는 객체(클래스)와 관계형 데이터베이스(RDB)에서 쓰이는 테이블을 자동으로 연결(매핑)하는 것을 의미
> 여기서 매핑은 영속성 부여를 의미한다. 영속성(Persistence)은 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성을 말한다. 
- 객체 간의 관계를 바탕으로 SQL 문을 자동으로 생성하여 객체(클래스)-테이블 간의 불일치를 해결
> 클래스와 테이블은 서로가 기존부터 호환가능성을 두고 만들어진 것이 아니기 때문에 불일치 발생
- ORM을 이용하면 따로 SQL 문을 짤 필요 없이 객체를 통해 간접적으로 데이터베이스를 조작할 수 있음

### ORM의 장점
1. 완벽한 객체지향 코드
비즈니스 로직에 더 집중할 수 있게 도와준다.
2. 재사용, 유지보수, 리팩토링 용이성
ORM은 독립적으로 작성되어 있고, 해당 객체들을 재사용할 수 있다. 이로 인해 모델에서 가공된 데이터를 컨트롤러에 의해 뷰와 합쳐지는 형태로 디자인 패턴을 견고하게 다지는데 유리하다. 또한, 매핑 정보가 명확하여 ERD를 보는 것에 대한 의존도를 낮출 수 있다.
3. DBMS(Database Management System) 종속성 하락
객체 간의 관계를 바탕으로 SQL문을 자동으로 생성하고, 객체의 자료형 타입까지 사용할 수 있기 때문에 RDBMS의 데이터 구조와 객체지향 모델 사이의 간격을 좁힐 수 있다.

### ORM의 단점
1. ORM이 모든 것을 해결해줄 수 없음
프로젝트의 복잡성이 커질 수록 난이도도 올라가고 부족한 설계로 잘못 구현되었을 경우 속도 저하 및 일관성을 무너뜨리는 문제점이 생길 수 있으므로 신중하게 설계해야 한다. 

### 객체-관계 간의 불일치가 생기는 특성
1. 세분성(Granularity)
데이터베이스에 있는 테이블 수보다 더 많은 클래스를 가진 모델이 생길 수 있다.
2. 상속성(Inheritance)
RDBMS에는 상속 개념이 없다.
3. 일치(Identity) 
RDBMS는 기본키를 이용하여 동일성을 정의한다. 자바는 객체 식별(a==b)와 객체 동일성(a.equals(b))을 모두 정의한다. 
4. 연관성(Associations) 
객체지향 언어는 방향성이 있는 객체의 참조(reference)를 사용하여 연관성을 나타내지만 RDBMS는 방향성이 없는 외래키를 이용해서 나타낸다.
5. 탐색(Navigation) 
자바와 RDBMS에서 객체를 접근 하는 방법이 다르다. 자바는 그래프 형태로 하나의 연결에서 다른 연결로 이동하며 탐색한다. RDBMS에서는 일반적으로 SQL문을 최소화하고 JOIN을 통해 여러 엔티티를 로드하고 원하는 대상 엔티티를 선택하는 방식으로 탐색한다.

### ORM 프레임워크
1. JPA/Hibernate
JPA(Java Persistence API)는 자바 ORM 기술 표준으로 인터페이스의 모음이다. 이것을 구현한 구현체 중 하나가 Hibernate이다.
2. Sequeilze
Postgres, MySQL, MariaDB, SQLite 등을 지원하는 Promise에 기반한 비동기로 동작하는 Node.js ORM이다.
3. Django ORM
Python 기반 프레임워크인 Django에서 자체적으로 지원하는 ORM

## 2. JPA(Java Persistence API)
- Java 진영에서 ORM 기술 표준으로 사용하는 인터페이스 모음
- 인터페이스이기 때문에 Hibernate, OpenJPA 등이 JPA를 구현한다.
- JPA는 애플리케이션과 JDBC 사이에서 동작한다. JPA 내부에서 JDBC API를 사용하여 SQL을 호출하여 DB와 통신한다. 
- 개발자가 ORM 프레임워크에 저장하면 적절한 INSERT SQL을 생성해 데이터베이스에 저장해주고, 검색을 하면 적절한 SELECT SQL을 생성해 결과를 객체에 매핑하고 전달해준다.

### JPA 사용 이유
1. 생산성
지루하고 반복적인 코드를 개발자가 직접 작성하지 않아도 되며, 데이터베이스 설계 중심을 객체 설계 중심으로 변경할 수 있다.
2. 유지 보수
필드를 하나만 추가해도 관련된 SQL과 JDBC 코드를 전부 수행해야 했지만 JPA는 이를 대신 처리해준다.
3. 패러다임의 불일치 해결
JPA는 연관된 객체를 사용하는 시점에 SQL을 전달할 수 있고, 같은 트랜잭션 내에서 조회할 때 동일성도 보장한다.
4. 성능
같은 트랜잭션 안에서는 같은 엔티티를 반환하기 때문에 데이터베이스와의 통신 횟수를 줄일 수 있다. 트랜잭션을 commit 하기 전까지 메모리에 쌓고 한번에 SQL을 전송한다.
5. 데이터 접근 추상화와 벤더 독립성
JPA는 애플리케이션과 데이터베이스 사이에서 추상화된 데이터 접근을 제공하기 때문에 종속이 되지 않도록 한다. 만약 DB를 변경한다면 JPA에게 알려주어 간단하게 변경할 수 있다. 

### 3. REST
REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미한다. 
> 자원: 이름을 지닐 수 있는 모든 정보(개념적인 대상), 일종의 객체(상태는 변화 가능, 변하지 않는 식별자 필요)

즉, REST란
1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시(식별자로 사용)하고
2. HTTP Method(POST, GET, PUT, DELETE)를 통해 
3. 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미

> CRUD Operation
CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말/ Rest에서는 POST, GET, PUT, DELETE가 있다.

### REST의 구성 요소
1. 자원(Resource) : HTTP URI
2. 자원에 대한 행위(Verb) : HTTP Method
3. 자원에 대한 행위의 내용(Representations) : HTTP Message Pay Load

### REST의 특징
1. Server-Client
2. Stateless
3. Cacheable
4. Layered System
5. Uniform Interface

### REST의 장단점
__장점__
1. HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
2. HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해준다.
3. HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
4. Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
5. REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
6. 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
7. 서버와 클라이언트의 역할을 명확하게 분리한다.

__단점__
1. 표준 자체가 존재하지 않아 정의가 필요하다.
2. 사용할 수 있는 메소드가 4가지밖에 없다.
3. HTTP Method 형태가 제한적이다.
4. 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
5. 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다. 

### 4. REST API
REST API란 REST의 원리를 따르는 API를 의미한다.
> API: Application Programming Interface, 요청과 응답을 사용하여 두 애플리케이션이 서로 통신하는 방법을 정의한다. 

__REST API 설계 규칙__
1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용해야 한다.
2. 마지막에 슬래시(/)를 포함하지 않는다.
3. 언더바(_) 대신 하이픈(-)을 사용한다.
4. 파일 확장자는 URI에 포함하지 않는다.
5. 행위를 포함하지 않는다. EX) delete-post

### RESTful
REST의 원리를 따르는 시스템을 의미, REST API의 설계 규칙을 올바르게 지킨 시스템은 RESTful하다 말할 수 있으며 모든 CRUD 기능을 POST로 처리하는 API 혹은 URI 규칙을 올바르게 지키지 않은 API는 RESTful하지 못한 시스템이라고 할 수 있다.

## 5. RestTemplate
Spring 3.0부터 지원하는 템플릿으로 Spring에서 HTTP 통신을 RESTful 형식에 맞게 손쉬운 사용을 제공해주는 템플릿이다. Rest API 서비스를 요청 후 응답 받을 수 있도록 설계되었으며 HTTP 프로토콜의 메소드(ex. GET, POST, DELETE, PUT)들에 적합한 여러 메소드들을 제공한다.
> 스프링 5부터는 WebClient(비동기 방식) 사용

### RestTemplate 특징
1. Spring 3.0 부터 지원하는 Spring의 HTTP 통신 템플릿
2. HTTP 요청 후 JSON, XML, String과 같은 응답을 받을 수 있는 템플릿
3. Blocking I/O 기반의 동기 방식을 사용하는 템플릿
> 동기 방식: REST API 호출 이후 응답을 받을 때까지 기다리는 방식
4. RESTful 형식에 맞춰진 템플릿
5. Header, Content-Type 등을 설정하여 외부 API 호출
6. Server to Server 통신에 사용

### RestTemplate 동작 원리
1. 애플리케이션 내부에서 REST API에 요청하기 위해 RestTemplate 메서드를 호출한다.
2. RestTemplate은 MessageConverter를 이용해 java object를 request body에 담을 메시지(JSON 등)로 변환한다.
3. ClientHttpRequestFactory에서 ClientHttpRequest를 받아와 요청을 전달한다.
> ClinetHttpRequestFactory의 실체는 HttpURLConnection과 같은 HTTP Client이다.
4. 실질적으로 ClientHttpRequest가 HTTP 통신으로 요청을 수행한다.
5. RestTemplate이 에러핸들링을 한다.
6. ClientHttpResponse에서 응답 데이터를 가져와 오류가 있으면 처리한다.
7. MessageConverter를 이용해 response body의 message를 java object로 변환한다.
8. 결과를 애플리케이션에 전달한다.

### RestTemplate 사용 방법
1. 결과값을 담을 객체 생성
2. 타임아웃 설정 시 HttpComponentsclientHttpRequestFactory 객체를 생성
3. RestTemplate 객체 생성
4. header 설정을 위해 HttpHeader 클래스를 생성한 수 HttpEntity 객체에 넣어줌
5. 요청 URL을 정의
6. exchange() 메소드로 api 호출
7. 요청한 결과를 HashMap에 추가

## 참고
- !(https://geonlee.tistory.com/207)
- !(https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80)
- !(https://www.youtube.com/watch?v=Nxi8Ur89Akw)
- !(https://girawhale.tistory.com/119)
- !(https://blog.naver.com/hj_kim97/222295259904)

