# What is JPA & What is REST

> 목차
> * What is JPA with ORM
> * What is REST
> * What is RestTemplate

## 1. What is JPA with ORM

### JPA
JPA ( Java Persistent API )는 JAVA ORM 기술에 대한 API 표준 명세를 의미한다. 즉, ORM을 사용하기 위한 인터페이스를 모아둔 것이다. JPA를 사용하기 위해서는 JPA를 구현한 Hibernate, EclipseLink, DataNucleus 같은 ORM Framework를 사용해야 한다.

### Adventage of JPA
* 생산성
navite query를 작성하는 반복적인 작업을 JPA가 대신 처리해준다. DB 설계 중심의 패러디임을 객체 설계 중심으로 역전시킬 수 있다.

* 유지보수
entity에 field가 추가될 때마다 대응해야 하는데, 등록/수정/조회 query를 수정할 필요 없이 JPA가 모두 대신 처리해준다. 즉, 유지보수해야 하는 코드 수가 줄어든다.

* 패러다임의 불일치 해결
상속, 연관관계, 객체 그래프 탐색, 비교하기와 같은 패러다임의 불일치 문제를 해결해준다.

* 성능
애플리케이션과 DB 사이에서 다양한 성능 최적화 기회를 제공한다. JPA는 애플리케이션과 DB 사이에서 동작한다.

* 데이터 접근 추상화와 벤더 독립성
RDB는 같은 기능도 벤더마다 사용법이 다르다. 각각 DB 마다 다른 점에 상관없이 기술에 종속되지 않을 수 있다. DB를 변경하게 되면 JPA에게 다른 DB를 사용한다고 알려주기만 하면 된다.

* 표준
JPA는 자바 진영의 ORM 기술 표준이다. 표준을 사용하면 다른 구현 기술로 손쉽게 변경할 수 있다.

### ORM
ORM( Object Relational Mapping )은 객체와 DB의 테이블을 매핑한다는 뜻이다.

**ORM Framework** <br/>
Native query를 직접 작성하지 않고도 데이터 조작이 가능하며, 객체를 자바 컬렉션에 저장하듯이 ORM Framework에 저장하면 쿼리 작성과 동일하게 작업을 수행할 수 있다. <br/>
또한, RDB의 경우 DB간의 매핑 방법만 ORM Framework에 전달해주면 개발자가 직접 SQL 쿼리 작성 없이 ORM Framework가 DB에 데이터를 전달해준다. <br/>
이로써 개발자는 RDB를 사용하더라도 객체지향 애플리케이션 개발에만 집중할 수 있게 되었다. <br/>

> **Hibernate**<br/>
> 오프소스 ORM Framework로, EJB(엔터프라이즈 자바 빈즈)의 ORM 기술과 비교해서 가볍고 실용적인데다 기술 성숙도도 높았다.

### Persistence 영속성
영속성은 데이터를 생선한 프로그램이 종료되더라도 사라지지 않는 데이터 특성을 의미한다. 영속성이 없는 데이터는 단지 메모리에서만 존재하므로, 프로그램 종료 시 모두 사라진다.

자바에서 데이터를 저장하는 방법은 다음과 같다.
* JDBC
> Java DatabaseConnectivity
> Java와 DB를 연결해주는 통로와 같다.
> SQL 문을 실행하기 위한 JAVA API를 제공해준다.
* Spring JDBC
* Persistence Framework (Hibernate, Mybatis ...)
> SQL Mapper  vs. ORM
> SQL Mapper는 SQL과 Object 필드 간의 매핑을 해준다. SQL문을 직접 작성하는 특징이 있다.
> ORM은 Database data와 Object 필드 간의 매핑을 해준다. 객체와 RDB의 데이터를 자동으로 매핑해주는 특징이 있다.

**ORM 장점**<br/>
1. 객체 지향적 코드를 사용함으로써 직관적이고 비즈니스 로직에 집중할 수 있다.
    > CRUD를 위한 SQL문 작성 불필요
    > Domain 별로 코드를 작성하기 때문에 가독성이 높다.
2. 재사용 및 유지보수의 편리성 증가
    > ORM은 독립적으로 작성되어 있고, 해당 객체들은 재사용이 가능하다.
3. DBMS에 대한 종속성이 줄어든다.


**ORM 단점**<br/>
1. 사용 편의성이 있지만, 설계에 신중해야 한다.
2. 복잡도가 높아질수록 난이도가 높아진다.
3. 경우에 따라 속도 저하 발생


[쉽게 풀어서 설명해준 블로그](https://devfunny.tistory.com/422)
[영속성 개념도 같이 소개해준 블로그](https://azderica.github.io/00-db-orm/)

## 2. What is REST

**REST**<br/>
자원을 이름으로 구분하여 자원의 상태를 주고 받는 모든 것을 지칭한다.

**구성요소**<br/>
1. URI
2. Http method
3. Response

**사용이유**<br/>
Client에 제약을 두지 않기 위해서이다. 
> 모바일, pc 등 다양한 방식의 접근과 통신의 형식으로부터 오는 제약

### REST API
HTTP 프로토콜을 통해 API를 설계하기 위한 아키텍처이다. <br/>
HTTP 요청을 통해 통신하여 데이터 생성, 읽기, 업데이트 및 삭제 기능을 처리한다. ( CRUD라고도 부른다. )

* 원칙
1. 클라이언트-서버 구조
2. Uniform Interface
> 하나의 URL로 한 개의 데이터만 가져와야 한다.
3. Stateless
> 요청들은 각각 독립적으로 처리되어야 한다.
4. Cacheable
> 요청을 통해 보내는 자원들은 캐싱이 가능해야 한다.
5. Layered System
> 여러 개의 레이어를 거쳐서 요청을 처리한다. ( 3 tier )
6. Code on demand
> 서버에서 코드를 클라이언트로 보내서 실행할 수 있어야 한다.

[여기 정리 잘되어 있음](https://jaeseongdev.github.io/development/2021/06/15/REST%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EC%9B%90%EC%B9%99-6%EA%B0%80%EC%A7%80/)

## 3. What is RestTemplate

RestTemplate는 Spring에서 제공하는 HTTP Client로 REST API를 호출하기 위한 함수를 제공하는 클래스이다. API Endpoint 호출을 간단하게 처리해준다.
* Spring 4.x 부터 지원한다.
* HHTP 요청 후 JSON, XML, String 과 같은 응답을 받을 수 있는 템플릿
* RESTful 형식을 지원
* 반복적 코드 줄여줌
* Blocking I/O 기반의 Synchronous API <> Nonblocking은 WebClient사용

### RestTemplate 동작 Flow
1. Application 내부에서 REST API에 요청하기 위해 RestTemplate의 메소드를 호출한다.
> [Application] --- call ---> [RestTemplate]
2. RestTemplate는 HttpMessageConverter를 이용해 JAVA Object를 RequestBody에 담을 message(JSON)로 변환한다. 
> [RestTemplate] --- convert(Object <> messgae)--> [HttpMessageConverter]
3. ClientHttpRequestFactory에서 ClientHttpRequest를 받아와 Request를 전달한다.
> [RestTemplate] <---bring(Request)--- [ClientHttpRequestFactory]
4. ClientHttpRequest가 실질적으로 HTTP 통신으로 Request를 수행한다.
[ClientHttpRequest] --- Http Mehtod (with Request) --> [REST API]
5. RestTemplate이 Error Handling을 한다.
> [RestTemplate이] --- throw Exception---> [ResponseErrorHandler]
6. ClientHttpResponse에서 Response를 가져와 오류가 있으면 처리한다.
> [ClientHttpResponse] --- throw Exception ---> [ResponseErrorHandler]
7. MessageConverter를 이용해 ResponseBody의 message를 JAVA Object로 변환한다.
> [RestTemplate] --- convert(Object <> messgae)--> [HttpMessageConverter]
8. 결과를 Application에 돌려준다.
> [Application] <--- bring back result --- [RestTemplate]

RestTemaplate이 통신 과정을 ClientHttpRequestFactory에 위임하여 처리한다. 즉, RestTemplate은 HTTP Client의 일종이지만 실제 커넥션을 만들기 위해 내부에서는 URIConnection이나 apache가 제공하는 HttpClient와 같은 걸 사용한다.

RestTemplate은 자동으로 Bean 등록되지 않기 때문에 직접 등록해줘야 한다. 
> @Configuration으로 클래스 만들기

**MessageConverter** <br/>
JAVA Object와 Message 간의 변환 시 사용한다. ( body에 담기는 데이터를 JSON 형태로 바꾸는 거라고 이해하면 된다. ) 이를 위해 사용하는 게, **Content-type*이다.

[내용이 알차다](https://e2e2e2.tistory.com/15)