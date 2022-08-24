# Process VS Thread 
### Process VS Thread 
> 프로세스 : 프로그램을 메모리 상에서 **실행 중인 작업** </br>
스레드 : 프로세스 안에서 실행되는 여러 **흐름 단위**

> 프로세스: 운영체제로부터 자원을 할당받은 **작업**의 단위 </br>
스레드: 프로세스가 할당받은 자원을 이용하는 **실행 흐름**의 단위

### Process란?
- 프로세스(process)란 실행 중에 있는 프로그램(Program)을 의미
 >프로그램: 어떤 작업을 하기 위해 실행할 수 있는 파일 또는 프로그램</br>
프로세스: 메모리에 적재되고 CPU 자원을 할당받아 프로그램이 실행되고 있는 상태 
- 스케줄링의 대상이 되는 작업(task)과 같은 의미로 쓰임
- 프로세스 내부에는 최소 하나의 스레드(thread)를 가지고있는데, 실제로는 스레드(thread)단위로 스케줄링
- 하드디스크에 있는 프로그램을 실행하면, 실행을 위해서 메모리 할당이 이루어지고, 할당된 메모리 공간으로 바이너리 코드가 올라가게 되며 이 순간부터 프로세스라 불림 https://blockdmask.tistory.com/22 [개발자 지망생:티스토리]

### Process 메모리 구조
- Code 영역 : 프로그램을 실행시키는 실행 파일 내의 명령어들이 올라가는 곳, 코드 자체를 구성
- Data 영역 : 전역변수, 정적변수, 배열 등 (초기화 된 데이터는 data 영역에
,초기화 되지 않은 데이터는 bss 영역에 저장)
- Heap 영역 : 동적할당을 위한 메모리 영역(new(), malloc() 등)
- Stack 영역 : 지역변수, 매개변수, 리턴 값 (임시 메모리 영역)
> cf) 스레드는 Stack만 따로 할당 받고 나머지 영역은 서로 공유   </br>
-> 프로세스는 자신만의 고유 공간과 자원을 할당받아 사용하는데 반해, 스레드는 다른 스레드와 공간, 자원을 공유하면서 사용 </br>
-> 만약 한 프로세스를 실행하다가 오류가 발생해서 프로세스가 강제로 종료된다면, 다른 프로세스에게 어떤 영향이 있을까?  </br>
공유하고 있는 파일을 손상시키는 경우가 아니라면 아무런 영향을 주지 x</br>
스레드의 경우는? </br>
스레드는 Code/Data/Heap 메모리 영역의 내용을 공유하기 때문에 어떤 스레드 하나에서 오류가 발생한다면 같은 프로세스 내의 다른 스레드 모두가 강제로 종료

### 멀티 태스킹
OS를 통해 CPU가 작업하는데 필요한 자원(시간)을 프로세스 또는 스레드간에 나누는 행위 </br>
이를 통해 여러 응용 프로그램을 동시에 열고 작업 가능
- Context Switching(문맥전환): Context Switching이란 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태를 보관하고 새로운 프로세스의 상태를 적재하는 작업 </br>
운영체제의 CPU자원을 할당하는 스케줄러(Scheuler)에 의해 발생 </br>
CPU를 적절하고 효율적으로 사용할 수 있도록 하는 작업 - 스케줄링

### CPU Scheduler
스케줄러(Scheduler)는 레디 큐에 존재하는 프로세스들을 특정한 우선순위를 기반으로 CPU를 할당받게 해주는 역할
- 목표
    1. Batch System: 가능하면 많은 일을 수행. 시간(time) 보단 처리량(throughout)이 중요
    2. Interactive System: 빠른 응답 시간. 적은 대기 시간.
    3. Real-time System: 기한(deadline) 맞추기.
- 종류
    - 선점 (preemptive) : OS가 CPU의 사용권을 선점할 수 있는 경우, 강제 회수하는 경우 (처리시간 예측 어려움)
        1. Priority Scheduling
    </br>정적/동적으로 우선순위를 부여하여 우선순위가 높은 순서대로 처리
    </br>우선 순위가 낮은 프로세스가 무한정 기다리는 Starvation 이 생길 수 있음
    </br>Aging 방법으로 Starvation 문제 해결 가능
        2. Round Robin
    </br>FCFS에 의해 프로세스들이 보내지면 각 프로세스는 동일한 시간의 Time Quantum 만큼 CPU를 할달 받음
    </br>Time Quantum or Time Slice : 실행의 최소 단위 시간
    </br>할당 시간(Time Quantum)이 크면 FCFS와 같게 되고, 작으면 문맥 교환 (Context Switching) 잦아져서 오버헤드 증가
        3. Multilevel-Queue (다단계 큐)
    </br>작업들을 여러 종류의 그룹으로 나누어 여러 개의 큐를 이용하는 기법
    </br>우선순위가 낮은 큐들이 실행 못하는 걸 방지하고자 각 큐마다 다른 Time Quantum을 설정 해주는 방식 사용
    </br>우선순위가 높은 큐는 작은 Time Quantum 할당. 우선순위가 낮은 큐는 큰 Time Quantum 할당.
        4. Multilevel-Feedback-Queue (다단계 피드백 큐)
    </br>다단계 큐에서 자신의 Time Quantum을 다 채운 프로세스는 밑으로 내려가고 자신의 Time Quantum을 다 채우지 못한 프로세스는 원래 큐 그대로
    </br>Time Quantum을 다 채운 프로세스는 CPU burst 프로세스로 판단하기 때문
    </br>짧은 작업에 유리, 입출력 위주(Interrupt가 잦은) 작업에 우선권을 줌
    </br>처리 시간이 짧은 프로세스를 먼저 처리하기 때문에 Turnaround 평균 시간을 줄옂줌
    - 비선점 (nonpreemptive) : 프로세스 종료 or I/O 등의 이벤트가 있을 때까지 실행 보장 (처리시간 예측 용이함)
        1. FCFS (First Come First Served)
    </br>큐에 도착한 순서대로 CPU 할당
    </br>실행 시간이 짧은 게 뒤로 가면 평균 대기 시간이 길어짐
        2. SJF (Shortest Job First)
    </br>수행시간이 가장 짧다고 판단되는 작업을 먼저 수행
    </br>FCFS 보다 평균 대기 시간 감소, 짧은 작업에 유리
        3. HRN (Hightest Response-ratio Next)
    </br>우선순위를 계산하여 점유 불평등을 보완한 방법(SJF의 단점 보완)
    </br>우선순위 = (대기시간 + 실행시간) / (실행시간)

### Thread란?
- 실제로 상태변화를 하거나 컴퓨터에서 task로 사용하는 단위는 
- 스레드(Thread)는 프로세스 내부의 작업의 흐름, 단위
- 스레드(thread)는 한 프로세스(process)내부에 적어도 하나 존재

### 멀티스레드(Multitread) 
 하나의 응용 프로그램에서 여러 스레드를 구성해 각 스레드가 하나의 작업을 처리하는 것</br> 
 각 스레드끼리는 프로세스의 일정 메모리 영역을 공유해 다수의 작업을 동시에 처리하도록 해줌</br> 
- Context-Switching할 때 공유하고 있는 메모리만큼의 메모리 자원을 아낄 수 있음
- 스레드는 프로세스 내의 Stack 영역을 제외한 모든 메모리를 공유하기 때문에 통신의 부담이 적어서 응답 시간이 빠름
- 스레드 하나가 프로세스 내 자원을 망쳐버린다면 모든 프로세스가 종료될 수 있음
- 자원을 공유하기 때문에 필연적으로 동기화 문제가 발생할 수밖에 없음
> **동기화 문제(Synchronization Issue)** 란? </br>
여러 스레드가 함께 전역 변수를 사용할 경우 발생할 수 있는 충돌 (멀티스레드를 사용하면 각각의 스레드 중 어떤 것이 어떤 순서로 실행될 지 그 순서를 알 수 없다. 만약 A 스레드가 어떤 자원을 사용하다가 B 스레드로 제어권이 넘어간 후 B 스레드가 해당 자원을 수정했을 때, 다시 제어권을 받은 A 스레드가 해당 자원에 접근하지 못하거나 바뀐 자원에 접근하게 되는 오류가 발생할 수 있다.)</br>

> 멀티스레드의 안전성에 대한 단점은 Critical Section 기법을 통해 대비함(
하나의 스레드가 공유 데이터 값을 변경하는 시점에 다른 스레드가 그 값을 읽으려할 때 발생하는 문제를 해결하기 위한 동기화 과정)

### 멀티프로세스
하나의 컴퓨터에 여러 CPU 장착 → 하나 이상의 프로세스들을 동시에 처리(병렬)
- 안전성 (메모리 침범 문제를 OS 차원에서 해결)
- 각각 독립된 메모리 영역을 갖고 있어, 작업량 많을 수록 오버헤드 발생. 
- Context Switching으로 인한 성능 저하

## 참고 자료
https://blockdmask.tistory.com/22 </br>
https://velog.io/@raejoonee/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98-%EC%B0%A8%EC%9D%B4</br>
https://charlezz.medium.com/process%EC%99%80-thread-%EC%9D%B4%EC%95%BC%EA%B8%B0-5b96d0d43e37</br>
https://gyoogle.dev/blog/computer-science/operating-system/Process%20vs%20Thread.html</br>
https://gyoogle.dev/blog/computer-science/operating-system/CPU%20Scheduling.html