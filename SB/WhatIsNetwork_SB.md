# Network

## Protocol

- 컴퓨터나 원거리 통신 장비 사이에서 메시지를 주고 받는 통신 규약 및 약속
- 통신 프로토콜은 신호 체계, 인증, 오류 감지 및 수정 기능을 포함함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/66af46e4-9a99-43e6-b3ab-2d00b759cd31/Untitled.png)

## OSI 7 Layers

- 네트워크 통신 과정을 7단계로 나눈 것
- 국제표준화기구(ISO)에서 네트워크 간 호환을 위해 만든 표준 네트워크 모델
- 통신이 일어나는 과정을 단계별로 파악 가능
- 통신 과정 중 특정한 곳에 이상이 생길 경우 **다른 단계와 독립적**으로 해결 가능
- 송신은 7계층 → 1계층 / 수신은 1계층 → 7계층
- **상위 > 하위** : 하위 계층은 상위 계층을 위해 기능하고, 상위 계층은 하위 계층에 관여하지 않음

### Layer 1. 물리 계층(Physical Layer)

<aside>
🧑🏻‍🔬 전기적, 기계적, 기능적인 특성을 이용하여 통신 케이블로 데이터를 전송함

</aside>

- 사용되는 통신 단위는 비트(0,1)
- 데이터를 전기적인 신호로 변환해서 주고 받는 기능만 함
- 통신 케이블, 리피터, 허브 등의 장비를 통해 데이터 전송
- 데이터 전송만 하고 에러는 신경 쓰지 않음

### Layer 2. 데이터 링크 계층(Data Link Layer)

<aside>
🔗 물리 계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보 전달을 수행할 수 있도록 함

통신에서의 오류 찾기 & 재전송 기능

</aside>

- MAC 주소를 가지고 통신함
  - MAC 주소 : 네트워크 세그먼트의 데이터 링크 계층에서 통신을 위한 네트워크 인터페이스에 할당된 고유 식별자, 물리 주소(ex. wifi 주소)
- 프레임(frame) : 데이터 링크 계층에서 데이터가 전송되는 단위
- 브리지, 스위치 등의 장비 사용
- 물리 계층에서 발생할 수 있는 오류를 찾아내고 수정하는데 필요한 기능적, 절차적 수단 제공

### Layer 3. 네트워크 계층(Network Layer)

<aside>
🌐 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 라우팅 기능이 핵심

</aside>

- **라우터**를 이용하여 경로를 선택하고, 주소를 정하고, 경로에 따라 패킷을 전달
- 여러 개의 노드를 거칠 때마다 경로를 찾아주는 역할을 함
- **라우팅, 흐름 제어, 세그멘테이션, 오류 제어, Internetworking** 등 수행
  - 흐름 제어 : 송수신측 사이 데이터 처리 속도 차이(흐름)을 제어하기 위한 기법으로 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우 방지
  - 세그멘테이션 : 프로세스를 논리적 내용을 기반으로 잘라서 메모리에 배치
  - 오류 제어 : 오류 검출과 재전송
  - Internetworking : 두 개 이상의 네트워크를 연결하여 네트워크 간 하드웨어나 소프트웨어 모두를 연결시키는 방법론
- IP 주소를 할당해주는 역할을 함

### Layer 4. 전송 계층(Transport Layer)

<aside>
📤 통신을 활성화하기 위한 계층, TCP 프로토콜 이용, 포트를 열어서 응용프로그램들이 전송할 수 있게 함

데이터가 올 경우 해당 데이터를 하나로 합쳐서 5계층(세션 계층)에 던져줌

</aside>

- 단대단 오류 제어 및 흐름 제어
  - TCP/UDP 프로토콜 사용
  - 단대단 : 양 끝
- 양 끝 단의 사용자들이 신뢰성 있는 데이터를 주고 받을 수 있도록 해 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해줌
- **시퀀스 넘버 기반의 오류 제어 방식** 사용
  - 시퀀스 넘버 : 통신과 제어에서 데이터를 관리하기 위해 부여한 번호
  - 순서 역전 방지, 중복 패킷 방지 등을 이유로 사용
  - 데이터를 패킷 단위로 나눠서 전송하는데, 이것들이 섞이거나 중복되지 않게 잘 전송될 수 있도록 함
- 특정 연결의 유효성을 제어함
- 일부 프로토콜은 상태 개념(stateful)이 있고 연결 기반(connection oriented)임
  - 패킷들의 전송이 유효한지 확인하고 전송 실패한 패킷들을 다시 전송
- 종단간 통신을 다루는 최하위 계층으로 종단간 신뢰성 있고 효율적인 데이터 전송, 오류 검출 및 복구와 흐름제어, 중복 검사 등 수행

### Layer 5. 세션 계층(Session Layer)

<aside>
📎 데이터가 통신하기 위한 논리적인 연결

양 끝 단의 프로세스가 데이터 통신(송수신)을 관리하기 위한 방법을 제시

</aside>

- 단방향 통신(Simplex), 반이중 방식(Half Duplex), 전이중 방식(Full Duplex) 통신, 체크 포인트의 유무, 종료, 다시 시작 과정 등 수행
  - `Simplex` : 한쪽 방향으로만 전송이 가능한 방식 ex. 라디오, TV
  - `Half Duplex` : 통신의 가능 방향을 구분하는 통신 방식 중 하나로 양쪽 방향으로 신호의 전송이 가능하기는 하지만 경우에 따라 반드시 한쪽 방향으로만 전송이 이루어지게 한 방식
  - `Full Duplex` : 송신을 하면서 동시에 수신도 할 수 있는 방식
- TCP/IP 세션을 만들고 없애는 책임을 짐
- 통신하는 사용자들을 동기화하고 오류 복구 명령들을 일괄적으로 다룸
- 통신을 하기 위한 세션을 확립하고 유지하고 중단함(운영체제)

### Layer 6. 표현 계층(Presentation Layer)

<aside>
🧑🏻‍🏫 데이터 표현이 상이한 응용 프로세스의 독립성을 제공하고 암호화

</aside>

- 사용자의 명령어를 완성하고 결과를 표현함
- 코드 간의 번역을 담당하여 사용자 시스템에서 데이터의 형식상 차이를 다루는 부담을 응용 계층으로부터 덜어줌
- MIME 인코딩이나 암호화 등의 동작이 일어남
  - MIME : ASCII가 아닌 문자 인코딩을 이용해 영어가 아닌 다른 언어로 된 전자 우편을 보낼 수 있는 방식을 정의하고 그림, 음악, 영화, 컴퓨터 프로그램과 같은 8비트짜리 이진 파일을 전자 우편으로 보낼 수 있도록 함
  - ex. EBCDIC로 인코딩된 문서 파일을 ASCII로 바꿔줌
  - 해당 데이터가 어떤 형식인지(text, gif, jpg 등) 구분함

### Layer 7. 응용 계층(Application Layer)

<aside>
📱 사용자가 보는 소프트웨어의 UI, 네트워크 서비스, 사용자의 입출력(I/O) 부분 담당

**HTTP, FTP, SMTP, POP3, IMAP, Telnet** 등의 프로토콜

</aside>

- 응용 프로세스와 직접 관계하여 일반적인 응용 서비스 수행
- 일반적인 응용 서비스는 관련된 응용 프로세스들 사이의 전환 제공
  - JVM, Terminal 등

## TCP/IP

- 모든 컴퓨터가 기본으로 제공하는 인터넷 표준 프로토콜
- TCP는 데이터의 추적/제어, IP는 데이터의 주소 지정/전달 담당

### TCP(Transmission Control Protocol)

- 전송 계층에서 동작하는 프로토콜
- 전송 조절 프로토콜
- IP 위에서 동작하는 프로토콜로 데이터의 전달을 보증하고 보낸 순서대로 받게함
- 신뢰성이 높고 데이터를 안정적으로 주고받을 수 있음
- 3-way Handshaking을 통해 Connection을 형성한 후 정보의 송수신이 이루어짐
- HTTP, FTP, SMTP 등 TCP 기반 애플리케이션 프로토콜들이 IP 위에서 동작하므로 TCP/IP로 부르기도 함

### UDP(User Datagram Protocol)

- OSI 7 Layers의 전송 계층에 해당하는 프로토콜
- TCP와 자주 비교되는 프로토콜
- TCP보다 신뢰성이 떨어지지만 데이터 처리가 빠름
- 연결 설정 없이 바로 전송 가능하며 순서 제어X
- 비교적 데이터의 신뢰성이 중요하지 않을 때(ex. 실시간 방송, 온라인 게임 등) 사용
- 멀티미디어 애플리케이션, DNS, SNMP 등에서 사용

### IP(Internet Protocol)

- 인터넷 상에서 사용하는 주소체계
- 네트워크 계층에서 동작하는 프로토콜
- 패킷 통신 방식의 인터넷 프로토콜
- 패킷 전달 여부 보증 X, 패킷을 보낸 순서와 받는 순서가 다를 수 있음

### IPv4

- IP 주소 체계의 4번째 버전
- 각 덩어리마다 0-255까지 나타낼 수 있음
- 2^(32)인 약 43억 개의 IP 주소 표현 가능

### IPv6

- IPv4로 할당할 수 있는 한계를 해결하기 위해 나온 주소 체계
- 2^(128)개의 IP 주소 표현 가능

### 용도가 정해져 있는 IP 주소

- `localhost`, `127.0.0.1` : 현재 사용 중인 로컬 PC
- `0.0.0.0`, `255.255.255.255` : broadcast address, 로컬 네트워크에 접속된 모든 장치와 소통하는 주소 → 모든 기기에서 서버에 접근 가능

## Port

- IP 내에서 애플리케이션 상호 구분(프로세스 구분)을 위해 사용하는 번호
- 포트 숫자는 IP 주소가 가리키는 PC에 접속할 수 있는 통로(채널)를 의미
- 이미 사용 중인 포트는 중복하여 사용 불가
- 포트 번호는 0 ~ 65,535 까지 사용 가능
  - 0 ~ 1024번까지의 포트 번호는 주요 통신 규약에 따라 이미 정해져 있음

<aside>
💡 [localhost](http://localhost):8080 < 이런 식으로 뒤에 붙는 게 포트 번호다

</aside>

### 잘 알려진 포트 번호

- 22 : SSH
- 80 : HTTP
- 443 : HTTPS

> 잘 알려진 포트의 경우 URI에 명시X, 잘 알려지지 않은 포트는 URI에 명시O
>

## HTTP & HTTPS

### HTTP(Hyper Text Transfer Protocol)

- 클라이언트 서버 모델을 따라 데이터를 주고 받기 위한 프로토콜
- 인터넷에서 하이퍼텍스트를 교환하기 위한 통신 규약
  - 하이터텍스트 : 하이퍼링크를 통해 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
- 80번 포트 사용
- 애플리케이션 레벨의 프로토콜, TCP/IP 위에서 작동함
- 상태를 가지고 있지 않은 Stateless 프로토콜

HTTP의 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e51566da-7437-47d2-bda9-74d69cd94ba3/Untitled.png)

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkdJ4Q%2FbtqK6AXLEtC%2FjBZzMuJBWzdLYmqILo5Ri1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkdJ4Q%2FbtqK6AXLEtC%2FjBZzMuJBWzdLYmqILo5Ri1%2Fimg.png)

- 암호화가 되지 않은 평문 데이터를 전송하는 프로토콜로, 비밀번호나 주민등록번호를 주고 받으면 제 3자가 정보 조회 가능

→ HTTPS의 등장

### HTTPS(Hyper Text Transfer Protocol Secure)

- HTTP에 데이터 암호화가 추가된 프로토콜
- 네트워크 상에서 중간에 제 3자가 정보를 볼 수 없도록 암호화 지원
- 443번 포트 사용
- 암호화 방식으로 **대칭키 암호화 방식**, **비대칭키 암호화 방식** 모두 사용

### 대칭키 암호화

- 클라이언트와 서버가 동일한 키를 사용해 암호화와 복호화 진행
- 키가 노출되면 위험하지만 연산 속도가 빠름

### 비대칭키 암호화

- 1개의 쌍으로 구성된 공개키와 개인키를 암호화와 복호화에 사용
  - 공개키 : 모두에게 공개 가능한 키
  - 개인키 : 나만 가지고 있고 알고 있어야 하는 키
- 키가 노출되어도 안전하지만 연산 속도가 느림
- 공개키 암호화 : 공개키로 암호화하고 개인키로 복호화
- 개인키 암호화 : 개인키로 암호화하고 공개키로 복호화

## URL(Uniform Resource Locator)

- 자원
- 웹사이트 주소가 요청하는 구분자
- 웹 상에 서비스를 제공하는 각 서버들에 있는 파일의 위치를 표시함
  - http://milky.com/hello/test.pdf
  - [milky.com](http://milky.com) 서버에서 hello 폴더 안의 test.pdf를 요청


## URI(Uniform Resource Identifier)

- 통합 자원 식별자
- 인터넷에 있는 자원을 나타내는 유일한 주소
- 인터넷에서 요구되는 기본 조건으로서 인터넷 프로토콜에 항상 붙어다님
- URI의 하위 개념에 URL, URN 포함
- http://milky.com/hello/test.pdf?docid=111
  - docid=111 처럼 쿼리스트링의 값에 따라 결과가 달라지게 됨
    - 식별자의 역할을 함
- http://milky.com/hello/test.pdf?docid=111, http://milky.com/hello/test.pdf?docid=112는
  같은 URL을 가지고 다른 URI를 가짐

## URN(Uniform Resource Name)

- 위치와 상관 없이 리소스의 이름값을 이용해서 접근하는 방식
- 위치 정보가 아닌 실제 리소스의 이름으로 사용하는 방식

EX.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fef9b551-2dea-4901-a900-3999c6eca3a4/Untitled.png)

[https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/09/URN-예시.png?w=1082&ssl=1](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/09/URN-%E1%84%8B%E1%85%A8%E1%84%89%E1%85%B5.png?w=1082&ssl=1)

## DNS(Domain Name System)

- 웹사이트 접속 시 IP 주소 대신 도메인 이름 사용
- 입력한 도메인을 실제 네트워크 상에서 사용하는 IP 주소로 바꾸고 해당 IP 주소로 접속하는 과정이 필요한데 이러한 전제 과정, 시스템
- 계층 구조를 가지는 분산 데이터베이스 구조 가짐
  - 닷(dot, .)이 계층을 나타냄

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f97d4608-09e3-4f88-a3fa-7cc9c674717f/Untitled.png)

[https://i0.wp.com/hanamon.kr/wp-content/uploads/2022/04/도메인-네임-스페이스-Domain-Name-Space.png?resize=1080%2C980&ssl=1](https://i0.wp.com/hanamon.kr/wp-content/uploads/2022/04/%E1%84%83%E1%85%A9%E1%84%86%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%AB-%E1%84%82%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%B7-%E1%84%89%E1%85%B3%E1%84%91%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%89%E1%85%B3-Domain-Name-Space.png?resize=1080%2C980&ssl=1)

### DNS 구성 요소

1. **도메인 네임 스페이스**(Domain Name Space) → 도메인 이름 저장 분산
2. **네임 서버**(Name Server) = 권한 있는 DNS 서버
   → 해당 도메인 이름의 IP 주소 찾음
3. **리졸버**(Resolver) = 권한 없는 DNS 서버
   → 클라이언트 요청을 네임 서버로 전달하고 찾은 정보를 클라이언트에게 제공함
- ‘이 도메인 이름은 이 IP 주소이다.’ 라는 텍스트를 저장하는 데이터베이스가 필요하고, 분산된 데이터가 어디 저장되어 있는지 찾을 프로그램들이 필요하고 찾았으면 해당 IP 주소로 이동할 프로그램(브라우저)이 필요하다

### DNS 동작 과정

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ee4c2945-31c5-4efc-83e9-834b69300a35/Untitled.png)

[https://i0.wp.com/hanamon.kr/wp-content/uploads/2022/04/DNS-동작과정.png?resize=1536%2C690&ssl=1](https://i0.wp.com/hanamon.kr/wp-content/uploads/2022/04/DNS-%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8C%E1%85%A1%E1%86%A8%E1%84%80%E1%85%AA%E1%84%8C%E1%85%A5%E1%86%BC.png?resize=1536%2C690&ssl=1)

### DNS Query

- DNS 클라이언트와 DNS 서버는 DNS 쿼리를 교환함
- DNS 쿼리는 Recursive와 Iterative로 구분됨

### Recursive Query (재귀적 질의)

- 결과물(IP 주소)을 돌려주는 작업 → 웹 브라우저 등에게 돌려줌
- 응답을 돌려주는 쿼리

### Iterative Query (반복적 질의)

- Recursive DNS 서버가 다른 DNS 서버에게 쿼리를 보내 응답을 요청하는 작업
- Recursive 서버가 권한 있는 네임 서버들에게 반복적으로 쿼리를 보내서 결과물(IP 주소)을 알아냄 → 이미 IP 주소가 캐시 되어 있다면 건너뜀

### DNS 동작 과정 전체 예시

1. 웹 브라우저에 [www.milky.kr](http://www.milky.kr)을 입력
2. 웹 브라우저는 이전에 방문한 적이 있는지(캐시가 있는지) 확인함
  1. 브라우저 캐시 확인 / OS 캐시 확인 / 라우터 캐시 확인 / ISP 캐시 확인(Recursive DNS Server)
3. ISP에서 DNS Iterative하게 쿼리를 날림
  1. ISP : 개인이나 기업체에게 인터넷 접속 서비스, 웹사이트 구축 및 웹호스팅 서비스 등을 제공하는 회사
4. ISP는 Authoritative DNS 서버에서 최종적으로 IP 주소를 응답받음
  1. Authoritative DNS 서버 : 실제 개인 도메인과 IP 주소의 관계가 기록/저장/변경 되는 서버
5. ISP는 해당 주소를 캐시함
6. 웹 브라우저에게 응답함

## 로드밸런싱(Load Balancing)

- 네트워크 또는 서버에 가해지는 부하(=로드)를 분산(=밸런싱)해주는 기술
- 이 기술을 제공하는 서비스 또는 장치를 로드밸런서(Load Balancer)라고 함
  - 클라이언트와 네트워크 트래픽이 집중되는 서버들 또는 네트워크 허브 사이 위치함

  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa4f18bd-3f0c-4c51-bcb5-ffa3510da26d/Untitled.png)


[https://velog.velcdn.com/images%2Fyanghl98%2Fpost%2Ff79ca6b9-8bb8-4b0d-938a-e03a824b328d%2Fimage.png](https://velog.velcdn.com/images%2Fyanghl98%2Fpost%2Ff79ca6b9-8bb8-4b0d-938a-e03a824b328d%2Fimage.png)

- 특정 서버 또는 네트워크 허브에 부하가 집중되지 않도록 트래픽을 분산함
  → 서버나 네트워크 허브들의 성능을 최적화
- 여러 대의 서버를 두고 서비스를 제공하는 분산 처리 시스템에서 필요한 기술

트래픽 증가 시 대처할 수 있는 방법은 Scale-up, Scale-out 두 가지이다

1. `Scale-up` : 서버 자체의 성능 확장 ex. i3 → i7
2. `Scale-out` : 기존 서버와 동일하거나 낮은 성능의 서버를 두 대 이상 증설하여 운영 ex. i3 * 2
   → 여러 대의 서버로 트래픽을 균등하게 분산해주는 로드밸런싱 필요

### 로드밸런싱의 종류

- 로드밸런싱은 OS 7 Layer의 위치(L4, L7)에 따라 다름

**L4 Load Balancing**

- **전송 계층**에서 로드 분산
- **IP 주소나 포트번호, MAC 주소** 등에 따라 트래픽을 나누고 분산처리 가능
- CLB(Connection Load Balancer) / SLB(Session Load Balancer)라고 부르기도 함

**L7 Load Balancing**

- **애플리케이션 계층**에서 로드 분산
- OSI 7 Layer의 프로토콜(HTTP, SMTP, FTP 등)을 바탕으로도 분산 처리 가능

### 로드밸런싱 알고리즘

1. 라운드로빈 방식(Round Robin Method)
- 서버에 들어온 요청을 **순서대로** 돌아가며 배정하는 방식
- 여러 대의 서버가 동일한 스펙을 갖고 있고, 서버와의 연결(세션)이 오래 지속되지 않는 경우에 활용하기 적합
1. 가중 라운드로빈 방식(Weighted Round Robin Method)
- 각각의 서버마다 가중치를 매기고 **가중치가 높은 서버**에 클라이언트 요청을 **우선적**으로 배분
1. IP 해시 방식(IP Hash Method)
- 클라이언트의 **IP 주소를 특정 서버로 매핑**하여 요청을 처리하는 방식
- 사용자의 IP를 고정된 길이로 해싱해 로드를 분배하기 때문에 사용자가 항상 동일한 서버로 연결되는 것을 보장함
1. 최소 연결 방식(Least Connection Method)
- 요청이 들어온 시점에 **가장 적은 연결상태**를 보이는 서버에 우선적으로 트래픽을 배분하는 방식
- 자주 세션이 길어지거나 서버에 분배된 트래픽들이 일정하지 않은 경우 적합
1. 최소 리스폰타임(Least Response Time Method)
- 서버의 **현재 연결 상태와 응답시간**(서버에 요청을 보내고 최초 응답을 받을 때까지 소요되는 시간)을 모두 고려하여 트래픽을 배분하는 방식
- 가장 적은 연결 상태와 가장 짧은 응답시간을 보이는 서버에 우선적으로 로드를 배분함

참고 링크는 목차 순

[https://aridom.tistory.com/61](https://aridom.tistory.com/61)

[https://kotlinworld.com/326](https://kotlinworld.com/326)

[https://mangkyu.tistory.com/18](https://mangkyu.tistory.com/18)

[https://velog.io/@miscaminos/Spring-MVC-framework](https://velog.io/@miscaminos/Spring-MVC-framework)

[https://velog.io/@dbwlgns98/네트워크-CS정리-1탄](https://velog.io/@dbwlgns98/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-CS%EC%A0%95%EB%A6%AC-1%ED%83%84)

[https://onecoin-life.com/19](https://onecoin-life.com/19)

[https://linked2ev.github.io/devlog/2019/06/03/WEB-What-is-protocol/](https://linked2ev.github.io/devlog/2019/06/03/WEB-What-is-protocol/)

[https://gomu92.tistory.com/37](https://gomu92.tistory.com/37)

[https://wooody92.github.io/network/TCP-IP/](https://wooody92.github.io/network/TCP-IP/)

[https://sdesigner.tistory.com/84](https://sdesigner.tistory.com/84)

[https://velog.io/@jsj3282/TCP-흐름제어혼잡제어-오류제어](https://velog.io/@jsj3282/TCP-%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4-%EC%98%A4%EB%A5%98%EC%A0%9C%EC%96%B4)

[https://hanamon.kr/네트워크-기본-ip-주소와-포트-port/](https://hanamon.kr/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B8%B0%EB%B3%B8-ip-%EC%A3%BC%EC%86%8C%EC%99%80-%ED%8F%AC%ED%8A%B8-port/)

[https://mangkyu.tistory.com/98](https://mangkyu.tistory.com/98)

[https://wsx2792.tistory.com/14](https://wsx2792.tistory.com/14)

[https://hanamon.kr/네트워크-기본-url-uri-urn-차이점/](https://hanamon.kr/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%EA%B8%B0%EB%B3%B8-url-uri-urn-%EC%B0%A8%EC%9D%B4%EC%A0%90/)

[https://hanamon.kr/dns란-도메인-네임-시스템-개념부터-작동-방식까지/](https://hanamon.kr/dns%EB%9E%80-%EB%8F%84%EB%A9%94%EC%9D%B8-%EB%84%A4%EC%9E%84-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D%EA%B9%8C%EC%A7%80/)

[https://www.stevenjlee.net/2020/06/30/이해하기-네트워크의-부하분산-로드밸런싱-load-balancing-그/](https://www.stevenjlee.net/2020/06/30/%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%EC%9D%98-%EB%B6%80%ED%95%98%EB%B6%84%EC%82%B0-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1-load-balancing-%EA%B7%B8/)

[https://velog.io/@yanghl98/OS운영체제-로드밸런싱-Load-Balancing-정의-종류-알고리즘](https://velog.io/@yanghl98/OS%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1-Load-Balancing-%EC%A0%95%EC%9D%98-%EC%A2%85%EB%A5%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)