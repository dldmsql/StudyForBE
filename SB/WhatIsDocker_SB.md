# Docker🐳

- 다양한 종류의 애플리케이션을 신속하게 구축, **테스트 및 배포**할 수 있는 소프트웨어 플랫폼
- VM과 달리 하드웨어 자원을 모두 가상화하는 게 아니라 **프로세스들만 격리**시켜 빠르게 환경 구축
    - VM과 비교하여 용량과 속도 부분에서 많은 차이가 있음
- Docker **Image로 만들어 배포**하고 **컨테이너로 실행** 가능함

<aside>
💡 컨테이너란?
격리된 공간에서 프로세스가 동작하는 가상화 기술
주로 OS를 가상화한 기존 가상화 방식과는 달리 프로세스를 격리하여
가볍고 빠르게 동작함. CPU나 메모리는 프로세스가 필요한 만큼만 추가로
사용하고 성능적으로도 손실이 없음.

</aside>

- Docker 객체 : `이미지`, `컨테이너`, `볼륨`, `네트워크`

### Docker의 장점🌟

- 구성 단순화 : Dockerfile에 전체 운영 환경변수를 담아 전달하여 **모든 플랫폼에서 실행 가능**
- 빠른 배포 : 컨테이너는 **별도의 OS를 부팅하지 않고 애플리케이션을 실행**하므로 컨테이너를 빠르게 만들 수 있음
- 코드 관리 : 환경 자체를 배포하므로 **개발 및 코딩을 편하게** 만들어줌
- 개발 생산성 향상 : 리눅스 기반 환경의 도커 컨테이너를 실행하는 경우에도 Shared Volume 기능을 통해 Windows 로컬 환경에서 에디터를 통해 소스코드를 수정 가능하여 업무 효율이 향상됨
- 애플리케이션 격리 : Web Server와 연결된 API 서버들을 격리해야할 경우 다른 컨테이너를 통해 API 서버를 실행시킬 수 있음

## Docker Engine

- 클라이언트와 서버 아키텍처를 따르는 애플리케이션
- 호스트 시스템에 설치되며 기본적으로 3가지 구성요소를 가짐

<aside>
💡 Server : Dockered라는 Docker Daemon을 통해 도커 이미지를 만들고 관리함
REST API : Docker Daemon에게 무엇을 할지 지시하는 용도로 사용
CLI(Command Line Interface) : Docker 명령어를 입력하는데 사용되는 클라이언트

</aside>

## Docker Image

- 어떠한 개발 환경을 구축하기 위해 **필요한 라이브러리 및 패키지를 모아 하나의 파일로** 만든 것
- 컨테이너 실행에 필요한 파일과 설정값 등을 포함함
- Docker Hub를 통해 배포된 각 Application의 공식 이미지인 `Base Image`에 사용자가 프로그램, 라이브러리, 소스를 설치한 뒤 하나의 파일로 만든 것을 `Docker Image`라고 함
- 레이어(Layer)가 쌓이는 방식으로 만들어짐

<aside>
💡 기존 이미지 레이어 위에 read-write 레이어를 추가하는 방식으로
`유니온 파일 시스템`을 이용하여 여러 개의 레이어를 하나의 파일 시스템으로 사용할 수 있도록 함

파일이 수정될 때마다 원본 이미지에 대한 중복을 없애고 수정된 값만 관리
새로운 버전 전체 설치 필요 X 추가된 새로운 레이어만 다운받으면 됨!

`유니온 파일 시스템`
읽기 전용의 파일을 수정할 때 쓰기가 가능한 파일을 임시로 생성하고
수정이 끝나는 경우 기존의 읽기 파일로 대체되는 방식의 파일 시스템

</aside>

## Docker Container

- 이미지가 **실행**된 형태, 즉 프로세스
- 모든 애플리케이션 및 환경은 생성된 컨테이너에서 실행됨
- 호스트와 이미지에는 아무런 영향을 주지 않고 **Docker 엔진에서 독립적으로 실행됨**
- 오버헤드가 적어 가볍고 빠르게 동작함
- CLI 또는 Docker API 등을 통해 컨테이너를 실행, 중지, 삭제할 수 있음
    - CLI(Command Line Interface) : 도스, 명령 프롬프트, bash, Terminal
    - Docker API

<aside>
💡 Docker Container의 종류
`시스템 컨테이너` : 운영체제 위에 하드웨어 가상화 없이 운영체제 실행
`애플리케이션 컨테이너` : MariaDB, Python 등 필요한 프로그램만 Docker로 별도 실행

</aside>

### Docker Daemon(Dockered)

- 도커의 API 요청을 수신하고 도커 객체를 관리함
- 데몬은 도커 서비스를 관리하기 위해 다른 데몬들과 통신할 수 있음

### Docker Registry

- 도커 이미지를 업로드해서 공유하는 저장소
- 도커 공식 레지스트리인 `Docker Hub`를 통해 다른 도커 사용자들의 이미지를 가져올 수 있고, `docker pull`, `docker run` 등의 CLI 명령어를 실행하여 필요한 도커 이미지를 가져올 수 있음

### Docker Repository

- 이름이 같지만 태그가 다른 도커 이미지들의 논리적 모음
- 각 태그의 구분으로 독립된 이미지임을 확인할 수 있음

### Docker Client

- 호스트에 명령을 내리는 인터페이스
- 보통 Terminal(Mac, Windows)에서 CLI 명령어 기반으로 진행됨
- CLI 명령어를 통해 실행될 때 클라이언트는 이를 데몬으로 보내 실행시킴

### Docker Volume

- 도커 컨테이너를 생성하면 컨테이너의 데이터는 기본적으로 볼륨에 저장됨
- 기본적으로 생성한 컨테이너를 제거하는 경우 볼륨에 저장된 내부 데이터도 함께 사라짐

    <aside>
    💡 `Bind mount`
    Host의 특정 폴더 경로를 마운트하여 컨테이너의 생명주기와 상관 없이 데이터를 Host에 지속적으로 저장 가능하게 하는 방법

    </aside>


### Docker Network

- 모든 격리된 컨테이너가 통신하는 통로
- 총 5가지의 네트워크 드라이버가 있음

<aside>
💡 `Bridge` : 컨테이너의 기본 네트워크 드라이버로, 도커 자체적으로 제공하는 드라이버이며 애플리케이션이 독립된 컨테이너에서 실행될 때 사용
`Host` : 컨테이너와 호스트 간의 네트워크 격리가 필요하지 않을 때 제거함
`Overlay` : 컨테이너가 다른 호스트에서 실행 중이거나 `Swarm` 서비스가 여러 애플리케이션에 의해 형성될 경우 사용
`None` : 모든 네트워킹을 비활성화
`Macvlan` : 물리적 주소처럼 보이도록 하기 위해 mac 주소를 컨테이너에 할당함. 네트워크 트래픽은 컨테이너 간 mac 주소를 통해 라우팅됨. VM 설정을 마이그레이션하는 동안 컨테이너를 물리적 장치처럼 보이게 하려는 경우 사용

+`Swarm`
여러 대의 Docker 호스트들을 마치 하나인 것처럼 만들어주는 도구

</aside>

[https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)

[https://www.dongyeon1201.kr/c20f7d07-6f23-4134-ae8e-e730dc7b5af6#04841df6-b6ab-40f0-ab08-8f01a9e3a2de](https://www.dongyeon1201.kr/c20f7d07-6f23-4134-ae8e-e730dc7b5af6#04841df6-b6ab-40f0-ab08-8f01a9e3a2de)

[https://artistdata.tistory.com/3?category=1182143](https://artistdata.tistory.com/3?category=1182143)

[https://artistdata.tistory.com/4?category=1182143](https://artistdata.tistory.com/4?category=1182143)