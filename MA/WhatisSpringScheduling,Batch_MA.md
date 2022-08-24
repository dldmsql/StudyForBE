# What is Spring Scheduling and Batch
### 배치(Batch)란?
데이터를 실시간으로 처리하는게 아니라, 일괄적으로 모아서 처리하는 작업을 의미 </br>
하루동안 쌓인 데이터를 배치작업을 통해 특정 시간에 한꺼번에 처리하는 경우</br>
ex) 은행의 정산작업과 같은 업무에서 이런 일괄처리를 수행</br>
사용자에게 빠른 응답이 필요하지 않은 서비스에 적용
특정 시간이후에는 자원을 거의 소비하지 x</br>
> Batch Processing란</br>
 일괄 처리</br>
실시간으로 요청에 의해서 처리되는 방식이 아닌 일괄적으로 한꺼번에 대량의 프로세스를 처리하는 방식
- 대량의 데이터를 처리
- 특정 시간에 프로그램을 실행
- 일괄적으로 처리

### Spring Batch
- Spring Batch는 로깅/추적, 트랜잭션 관리, 작업 처리 통계, 작업 재시작, 건너뛰기, 리소스 관리 등 대용량 레코드 처리에 필수적인 기능을 제공
- 또한 최적화 및 파티셔닝 기술을 통해 대용량 및 고성능 배치 작업을 가능하게 하는 고급 기술 서비스 및 기능을 제공
- 배치가 실패하여 작업 재시작을 하게 된다면 처음부터가 아닌 실패한 지점부터 실행을 
- 중복 실행을 막기 위해 성공한 이력이 있는 Batch는 동일한 Parameters로 실행 시 Exception이 발생

### 관련 용어
- Job: 배치처리 과정을 하나의 단위로 만들어 놓은 객체. 배치처리 과정에 있어 전체 계층 최상단에 위치
- JobInstance:  Job의 실행의 단위
- JobParameters: JonInstance 구분, 개발자 JobInstacne에 전달되는 매개변수 역할(String, Double, Long, Date 4가지 형식만을 지원)
- JobExecution: JobInstance에 대한 실행 시도에 대한 객체
- Step: Job의 배치처리를 정의하고 순차적인 단계를 캡슐화, Job은 최소한 1개 이상의 Step을 가져야 하며 Job의 실제 일괄 처리를 제어하는 모든 정보가 들어있음

### Spring Batch vs Scheduler
Spring Batch는 Scheduler가 아님.
Spring Batch는 Batch Job을 관리하지만 Job을 구동하거나 실행시키는 기능은 지원하고 있지 않으며 Spring에서 Batch Job을 실행시키기 위해서는 Quartz, Scheduler, Jenkins등 전용 Scheduler를 사용해야함

### Scheduler
일정한 시간간격 또는 일정한 시각에 특정 로직을 돌리기 위해서 사용하는 것</br>
Spring에서 Spring Scheduler와 Spring Quartz라는 2가지 방식으로 제공

### Spring Scheduler
- Spring Scheduler는 별도의 추가적인 의존성이 필요하지 x
- Spring Boot starter에 기본적인 의존성으로 제공 (@EnableScheduling 어노테이션)
- @Scheduled 어노테이션을 메서드에 붙임 -> 해당 어노테이션의 내부 값으로 scheduler가 실행 될 타이밍을 정할 수 있음
- 조건
1. method는 void의 return 타입을 가져야함
2. method는 파라미터를 가질 수 없음

### Spring Quartz
- 해당 라이브러리의 의존성을 추가 필요
   ```implementation "org.springframework.boot:spring-boot-starter-quartz"```

- Quartz의 Job의 필수적인 요소
1. Job : 실제로 실행되는 로직이 있는 곳. Quartz에서 interface로 제공
2. JobDetail : Job을 실행시키기 위한 구체적인 정보를 가지고 있는 인스턴스. JobBuilder API를 통해 만들 수 있음.  Job에 대한 설명 Job의 ID 등을 설정할 수 있음
3. Trigger : Trigger는 Job이 실행되는 실행 조건을 가지고 있는 인스턴스. TriggerBuilder API를 통해 만들 수 있음. 조건으로 단순히 특정 시간 간격으로 할 수 있으며 Cron으로도 작성할 수 있음

## 참고 자료
https://velog.io/@jch9537/%ED%95%9C-%EC%A4%84-%EC%9A%A9%EC%96%B4%EB%B0%B0%EC%B9%98Batch%EB%9E%80</br>
https://khj93.tistory.com/entry/Spring-Batch%EB%9E%80-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0</br>
https://sabarada.tistory.com/113