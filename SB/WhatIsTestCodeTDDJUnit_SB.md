# Test Code

- 개발자가 작성한 메소드가 제대로 동작하는지 테스트하기 위한 코드
- 잘 작동하는 깔끔한 코드를 얻기 위함
- 단위테스트의 경우 수십ms로 테스트가 매우 빠름
- 문서로서의 역할 가능 → 처음 코드를 보는 개발자들이 수월하게 이해 가능
    - 개발자가 작성한 메소드가 어떻게 동작했으면, 어떤 결과를 반환했으면하는 것을 작성한 코드이기 때문

## Test Code 작성 방법

```java
public class PersonTest {
	@Test
	public void helloTest() throws Exception {
		//given
		String name = "Chosubin";
		Person p1 = new Person();

		//when
		String ment = p1.hi(name);

		//then
		assertThat(ment, is("hi Chosubin"));
	}
}
```

## Test Code Tips

### given, when, then

- **given** : 어떤 값이 주어지고
- **when** : 무엇을 했을 때
- **then** : 어떤 값을 원한다
- 테스트 코드의 가독성 향상

### 모든 Response에 대한 테스트 진행

- api가 조금이라도 수정될 경우 테스트 코드가 실패하게 됨으로써 항상 올바른 테스트 코드를 유지할 수 있도록 도움
- 테스트 코드는 커버리지가 높을 수록 좋음

### F.I.R.S.T

- **Fast** : 단위테스트는 **가능한 빠르게** 실행되어야 함
- **Independent** : 단위테스트는 객체의 상태, 메소드, 이전 테스트 상태, 다른 메소드의 결과 등에 **의존해서는 안 됨** (실행 순서에 상관 없어야 함)
- **Repeatable** : 단위테스트는 **반복 가능**해야 함
- **Self-validating** : 단위테스트는 **자체검증**이 가능해야 함 (Assert문 등)
- **Timely** : 단위테스트를 통과하는 제품코드가 작성되기 **바로 전에** 작성해야 함

## TDD(Test Driven Development)

- 테스트 주도 개발
- 반복 테스트를 이용한 소프트웨어 방법론
- 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현함
    - 단위 : 일반적으로 class
- 짧은 개발 주기의 반복에 의존하는 개발 프로세스
- eXtream Programming(XP)의 ‘Test-First’ 개념에 기반을 둔 단순한 설계 중요시함

### TDD 개발 주기

![https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/04/TDD-%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%8C%E1%85%AE%E1%84%80%E1%85%B5.png?w=1024&ssl=1](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/04/TDD-%E1%84%80%E1%85%A2%E1%84%87%E1%85%A1%E1%86%AF%E1%84%8C%E1%85%AE%E1%84%80%E1%85%B5.png?w=1024&ssl=1)

- Red : 실패하는 테스트 코드 작성
- Green : 테스트코드를 성공시키기 위한 실제 코드 작성
- Blue : 중복 코드 제거, 일반화 등 리팩토링

### TDD 개발 방식

- 일반 개발 방식 : 요구사항 분석 → 설계 → 개발 → 테스트 → 배포
- TDD 개발 방식 : 테스트 코드 작성 뒤 실제 코드 작성
- 설계 단계에서 프로그래밍 목적을 미리 정의하고 테스트해야할지 미리 정의(테스트 케이스 작성)해야 함
- 테스트 코드 작성 도중 발생하는 예외 사항은 테스트 케이스에 추가하고 설계 개선함
- 테스트가 통과된 코드만을 코드 개발 단계에서 실제 코드로 작성함

![https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/04/TDD%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%89%E1%85%A6%E1%84%89%E1%85%B3.png?w=528&ssl=1](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/04/TDD%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%89%E1%85%A6%E1%84%89%E1%85%B3.png?w=528&ssl=1)

- 반복적인 단계가 진행되며 코드의 버그가 줄어들고 소스코드는 간결해짐

[https://hanamon.kr/tdd란-테스트-주도-개발/](https://hanamon.kr/tdd%EB%9E%80-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A3%BC%EB%8F%84-%EA%B0%9C%EB%B0%9C/)

## JUnit

- 자바 프로그래밍 언어용 유닛 테스트 프레임워크
    - 유닛테스트 : 프로그래밍에서 모든 함수와 메소드에 대한 테스트 케이스를 작성하여 의도된 대로 잘 동작하는지 검증하는 절차
- TDD의 대표적인 Tool
- 테스트 결과는 Test 클래스로 개발자에게 테스트 방법 및 클래스의 history 공유 가능
- assert 메소드로 테스트 케이스의 수행 결과 판별
- 어노테이션으로 간결하게 지원

### Assert Method

- **assertArrayEquals(a, b)** : 배열 A와 B가 일치하는지 확인
- **assertEquals(a, b)** : 객체 A와 B가 같은 값을 가지는지 확인
- **assertEquals(a, b, c)** : 객체 A와 B가 일치함을 확인 (a: 예상값, b: 결과값, c: 오차범위)
- **assertSame(a, b)** : 객체 A와 B가 같은 객체임을 확인
- **assertTrue(a)** : 조건 A가 참인지 확인
- **assertNotNull(a)** : 객체 A가 null이 아님을 확인

[http://junit.sourceforge.net/javadoc/org/junit/Assert.html](http://junit.sourceforge.net/javadoc/org/junit/Assert.html)

### 기본 어노테이션

- **@Test** : 테스트를 만드는 모듈 역할
- **@DisplayName** : 테스트 클래스 또는 테스트 메소드의 사용자 정의 표시 이름을 정의
- **@ExtendWith**: 사용자 정의 확장명을 등록하는데 사용
- **@BeforeEach**: 각 테스트 메서드 전에 실행됨을 나타냄
- **@AfterEach**: 각 테스트 메서드 후에 실행됨을 나타냄
- **@BeforeAll**: 현재 클래스의 모든 테스트 메서드 전에 실행됨을 나타냄
- **@AfterAll**: 현재 클래스의 모든 테스트 메서드 후에 실행됨을 나타냄
- **@Disable**: 테스트 클래스 또는 메서드를 비활성화

[https://galid1.tistory.com/783](https://galid1.tistory.com/783)

[https://velog.io/@ehddek/JUnit-이란](https://velog.io/@ehddek/JUnit-%EC%9D%B4%EB%9E%80)