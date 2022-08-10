# What is Build and Deploy

- 컴파일과 빌드
    - 컴파일
    - 빌드
- 배포
    - CI/CD
- Jenkins
- 참고

<br/>

# 컴파일(Compile)과 빌드(Build)
## 컴파일(Compile)
컴퓨터가 이해할 수 있는 언어(바이너리 코드)로 바꾸어주는 과정을 말하며, java의 경우 컴파일의 결과로 JVM에서 실행가능한 .class 파일이 생긴다.   
__소스코드를 컴퓨터가 이해할 수 있는 기계어로 변환하는 작업__   

    <같이 알아두면 좋은 개념>
    링크: 여러 개로 분리된 소스 코드들을 컴파일한 결과물들에서 최종 실행 가능한 파일을 만들기 위해 필요한 부분을 찾아서 연결해주는 작업 

## 빌드
소스 코드를 실행 가능한 소프트웨어 산출물로 만드는 일련의 과정(jar, war)   
빌드 = 컴파일 + 링크 + etc.

> Build Tool: 일반적으로 전처리(Preprocessing), 컴파일(Compile), 패키징(packaging), 테스팅(testing), 배포(distribution)을 제공해주고 Maven, Gradle이 대표적이다.   

<br/>

# 배포(Deploy)
작성한 코드를 빌드하고, 빌드가 완성된 __실행 가능한 파일__(jar, war)을 사용자가 접근할 수 있는 환경에 __배치__ 하면 배포가 완료된 것이다.   
빌드를 하고 생성된 jar 또는 war 파일을 WAS에 올리는 것을 배포라고 한다.

<br/>

### 배포 프로세스 
1. git에 소스를 올리고
2. 코드가 제대로 동작하는지 테스트 코드를 작성하고
3. 이를 수행 및 검증하는 작업까지

이를 사용자가 직접 반복해서 해야한다. 이것을 자동화시키는 빌드 자동화/배포 자동화를 해야한다(CI/CD)

<br/>

## CI/CD 
### CI (Continuous Integration, 지속적 통합)
고전적 방식: 모든 개발이 끝난 이후에 코드 품질을 관리   
단점: 모든 코드를 다 작성하고 통합하면 많은 에러 발생   
 그때 가서 다시 고치면 비용이 많이 들었을 것   
-> 이를 해결하기 위해 주기적으로 계속 통합시켜 주면서 발생하는 에러들을 확인하고 고쳐나가는 방식이 등장   
-> 이것이 바로 CI(지속적 통합)

<br/>

### CI의 프로세스
1. 코드를 통합한다.   
    - 깃에 자신이 개발한 코드를 올려서 통합한다.
2. 통합한 코드가 제대로 동작하는지 테스트한다.
3. 제대로 빌드가 되는지 테스트한다.
4. 결과를 정리하고 버그가 존재한다면 적어둔다.   
5. 이 과정을 반복한다.   
-> 자동화 도구가 필요해짐   
-> Jenkins, Travis CI

<br/>

### CD (Conticuous Delivery / Continuous Deployment, 지속적 배포)
소프트웨어가 항상 __신뢰 가능한 수준__ (테스트 통과, 빌드 성공..)에서 지속적으로 배포될 수 있도록 관리하자는 개념   
CI가 선행되어야 CD가 가능해짐   
- Continuous Delivery:   
 코드가 정상적으로 배포가 가능한지 검증(Prepare Release)하고 검증이 완료되면 수종으로 배포
- Countinuous Deployment:   
  Prepare Release 후 자동으로 배포   

<br/>

### CI/CD 장점
1. 테스트 자동화로 인한 소스의 버그 및 충돌 방지
2. 배포 과정의 단순화로 프로젝트의 데브옵스 의존도가 낮아져 더 신속한 배포가 가능해짐

<br/>

# Jenkins
대표적인 CICD 툴   
Java 기반 무료 open source로 잘 갖춰진 에코시스템, 커뮤니티, 다양한 플러그인이 있어 CI/CD 툴로 많이 사용한다.   
Java Runtime 위에서 동작하는 자동화 서버(설치되는 서버에 자바가 설치되어 있어야 함)    
Jenkins가 할 수 있는 일   
1. 프로젝트 환경 세팅 자동화
2. 프로젝트 별 구분된 파이프라인 구성   
3. 자동화된 정적 테스트를 통한 소스코드 품질 향상
> 파이프라인: 데이터 처리 단계의 출력이 다음 단계의 입력으로 이어지는 형태로 연결된 구조(일련의 자동화 작업 순서들의 집합), Pipeline DSL로 작성

<br/>

## Jenkins의 동작 방식
1. 개발자가 Pipeline에 빌드 및 배포 방식을 지정
2. git main branch에 commit
3. PipeLine이 Webhooks를 감시하여 이벤트를 실행 
> Jenkins Pipeline: CICD를 구현하기 위한 플러그인 모임으로 빌드 및 배포에 대한 설정 관리, Jenkins Pipeline을 이용하면 Jenkins의 Job들을 순차적, 병렬적으로 실행시킬 수 있게 된다. 
>   > Job: Jenkins에서의 모든 작업 단위

<br/>

### 배포 환경의 종류
- 개발자가 개발을 하는 local 환경
- 개발자들끼리 개발 내용에 통합 테스트를 하는 개발 환경
- 개발이 끝나고 QA 엔지니어 및 내부 사용자들이 사용해 보기 위한 QA 환경
- 실제 유저가 사용하는 Production 환경

<br/>

## Jenkins로 Git 프로젝트 빌드 자동화 하는 방법
기본적으로 자바가 설치되어있어야 함
1. 배포할 서버에 Jenkins 설치
2. Jenkins 디폴트 포트는 8080, 필요한 경우 포트 변경
3. AWS EC2 서버를 사용하고 있는 경우 인바운드 규칙에 해당 포드 허용
4. IP:Jenkins 포트로 Jenkins에 로그인(pw는 Jenkins/secrets/initialAdminPassword 에 존재)
5. Jenkins에서 기본 설치
6. 필요한 로그인 정보 입력(Jenkins에 접속할 때마다 로그인 필요)
7. Jenkins url을 입력하면 Jenkins 메인 페이지가 보여짐
8. 프로젝트 생성
9. 소스코드 관리 부분에 Git 레포지토리 URL을 적음   
    - 오류가 날 경우 배포 서버에 git이 설치되어 있는지 확인해보자
10. git에서 Webhooks 설정
11. 소스코드를 commit push 하고 Jenkins 프로젝트 내의 빌드에서 Console Output으로 정상적으로 배포 완료되었는지 확인한다.

<br/>

# 참고

[빌드, 배포, 컴파일의 개념&차이점](https://choseongho93.tistory.com/296)   
[테코톡_빌드와배포](https://www.youtube.com/watch?v=6SvUZqbU37E)   
[젠킨스로 git 프로젝트 빌드 자동화 하는 방법](https://mysql.tistory.com/26)   


