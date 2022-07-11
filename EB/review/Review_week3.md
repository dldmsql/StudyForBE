# Review of Studying week 3

> 목차
> * What is JPA
> * What is ORM
> * What is REST
> * What is RestTemplate
> * What is difference between RestTemplate and ResponseEnity
> * What is Lamba Expression
> * What is JVM
> * How do lambda Expression execute on the JVM
> * Why to come up with using functional programming

## 1. What is JPA
JPA는 Java Persistence api의 약자로, Java 기반 ORM이다.

## 2. What is ORM
ORM은  RDB와 object 간의 매핑을 해준다.

ORM을 사용함으로써, OOP 개발에 집중할 수 있다. 그리고, 코드의 중복을 제거할 수 있으며 유지보수성이 높다.

## 3. What is REST
자원을 이름으로 구분하여 자원의 상태를 주고 받는 모든 것을 지칭한다.

REST는 URI, Method, Representation으로 구성된다.
* URI - 자원 명시
* Method - 자원에 대한 행위 명시
* Representation - 자원에 대한 표현

REST 기반으로 동작하기 위해서는 1) 클라이언트-서버 구조이어야 하며, 2) Cachable 3) Stateless 4) Uniform Interface 5) Layered System 6) Code on demand여야 한다.

## 4. What is RestTemplate
Spring 3.x부터 지원하였으며, 4.x부터 비동기도 지원한다.

Spring에서 Restful하게 개발할 수 있도록 지원해준다. RestTemplate을 통해 request와 response를 JSON<> Entity로 변환하도록 converter를 부르고 api 동작 중에 error handling을 한다.

## 5. What is difference between RestTemplate and ResponseEntty
RestTemplate을 통해서 RestFul하게 개발할 수 있다고 되어 있다. 하지만, RestTemplate을 사용하지 않고 우리는 REST API를 개발했다 ? 

ResponseEntity<T>를 통해 클라이언트에게 응답을 보내는데, 이 클래스는 data, status, message를 기본으로 갖고 있다. 그리고, HttpEntity를 상속 받고 있는데, Http 기반으로 동작하는 웹 서비스에서 서버-클라이언트가 주고 받는 기본 형식을 추상화 한 것이다.

따라서, REST하게 개발하였다고 단언할 수는 없지만 그 형식을 따르는 ResponseEntity, RequestEntity,RestTemplate과 같은 미리 정의되어 있는 클래스를 통해 REST API를 개발할 수 있다.

## 6. What is Lambda Expresion
람다식은 Stream을 활용하기 위한 기술로서, java 8부터 등장하였다. 

람다식을 사용하면 불필요한 코드를 줄이고, 가독성을 확보할 수 있다.

## 7. What is JVM
jvm은 자바 가상 머신으로 자바 프로그램이 운영체제에 종속되지 않은 채 동작할 수 있도록 해준다. 

jvm은 GC, execution engine, class loader, runtime data area로 구성되어 있다.

runtime data area는 method, stack, heap, pc register, native mehtod stack으로 구성되어 있다.

## 8. How do lambda Expression execute on the JVM
람다식 내부의 변수가 아닌 것들을 자유 변수라 부른다. 람다식 내부에서 자유변수를 참조하는 행위를 람다 캡처링이라고 한다.

람다캡처링을 할 때, 자유변수로서 지역변수를 사용할 경우 컴파일 에러가 발생한다. 이는 지역변수를 final로 선언하지 않았기 때문이다.

지역변수는 stack영역에 저장되는 값으로, thread 간의 고유의 stack을 갖기 때문에. 멀티 쓰레드 환경에서 이는 동기화 문제를 야기시킨다.

따라서 값의 재할당이 일어나지 않도록 final로 선언하여야 한다.

만일, 자유변수로 인스턴스변수나 정적 변수를 사용한다면 에러가 발생하지 않는다. 그 이유는 두 변수 모두 heap에 저장되기 때문이다. heap은 모든 thread가 공유하는 메모리 영역이다.

fianl로 선언된 지역변수를 자유변수로서 람다 캡처링에서 사용한다면, thread는 이 변수를 직접 접근하여 사용하지 않고, 자신의 stack영역에 복사하여 사용한다. 따라서 원본 thread가 사라져도 에러가 발생하지 않는다.

## 9. Why to come up with using functional programming
함수형 프로그래밍은 상태, 가변 데이터를 지양하는 패러다임이다.

코드의 가독성을 높이고, 직관적으로 코딩을 하며, 상태가 변하지 않는 것을 기반으로 하기 때문에 병렬 처리에서도 오류가 발생하지 않는다.
