# Kubernetes

## Container Orchestration Tool

- 도커 컨테이너 갯수가 늘어나면 필요한 자원도 지속적으로 늘어나게 되고, 서버도 여러 대로 늘어날 수 있음 → **서버들과 컨테이너들을 효율적으로 관리**하기 위해 등장한 **Container Orchestration tool**
    - `Container Orchestration tool` : 컨테이너를 쉽고 빠르게 배포/확장하고 관리를 자동화해주는 도구
        - Docker Swarm, **Kubernetes**, Apach Mesos 등

<aside>
💡 **오케스트레이션 툴의 기능**
- 컨테이너 배포
- 서비스를 관리하고 유지보수하기 위한 여러 기능 자동화
➡️ 노드 클러스터링, 컨테이너 로드 밸런싱, 컨테이너의 배포와 복제 자동화, 컨테이너 장애 복구 기능, 컨테이너 오토스케일링, 컨테이너 스케줄링(적절한 서버에 배포해주는 것), 로깅 및 모니터링

</aside>

> CNCF(Cloud Native Computing Foundation) 애플리케이션 대부분이 쿠버네티스와 궁합이 잘 맞고, 주요 클라우드 벤더(Amazon, Google, Azure)와 On-premise 솔루션 업체(IBM, 시스코) 등에서 쿠버네티스를 지원하여 여러 툴 중 쿠버네티스가 제일 각광 받고 있음
>

## Desired State, Control Loop

- 쿠버네티스는 **Desired State** 즉 원하는 상태를 유지하는 게 핵심
    - 구체적으로 몇 개의 웹서버를 띄울지, 몇 번 포트로 서비스할지 등
- **Desired State**를 만들기 위해 사용하는 메커니즘이 **Control Loop**

<aside>
💡 **Control Loop**
1. Observe : Object들이 원하는 상태가 무엇인지 확인
2. Check Differences : Object들의 현재 상태가 어떤지 체크하고 원하는 상태와 비교해 어떤 차이가 있는지 확인
3. Take Action : 현재 상태를 원하는 상태로 만들어줌

위 과정을 반복하며 지속적으로 원하는 상태를 유지하게 됨

</aside>

## 관련 용어 정리

### Object

- 상태를 관리하기 위한 대상
- Basic Object와 Controller로 나뉨
- 오브젝트들은 모두 오브젝트의 특성(설정 정보)을 기술한 `Object Spec`으로 정의함
    - yaml이나 json, cl을 통해 스펙 정의

### Basic Object

- 컨테이너화가 되어 배포되는 애플리케이션의 워크로드를 기술하는 오브젝트
    - 워크로드 : 쿠버네티스에서 구동되는 애플리케이션, 서비스, 기능 등
- 종류 : `Pod`, `Service`, `Volume`, `Namespace`

### Pod

- 쿠버네티스에서 **가장 기본적인 배포 단위**로 **컨테이너를 포함**한 단위
- 쿠버네티스는 컨테이너를 개별적으로 하나씩 배포하는 것이 아니라 **Pod이라는 단위로 배포**함(Pod은 1개 이상의 컨테이너로 구성)
- Pod 내 컨테이너 간에는 IP와 Port, 디스크 Volume 공유 가능(다른 컨테이너의 파일을 읽어올 수 있음)
- 하나의 Pod은 하나의 노드에만 배치 가능, Pod 내 컨테이너가 각각 다른 노드에 배치될 수 없음

### Volume

- 컨테이너 restart와 상관 없이 파일을 영속적으로 저장해야하는 경우 사용 (ex. DB)
- 컨테이너의 외장 디스크
- Pod을 기동할 때 컨테이너에 마운트하여 사용

### Service

- 여러 개의 Pod들을 서비스할 때, 현재 요청이 어느 Pod으로 갈지 선택하는 오브젝트
- 부하가 많을 때 이를 분산시키는 로드밸런서 역할

### Controller

- 여러 개의 Pod 배포를 적절하게 관리하는 오브젝트
- Pod들을 생성하고 삭제함
- 종류 : `Replication Controller`, `ReplicaSet`, `Deployment`, `DaemonSet`, `Job`, `StatefulSet` 등

### Name Space

- 한 클러스터 내의 논리적인 분리 단위
- 한 클러스터 자원을 가지고 개발 / 운영 / 테스트 식으로 나눌 수 있음

### Label

- 리소스를 선택하는데 사용됨

## K8S Architecture

- 쿠버네티스 내부 구조는 마스터와 노드로 분리됨

### Master

- 쿠버네티스 클러스터 전체를 컨트롤하는 시스템
- **API 서버**
    - 모든 명령과 통신을 REST API로 제공
- **Etcd**
    - 분산형 key-value 스토어로 쿠버네티스 클러스터 상태나 설정 정보 저장
- **스케쥴러**
    - Pod, Service 등 각 리소스를 적절한 노드에 할당
- **컨트롤러 매니저**
    - 컨트롤러들을 생산, 배포 등 관리
- **DNS**
    - 동적으로 생성되는 Pod, Service 등의 IP를 담는 DNS


### Node

- 마스터에 의해 명령을 받고 실제 서비스하는 컴포넌트
- **Kubelet**
    - 마스터 API 서버와 통신하는 노드 에이전트
- **Kube-proxy**
    - 노드로 오는 네트워크 트래픽을 적절한 컨테이너로 라우팅
    - 네트워크 트래픽 프록시 등 노드-마스터간 네트워크 통신 관리
- **Container runtime**
    - 컨테이너를 실행 (ex. dcoker)
- **cAdvisor**
    - 각 노드 내 모니터링 에이전트
    - 노드 내 컨테이너들의 상태, 성능 수집하여 마스터 API서버에 전달

[https://dalsacoo-log.tistory.com/entry/쿠버네티스-Kubernetes](https://dalsacoo-log.tistory.com/entry/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-Kubernetes)

[https://dailyheumsi.tistory.com/208](https://dailyheumsi.tistory.com/208)