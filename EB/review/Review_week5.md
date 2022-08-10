# Review of Studying week 5

> 목차
> * Another patteren about MVC pattern
> * What is difference between http(1.0, 2.0) and https
> * issue of spring security with restful api
> * How to execute spring application with spring security in web
> * What is sever push ( that is skill about http 2.0 )

## 1. Another patteren about MVC pattern

### Observer Pattern

옵저버 패턴은 관찰자 패턴이라고도 한다. 1:N 관계를 이루고 있으며, 상태가 업데이트되면 모든 옵저버들에게 정보를 갱신할 수 있도록 하는 것을 의미한다. 예시를 통해 알아보자.

1. 유튜브 구독 알람

짱구가 공부하며 들을 음악을 찾기 위해 유튜브에서 음악 관련 영상을 찾고 있다. 그런데 힙재라는 사람의 채널에 마음에 드는 플레이리스트가 있는 걸 알게 되었다. 해당 채널의 업로드된 영상을 모두 시청해서 새로운 영상이 올라오는 것을 기다리고 있다. 영상이 언제 올라올 지 모르니, 채널 구독을 하여 업데이트 알림을 받으려 한다. 구독 시, 유튜브 채널 관리자가 영상을 업데이트하면, 구독하고 있는 모든 사용자들에게 알림을 전달할 수 있다.

2. 신문사 구독

스물다섯, 스물하나 드라마에서 남자 주인공이 신문 배달을 하는 장면이 나온다. 과거에는 지금처럼 스마트폰이나 인터넷 보급이 널리 되지 않아서 종이 신문을 구독하였다. 사용자가 신문사에 신문 구독을 신청하면, 신문사에는 해당 집에 신문을 배달하도록 주소를 등록한다. 그리고 다음날 새벽에 신문 배달부가 구독자 목록에 있는 모든 집에 신문을 배달한다. 만일 사용자가 구독 해지를 한다면, 이 고객은 신문사의 구독자 목록에서 제외된다. 

위의 2개의 예시를 다이어그램으로 표현하면 구조가 동일하다. 특정 정보를 여러 뷰에서 동시에 얻기 위해 Subject ( 정보를 갖고 있는 대상 )에 Observer ( 정보를 원하는 대상 )를 등록하여 정보를 받는 것을 옵저버 패턴이라고 한다. 

### 정의

Head First Design Pattern에서는 옵저버 패턴에 대해 다음과 같이 정의한다.
> 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들에게 정보가 자동으로 갱신되는 방식으로, 일대다 의존성을 정의한다. 

옵저버 패턴에서 Subject는 Obeserver에 관한 정보가 없다. 오직 Observer가 특정 인터페이스를 구현한다는 것만 알고 있다. 즉, Observer가 무슨 동작을 하는지 모른다는 것이다. 또한, Observer는 언제든지 새로 추가되거나 제거될 수 있으며 새로운 형식의 Observer를 추가할 때에도 Subject에 전혀 영향을 주지 않는다. 이러한 관계를 **느슨한 관계**라고 한다. 

![image](https://user-images.githubusercontent.com/61505572/182789091-918553c0-c1d6-4980-8e28-375322c2a5c8.png)
> 출처 : [디자인 패턴 정리 블로그](https://luckygg.tistory.com/181)

위의 그림을 보면, Subject 인터페이스는 등록, 해제, 갱신을 위한 API를 제공한다. 그리고 이를 상속받는 concrete Subject 클래스는 등록, 해제, 갱신을 구현하고 기타 함수도 구현할 수 있다. Observer 인터페이스는 Subject에서 갱신할 때 호출되는 udpate API만 제공한다. 마지막으로 Observer 인터페이스를 상속받는 A,B, C 클래스에 update를 구현한다. 이것이 가장 기본적인 구조이다.

## 2. What is difference between http(1.0, 2.0) and https

|HTTP 1.0|HTTP 2.0|HTTPS|
|------|---|---|
|한 번의 요청이 있을 때마다 연결 설정 과정을 거치고, 응답을 받으면 바로 연결을 해제한다.|한 커넥션으로 동시에 여러 개의 메세지를 주고 받을 수 있으며, 응답은 순서에 상관 없이 stream으로 주고받는다.|HTTP에 데이터 암호화가 추가된 프로토콜로, HTTP가 SSL과 통신하고 SSL이 TCP와 통신한다.|

### HTTP 1.0 vs. HTTP 1.1

![image](https://user-images.githubusercontent.com/61505572/182791280-dded8376-d765-456b-87c1-5160486994f2.png)

위의 그림을 보면 알 수 있듯이, HTTP 1.0 버전에서는 TCP Connection을 위해 요청마다 연결 설정 과정을 해야 한다.
반면, HTTP 1.1 버전에서는 하나의 TCP Connection이 열려 있으면 그 연결을 통해 여러 요청에 대한 응답을 받을 수 있다. 이를 Kepp-alive 상태라 한다.

![image](https://user-images.githubusercontent.com/61505572/182791609-afe603ce-9fe5-4c70-9028-4cabfc9c4a08.png)

위의  그림을 보면, HTTP 1.1 Keep-Alive PipeLining은 파이프라이닝을 사용할 때, 클라이언트는 여러 개의 요청을 보낼 때, 각각의 요청에 대한 응답이 오길 기다리지 않고 보낼 수 있다.
반면, HTTP 1.1 Keep-Alive Multiple Connections는 클라이언트가 많은 양의 데이터를 검색하는 성능을 높이기 위해 다중 연결을 할 수 있게 한다. ( 이를 그림에서는 세션으로 표현했다. )

### HTTP 2.0

HTTP 1.1 프로토콜을 계승해 동일한 API면서 성능 향상에 초점을 맞췄다. 기존에 평문을 사용하고 개행으로 구별되던 HTTP 1.x 프로토콜과 달리 2.0은 바이너리 포맷으로 인코딩된 Message, Frame으로 구성된다.

![image](https://user-images.githubusercontent.com/61505572/182792338-e1191b6c-f854-4f9d-a47d-80cdce2478c4.png)

* Stream
구성된 연결 내에서 전달되는 바이트의 양방향 흐름, 하나 이상의 메세지가 전달 된다.
* Message
논리적 요청 또는 응답 메세지에 매핑되는 프레임의 전체 시퀀스이다.
* Frame
HTTP 2.0 에서 통신의 최소 단위로, 하나의 frame haeder가 포함된다. 

## 3.  What is sever push ( that is skill about http 2.0 )

클라이언트가 요청한 자원에 연관된 다른 자원들을 임의로 클라이언트에 초기 요청의 결과값과 함께 push할 수 있다. 이를 통해 클라이언트의 요청을 최소화해 성능 향상을 이끌어낸다.

예를 들어, 클라이언트가 index.html을 요청한 경우 HTTP 2.0은 index.html에서 사용되는 script.js와 style.css를 index.html과 함께 임의로 클라이언트에게 push할 수 있다.

![image](https://user-images.githubusercontent.com/61505572/182796116-0dac6af1-d512-4bdc-85b1-c600e3836eee.png)

[Nginx 실습 코드 포함 정리글](https://minholee93.tistory.com/entry/Nginx-HTTP2)
[이것도 보면 좋은 글](https://sunjoong85.github.io/pwa,/http2/2018/04/16/Pogressive-Web-App-HTTP2%EC%99%80-Server-Push.html)
[흐름 정리 stackoverflow](https://stackoverflow.com/questions/11077857/what-are-long-polling-websockets-server-sent-events-sse-and-comet)


## 4. How to execute spring application with spring security in web

### How to execute Spring Security 
![image](https://user-images.githubusercontent.com/61505572/182797438-533e1a5f-d43d-428f-8eb2-cc171696e1c0.png)

Spring security는 요청이 들어오면 Servlet Filter Chain을 자동으로 구성하고 거치게 한다. 

![image](https://user-images.githubusercontent.com/61505572/182797580-30deb509-a904-4439-827d-7f176a8b0824.png)

Filter는 J2EE 표준 스펙 기능으로 디스패처 서블릿에 요청이 전달되기 전후에 url 패턴에 맞는 모든 요청에 대해 필터링을 할 수 있는 기능을 제공한다.
즉, 스프링 컨테이너에서 제공되는 기능이 아니라 톰캣과 같은 웹 컨테이너에 의해 관리되는 것이다.

![image](https://user-images.githubusercontent.com/61505572/182797804-176c7457-26ca-40ab-a5a2-3515acc699b8.png)

filter는 서블릿 기술이어서 스프링 컨테이너 밖에서 동작한다. 그래서 과거에는 스프링으로 filter를 통제할 수 없었다. 하지만 DelegatingFilterProxy라는 기술로 spring bean으로 filter를 등록하여 사용할 수 있게 되었다.

![image](https://user-images.githubusercontent.com/61505572/182798047-006bdb17-76c7-4920-8d6f-6a059ff7ac4b.png)

그리고 Spring security는 DelegatingFilterProxy에 의해 빈으로 등록된 FilterChainProxy를 활용해 SecurityFilterChain에 있는 filter들을 사용할 수 있다.

[DelegatingFilterProxy] -> [FilterChainProxy] -> [SecurityFilterChain]

![image](https://user-images.githubusercontent.com/61505572/182798383-ac11aba2-a0d0-435f-bc9a-c705651590a9.png)

SecurityFilterChain은 FilterChainProxy에서의 요청에 대해 호출해야하는 *스프링 보안 필터를 결정하는 데* 쓰인다.

### FilterChainProxy를 사용하는 이유

DelegatingFilterProxy라는 기술로 spring bean으로 filter를 등록하여 사용할 수 있게 되었다. 그런데 FilterChainProxy를 사용해 프록시를 한 번 더 사용하는 이유는 무엇일까??? FilterChainProxy를 사용하면 아래와 같은 이점이 있기 때문이다.

1. 모든 Spring Security의 서블릿 이용에 대한 시작점을 제공한다.
따라서 문제가 생기면 FilterChainProxy에 디버깅 포인트를 잡아서 빠르게 오류를 수정할 수 있다.
2. Spring Security의 중심점으로 잡으로서 선택이 아닌 필수적 작업들이 누락없이 실행된다. 
예를 들어, 메모리 낭비를 방지해 SecurityContext를 지우거나 Http Firewall를 적용하여 특정 공격으로부터 어플리케이션을 보호한다.
3. SecurityFilterChain의 호출시기를 유연하게 조저랗ㄹ 수 있다. 
원래 서블릿 컨테이너는 URL에 따라서만 호출할지 안할지를 결정할 수 있다. 하지만 FilterChainProxy를 사요앟면 RequestMatcher 인터페이스를 활용하여 HttpServletRequest의 조건을 걸어 호출이 가능하다.
4. FilterChainProxy는 어떤 SecurityFilterChain를 사용할지 결정하는 데 쓰일 수 있다.
한 어플리케이션 안에서 여러가지 인증 방식을 사용하는 데에 설정을 완전히 분리할 수 있는 환경을 만들 수 있다. 

![image](https://user-images.githubusercontent.com/61505572/182799606-10d017ae-9eff-4e6f-8f93-12c300f2db27.png)

### Summary

![image](https://user-images.githubusercontent.com/61505572/182803335-3a172673-3a89-46cf-b4b1-8e3e28c159f9.png)

1. 클라이언트가 요청을 보낸다.
2. Spring Security의 Filter에서 요청을 받는다.
3. DelegatingFilterProxy에서 SecurityFilterChain에 등록된 권한을 확인한다.
4. 추가로 등록된 Filter가 있다면 확인한다.
5. 최종적으로 통과한 요청을 Servlet에게 전달한다.
6. Servlet은 HandlerMapping에게 요청에 맞는 Controller 객체를 추출하도록 한다.
7. Controller는 처리 결과 데이터를 Model에 담아서 반환한다.
8. Servlet은 ViewResolver로부터 응답 결과를 생성할 view 객체를 추출한다.
9. View는 클라이언트에게 전달할 응답을 반환한다.
10. 최종적으로 클라이언트에게 응답을 전달한다.

## 5. issue of spring security with restful api

Spring Security에서 filter를 통해 접근 가능한 url을 판별한다. RESTful API의 경우, 자원을 기반으로 url을 구분하는데, 자원 뒤에 슬래시 구분자를 붙여서 요청을 할 경우 Spring Security에서는 부정확한 url에 대해 통과시키지 말아야 할 것을 통과시킬 수 있다.

