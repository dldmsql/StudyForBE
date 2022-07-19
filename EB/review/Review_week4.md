# Review of Studying week 4

> 목차
> * What is difference RDB vs NoSQL
> * What is Drity checking
> * What is Spring Bean

## 1. What is difference RDB vs NoSQL

1. 데이터 저장

RDB는 SQL을 이용해서 저장한다.

NoSQL은 다양한 방식 ( Key-value, document, graph, ... )

2. 스키마

RDB에서 SQL을 사용하기 위해서는 고정된 형식의 스키마가 필요하다. 즉, 스키마는 테이블의 형식과 테이블 간의 관계를 미리 정의한 것이다.

스키마는 변경 가능하지만, 그에 따른 side-effect가 클 수 있다.

NoSQL은 동적으로 스키마의 형태를 관리할 수 있다. 데이터를 추가할 때 즉시 새로운 컬럼을 추가할 수 있고, 개별 속성에 대해서 모든 컬럼에 대한 데이터를 반드시 입력하지 않아도 된다.

3. 쿼리

RDB는 테이블의 형식과 테이블 간의 관계를 맞춰 데이터를 요청해야 한다. 그래서 SQL을 사용한다.

NoSQL은 SQL 없이 데이터 요청이 가능하다.

4. 확장성

RDB는 수직적으로 확장한다.

NoSQL은 수평적으로 확장한다.

## 2. Case by Case when to use

1. RDB

* DB의 ACID 성질을 준수해야 하는 경우

> ACID란? <br/>
> 워자성 -> 모든 작업이 반영되거나 모두 롤백되는 특성 <br/>
> 일관성 -> 데이터는 미리 정의된 규칙에서만 수정이 가능한 특성이다. type 준수 <br/>
> 고립성 -> 2개의 트랜잭션이 실행되고 있을 때, 한 곳에서의 작업들이 다른 곳에서 보여지는 정도이다. <br/>
> 영구성 -> 한 번 커밋된 트랜잭션의 내용은 영원히 적용된다.

[ACID란](https://chrisjune-13837.medium.com/db-transaction-%EA%B3%BC-acid%EB%9E%80-45a785403f9e)

* 소프트웨어에 사용되는 데이터가 구조적이고 일관적인 경우

2. NoSQL

* 데이터의 구조가 거의 또는 전혀 없는 대용량의 데이터를 저장하는 경우

* 클라우드 컴퓨팅 및 저장 공간을 최대한 활용하는 경우

* 빠르게 서비스를 구축하는 과정에서 데이터 구조를 자주 업데이트 하는 경우

## 3. What is Drity Checking

코드의 상황을 빗대어 이해하자.

트랜잭션이 시작되고, 엔티티를 조회하고, 엔티티의 값을 변경하고, 트랜잭션을 커밋한다.
> 트랜잭션은 여러 작업을 하나로 묶은 단위이다.

이 과정에서 DB에 update하는 쿼리는 존재하지 않는다.

위의 로직을 테스트 코드로 작성하여 실행하면, 콘솔창에 update 쿼리가 실행됨을 확인할 수 있다.

그 이유는 **dirty checking** 때문이다.

Dirty는 상태가 변함을 의미하고 checking은 말 그대로 검사이다. 따라서 상태 변경 검사로 이해하면 된다.

JPA에서는 엔티티를 조회하면 해당 엔티티의 조회 상태 그대로 스냅샷을 만들어둔다. 

그리고 트랜잭션이 끝나는 시점에는 이 스냅샷과 비교해서 다른 점이 있다면 update 쿼리를 날린다.

dirty checking의 대상은 영속성 컨텍스트가 관리하는 엔티티에만 적용된다.

## 4. What is Persistence context

영속성 컨텍스트는 엔티티를 영구 저장하는 환경이라는 뜻이다. 

애플리케이션과 DB 사이에서 객체를 보관하는 가상의 DB 같은 역할을 한다. 

서비스별로 하나의 EntityManager Factory가 존재하며, DB에 접근하는 트랜잭션이 생길 때마다 쓰레드 별로 Entity Manager를 생성하여 영속성 컨텍스트에 접근한다.

EntityManager에 엔티티를 저장하거나 조회하면, EntityManager는 영속성 컨텍스트에 엔티티를 보관하고 관리한다.

spring에서는 서비스 별 entitymanager 들이 동일한 영속성 컨텍스트를 바라보는 구조이다.

* EntityManager

영속성 컨텍스트 내에서 엔티티를 관리한다.

JPA에서 제공하는 인터페이스로 스프링 빈으로 등록되어 있어 Autowired 어노테이션을 이용해 사용할 수 있다.

Query 함수와 JPA Repository는 직접적으로 EntityManager를 사용하지 않도록 한 번 더 감싸준 것이다.

JPA에서 제공하지 않는 기능을 사용하거나 커스터마이징을 해야 한다면 EntityManager를 직접 받아서 처리하면 된다.

EntityManager는 Entity Cache를 갖고 있다.

* Entity 생명주기

1. 비영속 ( new / transient )

영속성 컨텍스트와 전혀 관계가 없는 상태이다.

엔티티 객체를 생성하였지만, 아직 영속성 컨텍스트에 저장하지 않은 상태를 의미한다.

```` bash

Post post = new Post();

````

2. 영속 ( managed )

영속성 컨텍스트에 저장된 상태이다.

영속 상태여도 바로 DB에 저장되지 않고 트랜잭션의 COMMIT 시점에 영속성 컨텍스트에 있는 정보들을 DB에 쿼리로 날린다.

```` bash

@Autowired
private EntityManager entityManager;

Post post = new Post(); // post는 비영속 상태

entityManager.persist(post); // post는 영속 상태

````

3. 준영속 ( detached )

영속성 컨텍스트에 저장되었다가 분리된 상태

엔티티를 준영속 상태로 만들기 위해서는 entityManager.detach()를 호출하면 된다.

```` bash

entityManager.detach(post); // 영속 -> 준영속

entityManager.clear(); // 관리되던 엔티티들이 모두 준영속 상태로 변경된다.

entityManager.close(); // 관리되던 엔티티들이 모두 준영속 태로 변경된다.

entityManager.merge(post); // 준영속 -> 영속

````

1차 캐시, 쓰기 지연, 변경 감지,지연 로딩을 포함한 영속성 컨텍스트가 제공하는 어떠한 기능도 동작하지 않는다.

식별자 값을 갖고 있는다.

4. 삭제 ( removed )

영속성 컨텍스트와 DB에서 해당 엔티티를 삭제한 상태이다.

```` bash

entityManager.remove(post);

````

### 영속성 컨텍스트 특징

* 1차 Cache

save 메소드와 같이 DB에 변경을 가하는 메소드를 실행하였을 때, 바로 DB가 업데이트 되지 않고 영속성 컨텍스트 내부에 있는 Cache를 거쳐서 DB가 업데이트 된다. 이를 1차 Cache라 한다.

1차 Cache에는 영속 상태의 엔티티를 저장하며, Map의 형태로 만들어진다. 
> 즉, key는 id 값으로 value는 해당 entity 값이 들어있다. <br/>
> key 값이 id이기 때문에 id를 조회하게 되면 영속성 컨텍스트에 있는 1차 cache에 value로 entity가 있는 지 확인하고 있다면 DB 조회 없이 return한다.

하나의 트랜잭션에서 id 값으로 조회하는 entity들은 1차 cache에 저장을 하여 관리함으로써 JPA의 조회 성능이 올라간다.

단, 1차 Cache는 쓰레드 간에 공유하지 않고 하나의 쓰레드 시작할 때부터 끝날 때까지 잠깐 사용한다.

* 쓰기 지연

Entity 값을 변경하면 DB에 바로 업데이트 하지 않는다.

트랜잭션 내부에서 영속 상태의 entity 값을 변경하면 INSERT 쿼리들은 DB에 바로 적용되지 않고 쿼리 저장소에 쿼리문들을 쌓아둔다.

쿼리 저장소에 쌓인 쿼리들은 EntityManager의 `flush()` 나 트랜잭션의 COMMIT을 통해 보내진다.

* 변경 감지 ( Drity Cheching )

[ Dirty Checking Flow ]

1. 트랜잭션을 COMMIT 하면, EntityManager의 내부에서 먼저 `flush()`가 호출된다.
2. 엔티티와 스냅샷을 비교하여 변경된 엔티티를 찾는다.
3. 변경된 엔티티가 있으면 update 쿼리를 생성해서 쓰기 지연 쿼리 저장소에 저장한다.
4. 쓰기 지연 쿼리 저장소에 쌓인 쿼리들을 `EntityManager.flush()` 로 플러시한다.
5. DB에 트랜잭션을 COMMIT 한다.

> FLUSH <br/>
> 영속성 컨텍스트의 변경 내용을 DB에 반영하는 것이다. <br/>
> 1차 Cache를 지우지 않고, 쿼리를 DB에 날려 DB와의 싱크를 맞추는 역할을 한다.

[영속성 컨텍스트의 모든 것](https://velog.io/@seongwon97/Spring-Boot-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8Persistence-Context)

## 5. What is Spring Bean

Bean은 스프링에서 사용하는 POJO 기반 객체이다.

상황과 필요에 따라 Bean을 사용할 때, 하나만 만들어야 할 수도 있고 여러 개가 필요할 때도 있고 어떤 한 시점에서만 사용해야 할 때가 있을 수도 있다.

이를 위해 scope를 설정해서 bean의 사용범위를 개발자가 설정할 수 있다.

기본 설정은 singleton 방식이다.

싱글톤 패턴은 특정 타입의 bean을 딱 하나만 만들고 모두 공유해서 사용하는 것이다.


* singleton

* prototype

* request

* session

* global session

[bean scope 정리](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)