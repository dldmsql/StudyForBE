# What is Spring  

> 목차
> * spring 개요
> * spring framework
> * 빌드 관리 도구

JAVA 기반의 **오픈소스** 애플리케이션 framework이다.
동적인 웹 사이트 개발을 위한 여러 가지 서비스를 제공해준다.
> 오픈소스 : 소프트웨어 혹은 하드웨어의 제작자의 권리를 지키면서 원시 코드를 누구나 열람할 수 있도록 한 소프트웨어, 오픈소스 라이선스에 준하는 모든 통칭을 일컫는다. <br/>
framework : 개발 시, 개발자가 준수해야 하는 정해진 규칙 ( 혹은 정의 ) <br/>
library : 특정 기능에 대한 도구나 함수들의 집합으로 개발자가 직접 library 안에 있는 도구나, 함수들을 사용한다.

## Srping Framework
1. 경량 컨테이너로서 자바 객체를 직접 관리한다.
    각각의 객체 생성, 소멸과 같은 life cycle을 관리하며 spring으로부터 필요한 객체를 얻어 올 수 있다.

2. IoC ( Inversion of Control )
    애플리케이션의 느슨한 결합을 도모한다.
    컨트롤의 제어권이 사용자가 아닌, framework에 있어 필요에 따라 spring에서 사용자의 코드를 호출한다.

3. DI ( Dependency Injection )
    각각의 계층이나 서비스들 간에 의존성이 존재할 경우, framework가 서로 연결해준다.
    > 컨트롤러에서 서비스를 생성자 주입하는 걸 떠올리자. <br/>
    장점
    * 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다.
    * 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.
    정적인 클래스 - public, template ?
    동적인 객체 인스턴스 - Model ?

4. AOP ( Aspect-Oriented Programming )
    transaction, logging, secure과 같이 여러 모듈에서 공통적으로 사용하는 기능의 경우, 해당 기능을 분리하여 관리할 수 있다. <br/>
    예: '그루터기' 코드에서 UserInterceptor를 떠올리자.
    > OOP는 관심사가 같은 데이터를 한 곳에 모아 분리하고 낮은 결합도를 갖게 하여 독립적이고 유연한 모듈로 캡슐화하는 것을 의미한다. 이 과정에서 중복된 코드 증가 및 가독성, 확장성, 유지보수성 감소의 문제가 있다.
    이를 보완하기 위해, 핵심기능과 공통기능을 분리시켜 핵심 로직에 영향을 끼치지 않게 공통기능을 추가하는 개발 형태이다.

5. iBatis, myBatis, Hibernate 등과 같이 DB처리 라이브러리와 연결할 수 있는 interface를 제공한다.

6. MVC 패턴
    MVC는 사용자 인터페이스, 데이터 및 논리 제어를 구현하는 데 널리 사용되는 소프트웨어 디자인 패턴이다. <br/>
    client < -- > WAS[controller <> service <> DAO] < -- > DB
    
    HTML과 JAVA를 분리하여 처리하기에 확장성과 유지보수성이 높다.

7. Batch framework
    spring은 특정 시간대에 실행하거나 대용량의 자료를 처리하는 데 쓰이는 일괄 처리( Batch Processing )를 지원한다.


**spring framework 사용이유**
DI를 통해 단위 테스트를 가능하게 하고, AOP를 통해 복잡도를 낮춘다. 이는 개발자의 생산성을 향상시키고, 유지보수성을 높이는 효과가 있다. <br/>
spring 기반 프로젝트는 배포, 클라우드, 빅데이터, 배치 및, 보안과 같은 솔루션을 제공한다.<br/>
추가적으로, spring framwork 5.0 부터는 `리액티브 프로그래밍`을 지원한다.<br/>
MSA는 일반적으로 EDD 기반이다. spring framwork에는 이를 지원하는 리액티브 스트림, 리액터, spring webflux가 있다.

## 빌드 관리 도구
프로젝트에서 작성한 JAVA 코드와 각종 필요한 파일 ( xml, properties, jar )을 JVM이나 WAS가 인식할 수 있도록 패키징 해주는 빌드 과정을 도와주는 도구이다.

1. maven
* JAVA용 프로젝트 관리 도구이다. 
* pom.xml 파일에 빌드 중인 프로젝트, 빌드 순서, 외부 라이브러리 종속성 관계를 명시한다.
* 외부저장소에서 필요한 라이브러리와 플러그인들을 다운로드 한 뒤, 로컬의 캐시에 모두 저장한다.

2. gradle
* maven의 대안으로 나온 완전한 오픈소스 빌드 관리 도구이다.
* 코드가 간결하다.
* 프로젝트의 어느 부분이 업데이트 되었는지 알기 때문에, 이미 업데이트가 된 것에 대해서는 재실행하지 않는다. ( 빌드 시간 단축 )

3. 둘의 차이
    * gradle이 코드가 더 간결하다.
    * gradle은 빌드시간이 훨씬 단축된다.
    * gradle은 설정 주입 방식을 제공한다. ( maven은 멀티 프로젝트에서 특정 설정을 다른 모듈에서 사용하려면, 상속을 받아야 한다. )

참고 사이트 : [spring이란](https://velog.io/@ssoyeong/Spring-Spring%EC%9D%B4%EB%9E%80) | [spring정의 및 특징 정리](https://goddaehee.tistory.com/156) | [maven과 gradle 차이](https://jisooo.tistory.com/entry/Spring-%EB%B9%8C%EB%93%9C-%EA%B4%80%EB%A6%AC-%EB%8F%84%EA%B5%AC-Maven%EA%B3%BC-Gradle-%EB%B9%84%EA%B5%90%ED%95%98%EA%B8%B0)