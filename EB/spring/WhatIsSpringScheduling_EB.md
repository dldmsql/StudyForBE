# What is Spring Scheduling and Batch

## Batch

**일괄처리**라는 의미로, 스프링에서 배치 작업의 단위를 JOB이라 부른다. 

예를 들어, 매일 전 날의 데이터를 집계해야 한다고 가정해보자. 이 집계 과정을 어디서 수행하면 될까?

Tomact + Spring 서버를 통해 집계를 수행한다면, 컴퓨터의 CPU, I/O 등의 자원을 모두 사용해버려 다른 요청을 처리할 수 없게 될 것이다.

그래서 보통 데이터 집계는 하루에 한 번만 수행하게 된다. 이를 위해 API를 구성하는 것은 낭비일 수 있다.

단발성 대용량 데이터를 처리하는 어플리케이션을 배치 어플리케이션이라 한다.

배치는 아래의 조건을 만족해야 한다.

1. 대용량 데이터

대량의 데이터를 가져오거나, 전달하거나, 계산하는 등의 처리를 할 수 있어야 한다.

2. 자동화

심각한 문제 해결을 제외하고 사용자 개입없이 실행되어야 한다.

3. 견고성

잘못된 데이터를 충돌/중단 없이 처리할 수 있어야 한다.

4. 신뢰성

무엇이 잘못되었는지를 추적할 수 있어야 한다. ( like 로깅, 알림 )

5. 성능

지정한 시간 안에 처리를 완료하거나 동시에 실핻되는 다른 어플리케이션을 방해하지 않도록 수행되어야 한다.

## 스케쥴러

스케쥴러는 단순히 Job을 특정 시간마다 실행되도록 도와주는 프로그램이다. 

배치를 주기적으로 동작하게 해주는 스케쥴러는 대표적으로 2가지가 있다.

- Quartz 스케쥴러
- 스프링 스케쥴러

## 1. Quartz 스케쥴러

Quartz에는 크게 3가지로 구성되어 있다.

- 실행하고자 하는 Job
- job의 실행 주기를 나타내는 Trigger
- Triggers을 스케쥴러로 만드는 Scheduler

사용 방법

1. build.gradle에 관련 dependency를 추가한다.
2. Job으로 배치 작업을 진행할 서비스 구현하기
3. Context.xml에 2번에서 만든 Job을 정의할 JobDetailFactoryBean, JobDetail와 연결해 실행주기를 정의할 CronTriggerFactoryBean, Trigger를 Scheduler로 생성해줄 SchedulerFactoryBean 3가지를 설정한다.

## 2. 스프링 스케쥴러

spring 3.1 버전부터 제공하는 스케쥴러이다. Quartz 스케쥴러보다 구현이 간단하다.

@Scheduler 어노테이션을 Job으로 사용할 서비스 메소드에 붙여주면 된다.

https://smujihoon.tistory.com/226    전반적 개요

https://data-make.tistory.com/699  구현 설명