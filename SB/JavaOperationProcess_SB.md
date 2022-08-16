# 자바 동작 과정

- 자바는 OS에 독립적인 특징을 가짐

  → JVM이 OS와 프로그램 사이에서 기계어로 해석


## 자바 코드 실행 과정

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcfSW9K%2FbtrnBzNjEGy%2FD2rZAIVpTkPYenTr2ZmDv1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcfSW9K%2FbtrnBzNjEGy%2FD2rZAIVpTkPYenTr2ZmDv1%2Fimg.png)

1. 개발자가 자바 코드를 작성한다.
2. .java 파일을 자바 컴파일러를 통해 java 바이트 코드로 컴파일한다.
3. 컴파일된 바이트 코드를 JVM의 Class Loader에 전달한다.
4. Class Loader는 동적 로딩을 통해 필요 클래스들을 로딩 및 링크하여 JVM의 메모리인 Runtime Data Area에 올린다.
5. Execution Engine은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다.

<aside>
👩🏻‍💻 `동적 로딩`
실행 시에 모든 클래스가 로딩되지 않고 **필요한 시점**에 로딩하여 사용
다형성과 같은 **객체지향** 개념이 적용될 수 있게 해줌

- 로드 타임 동적 로딩 : 하나의 클래스를 로딩하는 과정에서 필요한 다른 클래스를 동적으로 로딩하는 것
- 런타임 동적 로딩 : 코드를 실행하는 순간에 필요한 클래스를 로딩하는 것

[https://stack07142.tistory.com/306](https://stack07142.tistory.com/306)

</aside>

## JVM(Java Virtual Machine)

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbzoUrb%2FbtrnBNx2A0r%2FkqQYneH2IkFo4rp9EaG5q0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbzoUrb%2FbtrnBNx2A0r%2FkqQYneH2IkFo4rp9EaG5q0%2Fimg.png)

- 컴파일된 바이트 코드를 기계어로 변환
- 스택 기반의 가상 머신
- 메모리 관리와 GC(Garbage Collection) 수행
- **Class Loader + Runtime Data Areas + Execution Engine**으로 구성

## Class Loader

- 자바의 동적 클래스 로딩 기능 처리
- 클래스를 처음 참조하는 런타임을 할 때 클래스 파일을 로딩, 연결, 초기화

### 1. **로딩(Loading)**

- BootStrap, Extension, Application 컴포넌트들에 의해 클래스들이 로드됨
- **BootStrap ClassLoader** : JRE의 lib 폴더에 있는 rt.jar 파일에서 기본 자바 API 라이브러리를 로드함. 가장 최우선으로 로드됨
- **Extension ClassLoader(Platform ClassLoader)** : JRE의 lib/ext 폴더에 있는 모든 확장 코어 클래스 파일들을 로드함. JDK 확장 디렉토리(JAVA_HOME/lib/ext 디렉토리 혹은 java.ext.dirs)에서 로드됨. BootStrap ClassLoader의 child
- **Application ClassLoader(System ClassLoader)** : Extension ClassLoader의 child, 어플리케이션 레벨에 있는 클래스들을 로드함. 즉 사용자가 직접 정의한 클래스 파일들 로드. Classpath 환경변수에 있는 클래스 파일이나 -classpath 또는 -cp 명령어 옵션이 있는 파일 로드

### 2. **연결(Linking)**

- **검증(verify)** : 바이트코드 검증기는 생성된 자바 바이트코드가 적절한지 아닌지에 대해 검증하며 검증 실패 시 검증 오류 내보냄
- **준비(prepare)** : 모든 정적변수의 메모리가 할당되며 기본 default 값으로 할당됨
- **해석(resolve)** : 모든 심볼릭한(명확하게 정의되지 않은) 메모리 참조를 메소드 영역에 있는 타입으로 직접 참조함

### 3. **초기화(initialize)**

- 클래스 로딩의 마지막 단계로, 모든 정적 변수가 자바 코드에 명시된 값으로 초기화되며 정적 블록 실행

## Runtime Data Area

### Method Area

- 모든 클래스 수준(클래스 명, 부모 클래스 명, 메소드, 변수) 데이터 저장 공간
- 공유자원으로 JVM 당 하나의 영역만 존재함 → 스레드에 안전 X

### Heap Area

- 모든 인스턴스 오브젝트(클래스, 배열 등) 저장 공간
- 공유자원으로 JVM 당 하나의 영역만 존재함 → 스레드에 안전 X

### Stack Area

- 각 스레드마다 개별 스택 영역 존재
- 메소드 호출 시 스택 프레임이라는 스택 메모리에 쌓이게 됨
- 모든 지역 변수의 저장 공간
- 공유자원이 아니므로 스레드에 안전함

### PC Register

- 현재 실행 중인 명령문의 주소를 가지기 위해 각 스레드마다 개별로 존재함

### Native Method Stacks

- native 메소드 정보를 가지고 있는 스택
    - native method : 다른 언어로 구현된 native라는 예약어를 가지는 메소드
- 각 스레드마다 개별로 생성됨

## **Execution Engine**

- Runtime Data Area에서 할당된 바이트 코드는 Execution Engine에 의해 실행됨
- Execution Engine은 바이트 코드를 읽으며 조각 단위별로 실행함

### Interpreter

- 바이트 코드를 빠르게 해석하지만 실행 속도 느림
- 하나의 메소드가 여러 번 호출 시 매번 해석이 필요하다는 단점

### JIT Compiler (Just-In-Time)

- 인터프리터의 성능적인 단점 보완
- 반복되는 코드가 발견될 시 JIT 컴파일러를 이용하여 반복되는 부분을 native code(원시코드)로 컴파일함

### Garbage Collector

- 아무 참조가 없는 생성된 인스턴스들을 모아서 제거하는 역할

[https://yaelimeee.tistory.com/70](https://yaelimeee.tistory.com/70)

[https://sas-study.tistory.com/262](https://sas-study.tistory.com/262)