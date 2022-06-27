# What is Spring?

## 스프링이란?

✔︎ `Spring` = `Spring Framework`

- 웹 애플리케이션 개발을 위한 Java 기반  `오픈 소스 애플리케이션 프레임워크`
- 좋은 객체 지향 애플리케이션 개발을 위한 프레임워크

😃 `Spring Boot` : Spring을 더 쉽게 이용하기 위한 도구

## 스프링 특징

- `경량 컨테이너`로서 자바 객체를 직접 관리
    - 각각의 객체 생성, 소멸 등 라이프 사이클 관리
    - 일종의 `컨테이너`로 볼 수도 있음
        - `myBatis`, `Hibernate` 등 DB 처리 라이브러리와 연결할 수 있는 인터페이스 제공
- 제어의 역전 (`IoC`, Inversion of Control)
    - 애플리케이션의 **느슨한 결합** 도모
    - 사용자의 제어권이 프레임워크에 있음

      → 사용자는 객체 직접 생성 X, 스프링이 **모든 의존성 객체(Bean)를 스프링이 실행될 때 만들고 사용자가 필요한 곳에 주입**함

- 의존성 주입 (`DI`, Dependency Injection)
    - 어떤 객체를 사용하는 주체가 객체를 생성하는 것이 아니라 **객체를 외부에서 생성해서 사용하려는 주체 객체에 주입**시켜주는 방식
    - 주체와 객체의 의존성 ⬇️
- 관점지향 프로그래밍(`AOP`, Aspect-Oriented Programming)
    - OOP(Object Oriented Programming)를 보완하기 위해 나온 개념
    - 핵심적인 관점, 부가적인 관점 등으로 나누어서 보고 관점을 기준으로 각각 `모듈화(어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것)`하고, 핵심적인 비즈니스 로직에서 분리하여 **재사용**함
- 트랜잭션 관리
    - 추상화된 트랜잭션 관리를 지원하고, 설정 파일(`xml`, `java`, `property` 등)을 이용한 선언적인 방식 및 프로그래밍을 통한 방식 모두 지원함
- MVC Pattern
    - `DispatcherServlet`이 `Controller` 역할을 담당하여 각종 요청을 적절한 서비스에 분산
    - 서비스들이 처리한 결과는 다양한 형식의 `View` 서비스들로 화면에 표시 가능


https://goddaehee.tistory.com/156
https://velog.io/@zayson/Spring-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8-1-Spring%EC%9D%98-%EB%93%B1%EC%9E%A5%EB%B0%B0%EA%B2%BD%EA%B3%BC-Spring%EC%9D%B4%EB%9E%80
https://melonicedlatte.com/2021/07/11/174700.html
https://engkimbs.tistory.com/746