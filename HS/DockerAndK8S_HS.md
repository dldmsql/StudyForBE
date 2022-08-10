# What is Docker and K8S?
- 도커
    - 컨테이너
    - 도커가 유용한 경우
    - Container Image
- 쿠버네티스
    - 컨테이너 오케스트레이션
    - 특징
- 도커 vs 쿠버네티스
- 참고

# 도커(Docker)
컨테이너 기술을 기반으로 한 일종의 가상화 플랫폼   
-> 독립된 환경을 만들어서 하드웨어를 효율적으로 활용하는 기술
> 가상화: 물리적 자원인 하드웨어를 효율적으로 활용하기 위해서 하드웨어 공간 위에 가상의 머신을 만드는 기술
> 컨테이너: 컨테이너가 실행되고 있는 호스트 OS의 기능을 그대로 사용하면서 프로세스를 격리해 독립된 환경을 만드는 기술

## 컨테이너
- 베이스 환경의 OS를 공유하면서 필요한 프로세스만 격리하는 방식    
    - 애플리케이션과 애플리케이션을 구동하는 환경을 Host OS로부터 격리한 공간
- 커널을 공유하기 때문에 호스트 OS의 기능을 모두 사용할 수 있음
- 컨테이너 위에서는 호스트 OS와 다른 OS를 구동할 수 없음
- 격리시킬 애플리케이션과 거기에 필요한 파일, 특정 라이브러리 등 종속 항목만 포함하기 때문에 배포를 위해 생성되는 이미지의 용량이 작아진다는 장점을 가짐
- 운영체제x 프로세스o

## 도커가 유용한 경우
- 개발 과정에서 다른 라이브러리와 충돌하는 것을 방지하기 위해 격리된 환경이 필요할 때
- 완성된 서비스를 배포할 때
- 배포 중인 서비스를 받아서 실행해 볼 때
- 특히 배포 과정에서 도커를 사용해 필요한 파일들만 묶으면 종속성 이슈에서 벗어날 수 있음 
- 서버가 한 대일 경우는 큰 문제가 없지만 서버가 여러 대일 경우 도커를 사용하는 것이 아주 효과적

## Container Image
- 컨테이너를 실행할 수 있는 실행 파일, 설정값 등을 가지고 있는 것
- 이미지는 상태값을 가지지 않고 변하지 않음
- 이미지를 컨테이너에 담고 실행시킨다면, 해당 프로세스들이 동작
- 예를 들어, ubuntu 이미지는 ubuntu를 실행하기 위한 모든 파일을 가지고 있고, MySQL 이미지는 MySQL을 실행하는 데 필요한 파일과 실행 명령어, 포트 등을 가지고 있다.
- 즉, 이미지는 컨테이너를 실행하기 위한 모든 것을 담고 있기 때문에 더이상 의존성 파일을 컴파일하고 새로운 환경에서 더 설치할 필요가 없어짐
- 새로운 서버가 추가된다면 미리 만들어놓은 이미지를 다운 받고 컨테이너를 생성하면 됨 

# 쿠버네티스
- 컨테이너 오케스트레이션 툴

## 오케스트레이션
- 컨테이너의 수가 많아지면 관리와 운영에 있어서 어려움이 있는데, 이러한 다수의 컨테이너 실행을 관리 및 조율하는 시스템을 __컨테이너 오케스트레이션__ 이라고 함
- 오케스트레이션 엔진을 통해 컨테이너의 생성과 소멸, 시작 및 중단 시점 제어, 스케줄링, 로드 밸런싱, 클러스터링 등을 할 수 있음
- 즉, 컨테이너로 애플리케이션을 구성하는 모든 과정을 관리할 수 있음

## 쿠버네티스 특징
1. 자동화된 복구
컨테이너들을 모니터링하며, 컨테이너 중 하나라도 죽으면 빠르게 재시작 시킴
2. 로드 밸런싱
트래픽이 급격히 많아질 때 그것을 수용하기 위해 자동으로 새로운 컨테이너들을 만들어줌     
트래픽이 줄어들면 컨테이너의 숫자를 지정해둔 최소한으로 자동으로 조절
3. 무중단 서비스
쿠버네티스는 점진적 업데이트를 제공하기 때문에 서버를 중단하지 않고도 애플리케이션을 업데이트 할 수 있음
4. 호환성
클라우드를 다른 제품으로 이전할 때 특정 환경에 종속되지 않고 클라우드 환경들을 이전할 수 있음

# 도커 vs 쿠버네티스
- 도커
    - 이미지를 컨테이너에 띄우고 실행하는 기술
    - 한 개의 컨테이너를 관리하는 데 최적화
    - ex. 컨테이너를 하나만 띄워서 사용해야지!
- 쿠버네티스
    - 도커를 관리하는 툴
    - 여러 개의 컨테이너를 서비스 단위로 관리하는 데 최적화
    - ex. 0월 0일 0시에 100개의 컨테이너를 자동으로 생성해야지!


# 참고
[도커란 무엇인가](https://velog.io/@markany/%EB%8F%84%EC%BB%A4%EC%97%90-%EB%8C%80%ED%95%9C-%EC%96%B4%EB%96%A4-%EA%B2%83-1.-%EB%8F%84%EC%BB%A4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)   
[도커](https://velog.io/@whitebear/%EC%84%A4%EB%A7%88-%EB%98%90-%EC%A7%80%EB%82%98%EC%B9%98%EB%8A%94-%EA%B1%B0-%EC%95%84%EB%8B%88%EC%A7%80-%EB%8F%84%EC%BB%A4%EC%9D%B8%EB%8D%B0)   
[도커와 쿠버네티스 간단 비교](https://wooono.tistory.com/109)