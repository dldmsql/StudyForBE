# What is Spring MVC?

### Spring MVC란?
> Spring 프레임워크에서 제공하는 웹 모듈이다.
- MVC 는 Model-View-Controller 의 약자 
- 기본 시스템 모듈을 MVC 로 나누어 구현
- Model 은 데이터 디자인( 어플리케이션의 정보나 데이터, DB) 담당 
  - ex. 상품 목록, 주문 내역 등
- View 는 실제로 렌더링되어 보이는 페이지( 사용자에게 보여지는 화면, UI) 담당
  - ex. .JSP 파일
  - 모델로부터 정보 얻고 표시
- Controller 는 사용자의 요청을 받고, 응답을 주는 로직 담당
  - ex. GET 등의 uri 매핑
  - 모델과 뷰 통제
  - MVC 패턴에서 모델과 뷰가 직접적인 상호 소통 못하도록
- Spring MVC 모듈을 사용하여, 백엔드 프로그래밍의 기본 프레임워크를 잡는다. </BR>
-> Web 서버에 특화되어 만들어진 모듈이라, 개발자가 해야할 영역을 더 적게 만들어줌</BR>
->
즉 기존에 Spring 보다 더 깔끔하고 간편하게 개발 가능

### MVC1 패턴
https://i.imgur.com/rzhzcZc.png
>  View와 Controller를 모두 JSP가 담당하는 형태
- JSP 하나로 유저의 요청을 받고 응답을 처리하므로 구현 난이도는 쉬움

- 단순한 프로젝트에는 괜찮겠지만 내용이 복잡하고 거대해질수록 JSP 하나에서 MVC 가 모두 이루어지다보니 재사용성도 떨어지고, 가독성 낮아짐 
</BR>
-> 유지보수에 있어서 문제가 발생합니다.

### MVC2 패턴
https://i.imgur.com/keastvz.png
> MVC2 패턴은 널리 표준으로 사용되는 패턴</BR>
 요청을 하나의 컨트롤러(Servlet)가 먼저 받아 MVC1과는 다르게 Controller, View가 분리</BR>
 따라서 역할이 분리되어 MVC1패턴에서의 단점을 보완 </BR> (개발자는 M, V, C 중에서 수정해야 할 부분이 있다면, 그것만 꺼내어 수정)  </BR> -> 유지보수성 좋아짐

- MV2는 MVC1 패턴보다 구조가 복잡해질 수 있지만, 개발자가 세부적인 구성까지 신경쓰지 않을 수 있도록 각종 프레임워크들이 잘 발전 
  - ex) 스프링 프레임워크

### Spring Framework의 MVC2 패턴
https://i.imgur.com/blr7x6q.png
> 요청 -> 프론트 컨트롤러 -> 핸들러 매핑 -> 핸들러 어댑터 -> 컨트롤러 -> 로직 수행(서비스) -> 컨트롤러 -> 뷰 리졸버 -> 응답(jsp, html)
- 컨트롤러 중에서도, 맨 앞단에서 유저의 유청을 받는 컨트롤러를 프론트 컨트롤러라고 한다.
  - DispatcherServlet 객체가 이 역할을 한다.
  - 본격적으로 로직에 들어오기 전에, 요청에 대한 선처리 작업을 수행한다.
  - ex. 지역 정보 결정, 멀티파트 파일 업로드 처리 등
- 프론트 컨트롤러는 요청을 핸들러 매핑을 통해 해당 요청을 어떤 핸들러가 처리해야하는지를 매핑한다.
  - HandlerMapping 객체가 핸들러 매핑에 대한 정보를 담고있다.
- 이렇게 매핑된 핸들러를 실제로 실행하는 역할은 핸들러 어댑터가 담당한다.
  - HandlerAdapter 객체가 이 역할을 한다.
- 컨트롤러는 해당 요청을 처리하는 로직을 담고있다.
  - 보통 요청의 종류 혹은 로직의 분류에 따라 내부적으로 Service 단위로 나누어 모듈화 한다.
  - 각 서비스에서는 DB 접근할 수 있는 Repository 객체를 이용하여 데이터에 접근할 수 있다.
- 컨트롤러는 서비스에서의 로직 처리 후, 결과를 뷰 리졸버를 거쳐 뷰 파일을 렌더링하여 내보낸다.
  - ViewResolver 객체가 이 역할을 한다.
  
## 참고 자료
https://dailyheumsi.tistory.com/159 </br>
https://chanhuiseok.github.io/posts/spring-3/ </br>
https://gmlwjd9405.github.io/2018/12/20/spring-mvc-framework.html