# Review of Studying week 1

> 목차
> 1. spring boot를 실행하면 그 시작점은 어디인가
> 2. DI란 무엇인가

# 1. spring boot를 실행하면 그 시작점은 어디인가

spring boot 프로젝트를 생성하면, `Application.java` 코드가 함께 생성된다.

```` bash

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
````
애플리케이션을 실행하면, 이 클래스의 `main`이 실행되는데, `run` 메소드가 실행되기 전에 `@SpringBootApplication` 어노테이션에 먼저 주목해야 한다.

## @SpringBootApplication
spring boot 프로젝트를 위한 자동 설정을 해주는 어노테이션이다.
`@EnableAutoConfiguration`, `@ComponentScan` 이 두 어노테이션도 함께 주의깊게 봐야 한다. 

공식문서에 따르면,
`@EnableAutoConfiguration`은 개발자가 추가한 jar 종속성을 기준으로 spring을 구성하는 방법을 Spring boot가 추측하도록 지시하는 어노테이션이다.
spring 프로젝트를 생성할 때, `spring-boot-starter-web` 종속성을 추가하면, 이 jar 종속성이 Tomcat과 SpringMVC를 함께 포함하기에, Spring boot도 웹 애플리케이션을 개발하고 있다고 가정하고 그에 따라 자동 구성 설정을 한다.
[개발자] -> [dependency 추가] -> [spring boot] -> [dependency에 따른 자동 구성] ( 왜? @EnableAutoConfiguration 이 어노테이션 때문에. )

auto configuration에 대상이 되는 클래스들은 미리 정의되어 있기 때문에 `@Configuration` 어노테이션이 없어도 자동으로 빈으로 등록한다. `spring.factories`를 열어 보면, 185줄에 달하는 많은 클래스들이 정의되어 있다.
이 많은 클래스들이 항상 빈으로 등록된다면, 엄청난 리소스 낭비가 될 것이다. 따라서 spring boot는 현재 프로젝트에서 필요한 부분만 auto configure 되도록 한다. 이 역할을 하는 것이 `AutoConfigurationImportSelector`이다. 
 - OnBeanCondtion
 - OnClassCondition
 - OnWebApplicationCondition
 이 3가지 조건을 기준으로 필요한 클래스인지 필터링을 한다.

[Flow]
1. SpringApplication.run()
2. SpringApplication.refreshContext()
3. ConfigurationClassPostProcessor.postProcessBeanDefinitionRegistry()
4. ConfigurationClassParser.parse()
5. AutoConfigurationImportSelector.process()
6. ConfigurationClassFilter.filter()
7. FilteringSpringBootCondition.match()

6,7번 단계를 보면, filter를 한 뒤, match()로 AutoConfigurationMetadata에 매칭되는 애가 있는 지 확인한다.

## Main Method
main method를 실행하면, spring boot의 SpringApplication 클래스의 run()을 호출한다. 그러면,auto-configure에 따라 Tomcat 웹 서버가 시작되고, Spring 기본 구성 요소인 SpringApplication을 알려주기 위해 Application.class를 인자로 전달한다.
> `SpringApplication.run(Application.class, args);`

# 2. DI란 무엇인가
**의존성** 이란?
어떤 객체가 생성되기 위해 다른 객체가 꼭 필요한 상태를 의미한다.
우리가 가장 잘 알고 있는 의존성이 바로 `new`이다.
의존성은 결합도에 비례하기 때문에, 의존성이 높아지면 결합도도 높아진다.
결합도가 높다면, 해당 클래스를 수정할 경우 그 side-effect이 발생할 수 있다. 따라서 재사용성이 힘듦을 알 수 있다.

의존성을 낮추기 위한 방법 중 하나가 바로 DI (Dependency Injection)이다.
추상화된 객체를 외부에서 주입받아야 한다.
예를 들어보자, 우리가 controller 클래스에서 service 클래스의 메소드를 호출해서 사용하는 식으로 코딩을 해왔다. 이때, Controller 클래스 내부에서 `Service service = new Service();`를 하였던가 ? controller 클래스 내부에서는 new 연산자로 생성하지 않고, `@Autowired` 어노테이션으로 생성자 주입 방식을 통해 의존성을 주입했다. 

## 의존성 주입의 장점
1. 테스트가 용이하다.
> 테스트 코드 작성 시, 필요한 클래스들을 모두 new 연산자로 생성하지 않았다.
2. 재사용성과 유연성이 높아진다.
> 추상화된 객체를 외부에서 주입받기 때문에, 추상화된 객체를 implements하는 다른 객체가 생겨나도 코드에 영향도가 낮다.
3. 코드의 중복이 없어지고, 단순해진다.
4. 코드의 가독성이 좋아진다.
5. 종속성과 결합도가 낮아진다.