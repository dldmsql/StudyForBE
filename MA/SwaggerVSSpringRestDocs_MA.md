# Swagger VS Spring Rest Docs
:  REST API를 문서화 하기 위한 Tools

### Swagger란?
- Open Api Specification(OAS)를 위한 프레임워크
- Springboot에서 Swagger를 사용하면, 컨트롤러에 명시된 어노테이션을 해석하여 API문서를 자동으로 생성
- Java에 종속된 라이브러리 아님
- URL에 /swagger-ui.heml으로 접근하면 swagger가 만들어주는 페이지에 접근할 수 있다.
- 제공되는 Annotation으로 문서를 보완 가능
- Production Code에 Annotation으로 기술하기 때문에 코드의 가독성이 떨어짐
- Spring Security를 적용하여 권한에 따라 노출할때 설정을 하기 까다로움

### Spring Rest Docs란?
- 테스트 코드를 기반으로 자동으로 API문서를 자동으로 작성해주는 프레임워크
- Controller에 대한 Test Code를 기반으로 REST API 문서를 작성
- Test Code에서 제공하는 메서드를 통해 문서를 보완가능 -> 따라서 Production 코드에 영향 x
- 실제 만들어지는 page는 html파일 하나이기 때문에 Spring Security를 적용하여 권한관리 하기 용이

### Swagger VS Spring Rest Docs
- Swagger

|장점|단점|
|---|---|
|예쁜 디자인의 문서|프로덕션 코드의 가독성이 떨어짐|
|API 테스트 기능을 제공|문서의 신뢰도가 떨어짐(테스트 기반이 아님)|
|객체들에 대한 정보를 제공|라이브러리의 무게가 무거움|
|의존성 추가만 해도 기본 UI를 제공|많은 어노테이션들 학습 필요|
|TDD 서비스가 아니어도 사용가능|

- Spring Rest Docs 

|장점|단점|
|---|---|
|테스트 기반으로 수행됨|세부 설정하기 어려움|
|프로덕션 코드에 영향을 주지 않음|엔드포인트에 따른 코드가 많음|
|문서에 대한 신뢰성이 높음|추가적인 기능을 제공하지 않음|
|문서 본연의 기능에 충실함|디자인|

[함께 사용 -> SpringRestdocs의 장점인 테스트를 통과한 검증된 API의 문서 + Swagger의 장점인 Web UI에서 테스트 가능 + MSA 환경에서 여러 곳에 흩어진 API 문서를 한곳에서 모으는 효과](https://taetaetae.github.io/posts/a-combination-of-swagger-and-spring-restdocs/)

### test code
> 테스트 코드 : 소프트웨어의 제품/서비스의 품질을 확인하거나 소프트웨어의 버그를 찾을 때 작성하는 코드. 작성한 코드가 예상하는 대로 동작 하는지 확인하는 것
- 작성 이유
  - 기능이 정상적으로 동작하는지 확인 가능
  - 미리 결함 발견 가능 
  - Refactoring (기존의 코드를 다른 코드로 리팩토링 할 때 기존의 소스와 동일한 동작을 하는지에 대한 걱정을 하지 않아도 됨)
  - 문서로서 작용 (코드 작성자의 의도, 사용법, 주의사항 등이 드러나게 되어 있어 문서화의 효과)

### TDD (Test Driven Development, 테스트 주도 개발)
> 반복 테스트를 이용한 소프트웨어 방법론으로, 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현 -> 테스트 코드를 먼저 작성하는 것부터 시작
- 과정
  - 항상 실패하는 테스트를 먼저 작성
  - 테스트를 통과 시키기 위한 실제 코드를 작성
  - 테스트가 통과하면 중복 코드를 제거하고, 일반화시키는 과정인 리팩터링을 수행
- 테스트 코드를 작성한 뒤에 실제 코드를 작성한다는 점이 가장 큰 특징
- 디자인(설계) 단계에서 프로그래밍 목적을 반드시 미리 정의해야만 하고, 또 무엇을 테스트해야 할지 미리 정의(테스트 케이스 작성)해야함
- 장점  
1. 객체지향적이고 효율적인 모듈형 코드를 생산 가능
   - TDD는 테스트 코드를 작성해놓으면 몇 번이나 테스트를 할 수 있어 모듈화가 잘 이루어질 수 있음 </br>이에 의존성이 낮은 독립적인 코드를 만들게 되고 코드에 문제가 생겨도 전체 코드에 영향을 덜 미칠 수 있으
2. 디버깅 시간을 단축시켜서 효율적인 개발이 가능
   - 테스트 코드로 테스트를 해서 코드를 짜면 에러가 발생했을 때 쉽게 문제를 파악할 수 있고, 해결할 수 있음
3. 유지보수가 쉽고 추가적인 기능 개발이 쉬움
   - 개발이 완료된 소프트웨어에 어떤 기능을 추가할 때 가장 우려되는 점은 해당 기능이 기존 코드에 어떤 영향을 미칠지 알지 못한다는 것 </br>
    하지만 TDD의 경우 모듈형 코드로 잘 생산되어 있고, 의존성이 낮은 독립적인 코드로 이미 만들어져 있기 때문에 기능 추가가 쉽고 이는 유지보수가 쉬움
- 단점
1. 시간이 오래 걸림
   - 그러나 장기적으로 봤을 때는 오히려 시간을 단축시킬 수 있다는 점

## 참고자료
https://doozi316.github.io/web/2020/10/16/WEB29/</br>
https://velog.io/@bread_dd/Spring-Rest-Docs%EC%99%80-Open-Api-Swagger</br>
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=songintae92&logNo=221410414713</br>
https://devbksheen.tistory.com/entry/Spring-REST-Docs</br>
https://velog.io/@jybin96/%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1-%EC%8B%9C-%EC%9E%A5%EB%8B%A8%EC%A0%90%EA%B3%BC-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A2%85%EB%A5%98-%EB%B3%84-%ED%8A%B9%EC%A7%95</br>
https://velog.io/@sms8377/Testing-%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%9D%98-%EC%A2%85%EB%A5%98-%EB%B0%8F-Jest</br>
https://resilient-923.tistory.com/327