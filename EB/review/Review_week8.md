# Review of Studying week 8

## What is thread and process

### Process
쉽게 말하면, 실행중인 프로그램을 의미한다.

하나의 운영체제 위에서 여러 개의 프로세스가 실행 되는 것을 멀티태스킹이라 한다. 멀티태스킹에서 프로세스와 프로세스가 전환되는 것을 컨텍스트 스위칭이라 한다. 

* 컨텍스트 스위칭
하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태를 보관하고 새로운 프로세스의 상태를 적재하는 작업이다.

컨텍스트 스위칭은 운영체제의 CPU 자원을 할당하는 스케쥴러에 의해 발생한다. 

* CPU 스케쥴러
스케쥴러는 READY 큐에 존재하는 프로세스들을 특정한 우선순위를 기반으로 CPU를 할당받게 해주는 역할을 한다.

* PCB
Process Control Block은 특정한 프로세스를 관리할 필요가 있는 정보를 포함하는 운영체제 커널의 자료구조이다.

앞서 컨텍스트 스위칭에서 프로세스의 상태를 보관한다고 했다. 그 상태를 저장하는 것이 PCB이다.

* Process Status

`new` : 프로세스가 생성되는 상태 <br/>
`ready` : 프로세스가 CPU에 할당되어, 처리되기를 기다리는 상태 <br/>
`running` : 프로세스가 CPU에 할당되어, 명령어들이 실행되는 상태 <br/>
`waiting` : 어떤 이벤트의 발생으로 인해 프로세스가 기다리는 상태 <br/>
`terminated` : 프로세스가 종료되는 상태<br/>

* Process Memory

`code` : 실행할 프로그램의 코드 및 매크로 상수가 기계어 형태로 저장되는 영역이다. CPU는 여기에 저장된 인스트럭션을 하나씩 처리한다. <br/>
`data` : 코드에서 선언한 전역 변수와 정적 변수가 저장되는 영역이다. 프로그램의 시작과 함께 할당되어 종료될 때 소멸된다. <br/>
`heap` : 동적 메모리 할당 공간이므로 사용이 끝나면 운영체제가 쓸 수 있도록 반납해야 한다. 메모리 주소 값에 의해서만 참조되고 사용되는 영역이다.

* IPC
프로세스들 간의 의사소통이다. 

`왜 의사소통이 필요하지?`

1. 프로세스 간의 정보 공유

2. 작업 처리 속도 높이기 위해

특정 작업을 여러 개의 작업으로 쪼개어 프로세스의 병렬성을 키움으로써 처리 속도를 높일 수 있다.

3. 모듈화

특정한 시스템 기능을 별도의 프로세스로 구분하여 모듈화된 시스템을 구성할 수 있다.

4. 편의성

다수의 사용자가 동시에 여러 가지 작업을 수행할 수 있다.


https://velog.io/@cchloe2311/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0

https://charlezz.medium.com/process%EC%99%80-thread-%EC%9D%B4%EC%95%BC%EA%B8%B0-5b96d0d43e37

https://y-oni.tistory.com/entry/%EC%89%AC%EC%9A%B4-%EC%84%A4%EB%AA%85-IPCInter-Process-Communication%EB%9E%80

### Thread

프로세스 내에서 실행되는 흐름의 단위를 말한다. 일반적으로 하나의 프로세스에는 1개 이상의 스레드를 갖는다.

스레드는 프로세스 내에서 스택 영역만 별도로 할당 받고 이외의 영역은 부모 프로세스의 영역을 공유한다.

* 스레드 사용 이점

1. 하나의 프로세스 안에 스레드를 여러 개 두면, 스레드 하나가 blocked일 때 다른 스레드가 CPU를 잡고 수행한다.
2. 자원을 공유한다.
3. 동일한 일을 수행하는 여러 스레드가 협력하여 높은 처리율과 성능 향상을 얻을 수 있다.

## What is difference between thread and process

### 1. 자원의 효율성

멀티 프로세스로 할 경우, 프로세스를 생성하여 자원을 할당하는 비용이 크다. 이를 멀티 스레드로 할 경우, 비용 측면에서 이점을 얻는다.

### 2. 응답 시간 단축 및 처리 비용 감소

IPC를 사용하는 통신은 상대적으로 비용이 크다. 왜? 스레드는 프로세스의 메모리 공간을 공유하기 때문에, 스택 영역에 대해서만 컨텍스트 스위칭을 하면 된다.

### 3. 멀티 스레드의 안정성 문제

여러 갱의 스레드가 동일한 데이터 공간인 임계 구역을 공유하면서 이들을 수정한다는 점에서 필연적으로 생기는 문제이다. 

멀티 프로세스의 경우, 문제가 발생하면 해당 프로세스를 중단하고 다시 시작하면 된다. 하지만, 멀티 스레드는 하나의 스레드가 임계구역을 망쳐두면 다른 스레드에게도 영향이 간다.


## What is spring batch

로깅/추적, 트랜잭션 관리, 작업 처리 통계, 작업 재시작, 건너뛰기, 리소스 관리 등 대용량 데이터 처리에 필수적인 기능을 제공한다. 또한, 최적화 및 파티셔닝 기술을 통해 대용량 및 고성능 배치 작업을 가능하게 하는 기능을 제공한다.

spring batch에서 배치가 실패하여 작업 재시작을 해야 한다면, 처음부터가 아닌 실패 지점부터 실행한다.

또한, 중복 실행을 막기 위해 성공한 이력이 있는 배치는 동일한 파라미터로 실행 시, 오류를 뱉는다.

spring batch는 배치 job을 관리하지만, job을 구동하거나 실행시키는 기능은 지원하지 않는다. job을 실행하기 위해서는 Quartz, Scheduler, Jenkins 등 전용 스케쥴러를 사용해야 한다.

### 용어 정리

* Job

배치 처리 과정을 하나의 단위로 만들어 놓은 객체이다. 

* JobInstance

Job의 실행의 단위를 나타낸다. job을 실행하면 하나의 job instance가 생긴다. 예를 들어, 오늘 날짜인 8월 29일 데이터를 배치 처리 하면 8월 29일 job instance가 생기고, 내일 8월 30일에 배치 처리를 하면 8우러 30일 job instance가 생긴다. 이 둘은 다른 instance이다.

* JobParameters

job instance는 어떻게 구분할까? JobParameters 객체로 구분한다. String, Double, Long, Date 4가지 형식만을 지원한다.

* JobExecution

JobInstance 실행 시도에 대한 객체이다. JobInstance에 대한 상태, 시작 시간, 종료 시간, 생성 시간 등의 정보를 담고 있다.

* Step

job의 배치 처리를 정의하고 순차적인 단계를 캡슐화한다. job은 최소한 1개 이상의 step을 가져야 하며, job의 실제 일괄 처리를 제어하는 모든 정보가 들어 있다.

* StepExecution

JobExecution과 동일하게 step 실행 시도에 대한 객체이다. job이 여러 개의 step으로 구성되어 있을 경우, 이전 단계의 step이 실패하면 다음 단계의 step이 실행되지 않는다.

* ExecutionContext

job에서 데이터를 공유할 수 있는 데이터 저장소다. JobExecutionContext와 StepExecutionContext 2가지 종류가 있다.

JobExecutionContext는 `commit`시점에 저장된다.

StepExecutionContext는 `실행`사이에 저장된다.

ExecutionContext를 통해 step간 데이터 공유가 가능하며 job 실패 시 ExecutionContext를 통한 마지막 실행 값을 재구성할 수 있다.

* JobRepository

모든 배치 처리 정보를 담고 있는 메커니즘이다. job이 실행되면 JobRepository에 JobExecution과 StepExecution을 생성하게 되며, JobRepository에서 Execution 정보들을 저장하고 조회하며 사용한다.

* ItemReader

Step에서 Item을 읽어오는 인터페이스이다. 

* ItemWriter

처리 된 데이터를 쓸 때 사용한다. 쓰기 작업은 처리 결과물에 따라 `insert`가 될 수도 `update`가 될 수도 있다. 

* ItemProcessor

`ItemReader`에서 읽어온 Item 데이터를 처리하는 역할을 한다. 

https://deeplify.dev/back-end/spring/batch-architecture-and-components

https://khj93.tistory.com/entry/Spring-Batch%EB%9E%80-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0

## What is Spring Scheduling

일정한 시간 간격 또는 일정한 시각에 특정 로직을 수행하기 위해서 사용하는 것을 스케쥴러라고 한다. spring에서는 spring scheduler와 spring Quartz 2가지 방식을 제공한다.

### Spring Scheduler

application 클래스에 `@EnableScheduling` 어노테이션을 붙인 뒤, 원하는 작업을 처리할 메소드 위에 `@Scheduled` 어노테이션을 붙이면 된다.

이때, 스케쥴링할 메소드에는 2가지 조건이 걸린다.

1. void 의 리턴 타입을 가져야 한다.
2. 파라미터를 가질 수 없다.

스케쥴러는 기본적으로 스레드 1개를 이용하여 동기 형식으로 진행된다. 여러 개의 스케쥴을 걸었다면, 1번 스케쥴이 끝나기 전에 다음 스케쥴의 시작 시간이 되어도 실행되지 않는다. 만일 비동기 형식으로 스케쥴링 하고 싶다면, `@EnableAsync` 어노테이션을 이용하면 된다.

### Spring Quartz

우선 의존성을 추가해야 한다.

```` bash
implementation "org.springframework.boot:spring-boot-starter-quartz"
````

Quartz는 `Job`, `JobDetail`, `Trigger` 3가지 구성요소를 가져야 한다.

* Job

실제로 실행되는 로직을 작성하는 곳이며, 인터페이스로 구현되어 있다.

* JobDetail

Job을 실행하기 위한 구체적인 정보를 갖는 인스턴스이다. 

* Trigger

Job이 실행되는 실행 조건을 갖는 인스턴스이다. 


Spring Scheduler보다 복잡한 대신 그만큼 다양한 기능을 제공해준다.

1. 스케쥴러 간의 클러스터링

여러 서비스 노드가 있을 때, 해당 노드들의 스케쥴러 간의 클러스터링을 책임진다. 

2. 스케쥴러 실패에 대한 후처리

만약 스케쥴러가 실패했을 때나 Thread Pool에서 사용할 수 있는 Thread가 없는 경우에 후처리 기능을 제공한다.

3. JVM 종료 이벤트를 캐치하여 스케쥴러에게 종료를 알려준다.

https://sabarada.tistory.com/113

## What is class loader

자바 컴파일러를 통해 `.class` 확장자를 가진 클래스 파일은 하나의 디렉토리에 있는 것이 아니다. 그리고 기본적인 라이브러리의 클래스 파일들은 $JAVA_HOME 내부 경로에 존재한다. 각각의  클래스 파일들을 찾아서 JVM 메모리에 탑재해주는 역할을 하는 것이 ClassLoader이다. 

ClassLoader는 이외에도 Loading, Linking, Initialization 3가지 역할을 한다.

**Loading**

클래스 파일을 탑재하는 과정

**Linking**

클래스 파일을 사용하기 위해 검증하고 기본 값으로 초기화하는 과정

**Initialization**

static field 값들을 정의한 값으로 초기화하는 과정

![image](https://user-images.githubusercontent.com/61505572/187200654-87b82c63-f39a-4cc2-b6ec-d913d64a3fab.png)

- 부트스트랩 클래스 로더

최상위 클래스 로더이다. 

JVM이 실행될 때, 같이 메모리에 올라간다.

Java API들을 로드한다.

- 익스텐션 클래스 로더

기본 Java API를 제외한 확장 클래스를 로드한다.

- 시스템 클래스 로더

어플리케이션의 클래스 자체를 로드한다.

### 1. Loading

ClassLoader가 필요한 클래스 파일들을 찾아 메모리에 적재하는 것을 말한다. 

![image](https://user-images.githubusercontent.com/61505572/187200698-0025860b-bbf0-4c32-9937-9e684598f053.png)

위의 그림에 보여지는 코드와 명령어는 클래스 로더를 테스트하기 위한 것이다. 부트스트랩 클래스 로더는 rt.jar 파일을 찾아 JVM에 올리고, 익스텐션 클래스 로더는 lib/ext 파일을 찾아 JVM에 올린다. 어플리케이션 클래스 로더는 시스템 클래스 로더라고도 하며, classpath에 있는 클래스들을 탑재한다. 

![image](https://user-images.githubusercontent.com/61505572/187200721-f193dc25-c023-46eb-9c83-c9893e0f22aa.png)

### 왜 부트스트랩 클래스 로더는 null 값이지?

![image](https://user-images.githubusercontent.com/61505572/187200758-00a22995-1f3b-4a47-830d-7682feace5c6.png)

JVM이 시작되면 부트스트랩 클래스 로더가 실행된다. 이건 기계 명령어 즉, C나 C++ 같이 네이티브 코드로 작성되어 있다. 

이렇게 계층형으로 클래스를 찾으려 했지만, 찾지 못했다면 ClassNotFoundException 이라는 예외를 던진다.

Loading 과정에서는 하위 클래스로더가 로딩한 클래스 파일은 상위 클래스 로더가 로딩한 클래스 파일을 볼 수 있다. 하지만 역은 불가능하다.

또한, 한 번 JVM에 탑재된 클래스 파일은 종료될 때까지 JVM에서 제거되지 않는다.

### 2. Linking

로드된 클래스 파일을 검증하고 사용할 수 있게 준비하는 과정을 의미한다.

![image](https://user-images.githubusercontent.com/61505572/187200789-3e20deeb-c97b-4c02-a2b3-cc12891d548c.png)

1. verify

클래스 파일이 유효한 지 확인하는 과정이다. 

1. prepare

클래스 및 인터페이스에 필요한 static field 메모리를 할당하고, 이를 기본값으로 초기화한다. 기본값으로 초기화된 static field는 뒤의 initialization 단계에서 코드에서 작성한 초기값으로 변경된다.

1. resolve

명확히 정의되지 않은 심볼릭 값을 JVM의 메모리 구성 요소인 method area의 런타임 환경 풀을 통해 메모리 주소 값으로 바꾸어준다.

### 3. Initialization

클래스 파일의 코드를 읽는 과정이다. 이때 JVM은 멀티 스레딩으로 작동하며, 같은 시간에 한 번에 초기화를 하는 경우가 있기 때문에 초기화 단계에서도 동시성을 고려해야 한다.

![image](https://user-images.githubusercontent.com/61505572/187200821-46bad0b4-879e-4a5d-b839-28e8c07ad0a9.png)

### JAVA 9버전 이후의 변경
![image](https://user-images.githubusercontent.com/61505572/187200840-a5054f95-8966-4121-a3f4-f394c6be331a.png)
