# Spring MVC Framework

## MVC Pattern

- 소프트웨어 공학에서 사용되는 디자인 패턴
- Model, View, Controller로 나뉘어 역할을 분할하여 처리함
- 서로의 결합도 ⬇️, 유지보수성 ⬆️


### MVC Pattern 처리 순서

1. 사용자의 Request를 Controller가 받음
2. Controller는 Business logic을 Service와 함께 처리한 후 결과를 Model에 담음
3. Model에 저장된 결과를 바탕으로 View를 제어하여 사용자에게 표현함

## What is Spring MVC Framework?

- Spring에서 제공하는 웹 모듈
- MVC 패턴을 따르면서 Spring만의 독자적인 Class를 통해 처리함
- 사용자의 다양한 HTTP request 처리, 여러 형태(텍스트 형식, REST 형식, View를 표시하는 html 등)의 응답을 할 수 있도록 도와주는 프레임워크
- `Model`, `View`, `Controller` 로 구성
- 웹 어플리케이션을 유연하고 확장 가능하게 만들어줌

## Spring MVC 주요 구성 요소

- `Model`, `View`, `Controller` 를 유기적으로 동작시키기 위해 다양한 구성요소가 함께함

### DispatcherServlet(Front Controller)

- HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 Front Controller
    - Front Controller : 앞쪽에서 처리하는 컨트롤러
- 다른 객체들 사이의 흐름을 제어함

### HandlerMapping

- Client의 요청 URL(or URI)을 어떤 Controller가 처리할지 결정함
- 요청 URL과 Controller Class의 Mapping 관리
- 클래스들을 또는 클래스 내의 메소드들을 Mapping 할 수 있음

### Controller(Handler)

- HTTP Request를 처리해 Model을 만들고 View를 지정
- DispatcherServlet에 의해 배정된 Controller는 HTTP Request를 처리하고, HTTP Request의 메시지를 처리해 필요한 데이터를 뽑아 Model에 저장
- HTTP Request에 따라서 HTTP가 보여줄 View Name을 지정함
    - View에 Model의 데이터를 세팅하지는 않음

### ModelAndView

- Controller에 의해 반환된 Model과 View가 Wrapping된 객체

### ViewResolver

- ModelAndView 객체를 처리해 View를 그림
- 사용자에게 보여지는 View가 만들어지는 곳


[그림 참고](https://kotlinworld.com/326#ModelAndView%25-A%25--Controller%EC%25--%25--%25--%EC%25-D%25--%ED%25--%25B-%25--%EB%25B-%25--%ED%25--%25--%EB%25--%25-C%25--Model%EA%25B-%BC%25--View%EA%25B-%25--%25--Wrapping%EB%25--%25-C%25--%EA%25B-%25-D%EC%25B-%25B-)