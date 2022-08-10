# What is DB?

### 데이터 베이스란?
>여러 사람이 공유하여 사용할 목적으로 체계화하여 관리되는 데이터의 집합

##### - DB 사용 이유
- 데이터 공유
- 데이터 중복 최소화
- 지속성
- 보안성

### DB의 구조
> 행과 열로 이루어진 테이블 형태로 데이터를 저장

##### - 행과 열
 - 모델링 단계:  튜플, 어트리뷰트
 - 일반적:  로우(Row), 컬럼(Column) 
 - 정식 명칭: 레코드, 필드

### DBMS란?
> DBMS (Database Management System) :
데이터의 집합인 DB를 관리하고 운영하는 소프트웨어 

##### DBMS 분류
- 계층형(Hierarchical) </br>
처음으로 등장한 DBMS 개념으로 1960년대에 시작</br>
각 계층이 트리형태로 구성</br>
변경, 다른 계층을 찾아가는 것 어려움
- 망형(Network)</br>
계층형 DBMS의 문제점을 개선하기 위해 1970년대에 등장</br>
하위 구성원끼리도 연결된 유연한 구조
프로그래머가 모든 구조를 이해해야만 한다는 단점
- 관계형(Relational)</br>
 RDBMS</br>
 대부분의 DBMS가 RDBMS 형태로 사용되는 중</br>
 RDBMS는 테이블로 이루어져 있으며, 테이블은 열과 행으로 구성
- 객체지향형(Object-Oriented)
- 객체관계형(Object-Relational) 
 
 ### SQL이란?
 > SQL(Structured Query Language) :  관계형 데이터베이스에서 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어

> 관계형 데이터베이스 관리 시스템에서 자료의 검색과 관리, 데이터베이스 스키마 생성과 수정, 데이터베이스 객체 접근 조정 관리를 위해 고안됨

# RDBMS & NOSQL

### RDBMS 특징
- RDBMS는 테이블로 이루어져 있으며, 테이블은 열과 행으로 구성
- 이 테이블이 다른 테이블들과 관계를 맺고 모여있는 집합체
- 관계를 나타내기 위해 외래 키(foreign key)를 사용하며 이를 이용한 테이블 간 Join이 가능
- 저장 방식은 SQL에 의해 저장되고 있으며 정해진 스키마에 따라 데이터를 저장

### NoSQL이란?
> NoSQL(Not Only SQL) : RDB 형태의 관계형 데이터베이스가 아닌 다른 형태의 데이터 저장 기술
- RDBMS와는 달리 테이블 간 관계를 정의X
- 데이터 테이블은 그냥 하나의 테이블이며 테이블 간의 관계를 정의하지 않아 일반적으로 테이블 간 Join도 불가능
- 빅데이터의 등장으로 인해 데이터와 트래픽이 기하급수적으로 증가 </br>-> RDBMS에 단점인 성능을 향상시키기 위해서는 장비가 좋아야 하는 Scale-Up의 특징(비용 기하급수적으로 증가) </br>->데이터 일관성은 포기하되 비용을 고려하여 여러 대의 데이터에 분산하여 저장하는 Scale-Out을 목표로 등장
- RDBMS 스키마에 맞추어 데이터를 관리해야 된다는 한계를 극복하고 수평적 확장성(Scale-out)을 쉽게 할 수 있다는 장점
- 정확한 데이터 구조를 알 수 없고 데이터가 변경/확장이 될 수 있는 경우에 사용하는 것이 좋음
- 데이터 중복이 발생할 수 있으며 중복된 데이터가 변경될 시에는 모든 컬렉션에서 수정 필요
- 이러한 특징들을 기반으로 Update가 많이 이루어지지 않는 시스템이 좋으며 또한 Scale-out이 가능하다는 장점을 활용해 막대한 데이터를 저장해야 해서 Database를 Scale-Out를 해야 되는 시스템에 적합

### NoSQL 유형
1. Key-Value Database
>데이터가 Key와 Value의 쌍으로 저장
- Key: Value에 접근하기 위한 용도
-값: 어떠한 형태의 데이터라도 담을 수 있음 (이미지, 비디오,,,) 
간단한 API를 제공하는 만큼 질의의 속도가 굉장히 빠름
- NoSQL Key-Value Model: Redis, Riak, Amazon Dynamo DB ,,,

2. Document Database
>데이터가 Key와Document의 형태로 저장
- Key-Value 모델과 다른 점이라면 Value가 계층적인 형태인 도큐먼트로 저장된다는 것 
객체지향에서의 객체와 유사하며, 이들은 하나의 단위로 취급되어 저장된다. 다시 말해 하나의 객체를 여러 개의 테이블에 나눠 저장할 필요가 없어진다는 뜻이다. 
- 객체-관계 매핑이 필요하지 않다. 객체를 Document의 형태로 바로 저장 가능하기 때문이다. 
- 검색에 최적화되어 있는데, 이는 Ket-Value 모델의 특징과 동일하다. - 사용이 번거롭고 쿼리가 SQL과는 다르다는 점이다. 도큐먼트 모델에서는 질의의 결과가 JSON이나 xml 형태로 출력되기 때문에 그 사용 방법이 RDBMS에서의 질의 결과를 사용하는 방법과 다르다. 
- NoSQL Document Model: MongoDB, CouthDB 
 
3. Wide Column Database
> 테이블, 행 및 동적 열에 데이터를 저장
- 각 행이 동일한 열을 가질 필요가 없다는 점에서 뛰어난 유연성을 제공
 키는 Row(키 값)와 Column-family, Column-name을 가진다. 연관된 데이터들은 같은 Column-family 안에 속해 있으며, 각자의 Column-name을 가진다. 관계형 모델로 설명하자면 어트리뷰트가 계층적인 구조를 가지고 있는 셈이다. 이렇게 저장된 데이터는 하나의 커다란 테이블로 표현이 가능하며, 질의는 Row, Column-family, Column-name을 통해 수행된다.
- NoSQL Column-family Model: HBase, Hypertable 
 
4. Graph Database 
>Node와 Edge, Property와 함께 그래프 구조를 사용하여 데이터를 표현하고 저장
- 개체와 관계를 그래프 형태로 표현한 것이므로 관계형 모델이라고 할 수 있음
- 소셜 네트워크, 사기 탐지, 권장 엔진 같은 패턴을 찾아보기 위해 관계를 상세히 검토는 것에 유용
- NoSQL Graph Model: Neo4J

### RDBMS VS NOSQL
#### RDBMS
- 정해진 스키마에 따라 데이터를 저장하여야 하므로 명확한 데이터 구조를 보장
- 중복 저장 불가능
- 테이블 간 관계를 맺고 있어 시스템이 커질 경우 JOIN문이 많은 복잡한 쿼리 가능성
- 성능 향상을 위해서는 서버의 성능을 향상 시켜야하는 Scale-up만을 지원
이로 인해 비용이 기하급수적으로 늘어날 수 있음
- 스키마로 인해 데이터가 유연X 
 
#### NoSQL
- 스키마가 없어 유연하며 자유로운 데이터 구조(언제든 저장된 데이터를 조정하고 새로운 필드를 추가 가능)
- 데이터 분산이 용이하며 Scale-up, Scale-out 가능
- 데이터 중복 발생 가능
스키마가 존재하지 않기에 명확한 데이터 구조를 보장하지 않으며 데이터 구조 결정가 어려울 수 있습니다.

## 참고자료
 https://bio-info.tistory.com/98 </br>
 https://hongong.hanbit.co.kr/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-databasedb-dbms-sql%EC%9D%98-%EA%B0%9C%EB%85%90/ </br>
 https://khj93.tistory.com/entry/Database-RDBMS%EC%99%80-NOSQL-%EC%B0%A8%EC%9D%B4%EC%A0%90 </br>
 https://www.mongodb.com/ko-kr/nosql-explained
 
 