# What is Process

## Process

프로그램이 실행되면 프로세스 인스턴스가 생성된다. 이는 다시 말해, 프로그램 실행에 필요한 내용이 컴퓨터 메인 메모리에 올라간다는 뜻이다. 

프로그램 : 어떤 작업을 하기 위해 실행할 수 있는 파일 또는 프로그램

프로세스 ; 메모리에 적재되고 CPU 자원을 할당받아 프로그램이 실행되고 있는 상태

운영체제를 통해 여러 프로세스를 실행하고 관리할 수 있다. 이를 멀티태스킹이라 한다.

## 멀티태스킹

운영체제를 통해 CPU가 작업하는 데 필요한 자원을 프로세스 또는 스레드 간에 나누는 것을 말한다. 이를 통해 여러 응용 프로그램을 동시에 실행할 수 있다는 장점이 있다.

예를 들어, 지금 내가 이렇게 화면 창을 2개 띄워서 내용을 정리하고, 웹 서핑을 하고 유투브로 노래를 들을 수 있는 게 모두 멀티 태스킹 때문이다.

사용자는 앞서 말한 모든 상황이 동시에 처리되는 것으로 느끼지만, 실제로는 CPU는 한 번에 1개의 명령만 처리할 수 있다. 즉, 동시가 아닌 빠른 속도로 프로세스를 번걸아가며 실행하고 있는 것이다. 이를 컨텍스트 스위칭이라 한다.

- 컨텍스트 스위칭

하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태를 보관하고 새로운 프로세스의 상태를 적재하는 작업을 말한다.

쉽게 말하면, 현재 작업 중이던 프로세스의 상태 정보를 어딘가에 저장하고, 새로운 프로세스의 상태 정보를 메모리에 올려 CPU를 통해 실행하도록 하는 것이다.

컨텍스트 스위칭은 운영체제의 CPU 자원을 할당하는 스케쥴러에 의해 발생한다. CPU를 적절하고 효율적으로 사용할 수 있도록 하는 작업을 스케쥴링이라 한다.

- CPU 스케쥴러

스케쥴러는 READY 큐에 존재하는 프로세스들을 특정한  우선순위를 기반으로 CPU를 할당 받게 해주는 역할을 한다. 

스케쥴러는
1. CPU 최대한 활용하기
2. 대기 시간 최소화하기
3. 처리량 최소화하기
위의 3가지 목적을 갖는다.

스케쥴링을 구현하기 위해서는 자료구조의 구현이 따른다. 대표적으로 Linked List, Hash List, Bitmap, Red-Black Tree 등이 있다.

그리고 알고리즘에 따라서도 스케쥴링 종류가 달라진다. 

1. FCFS : First Come, First Serve 말 그대로 선착순 알고리즘이다.
2. SJF : Shorted Job First 최단 작업을 우선으로 처리한다.
3. Priority Scheduling : 우선 순위에 따라 처리한다.
4. RR : Round Robin 정해진 시간에 주어진 만큼 프로세스를 할당한 뒤 작업이 끝난 프로세스는 READY 큐 가장 마지막에 가서 재할당을 기다린다.
5. MulitLevel-Queue : READY 큐를 여러 개의 큐로 분류하여 각 큐가 각각 다른 스케쥴링 알고리즘을 가지는 방식이다.
6. MultiLevel-Feedback-Queue : MulitLevel-Queue는 특정 프로세스가 큐에 고정되어 있지만, MultiLevel-Feedback-Queue는 큐와 큐 사이에 프로세스가 이동하는 것을 허용한다.

## PCB

Process Control Block은 특정한 프로세스를 관리할 필요가 있는 정보를 포함하는 운영체제 커널의 자료구조이다.

PCB가 필요한 이유는 컨텍스트 스위칭 때문이다. PCB에는 아래와 같은 정보가 포함된다.

- 프로세스 식별자
- 프로세스 상태
- Program Counter
- CPU Register , Register
- CPU 스케쥴링 정보
- 메모리 관리 정보
- 프로세스 계정 정보
- 입출력 상태 정보

## 프로세스 상태

- new : 프로세스가 생성되는 상태
- ready : 프로세스가 CPU에 할당되어, 처리되기를 기다리는 상태
- running : 프로세스가 CPU에 할당되어, 명령어들이 실행되는 상태
- waiting : 어떤 이벤트의 발생으로 인해 프로세스가 기다리는 상태
- terminated : 프로세스가 종료되는 상태

컨텍스트 스위칭도 메모리에 I/O를 하는 작업이기 때문에 실행되는 프로세스의 수가 많거나, 빈번하게 발생할 경우 오버헤드를 발생시켜 성능 저하 결과를 가져온다.

## 메모리 영역

하나의 프로세스는 각각 독립된 메모리 영역을 할당 받는다.

- 코드 영역

실행할 프로그램의 코드 및 매크로 상수가 기계어 형태로 저장되는 영역이다. CPU는 여기에 저장된 인스트럭션을 하나씩 처리한다.

- 데이터 영역

코드에서 선언한 전역 변수와 정적 변수가 저장되는 영역이다. 프로그램의 시작과 함께 할당되어 종료될 때 소멸된다.

- 스택 영역

함수 안에서 선언된 지역변수, 매개변수, 리턴 값 등이 저장되고 함수 호출시 기록하고 종료되면 제거한다. 

- 힙 영역

관리가 가능한 데이터 이외의 다른 형태의 데이터를 관리하기 위한 공간이다. 동적 메모리 할당 공간이므로 사용이 끝나면 운영체제가 쓸 수 있도록 반납해야 한다. 동적 메모리 할당은 어느 시점에 어느 정도의 공간을 할당할 수 있을 지 정확하게 예측할 수 없으므로, 런타임에 확인 가능하다.

## IPC

Inter-Process Communication

각 프로세스는 별도의 공간에서 실행되기 때문에, 한 프로세스에서 다른 프로세스의 메모리 영역에 접근할 수 없다. 만약 프로세스가 다른 프로세스 자원에 접근하려면 IPC를 사용해야 한다.

IPC는 운영체제 상에서 실행 중인 프로세스 간에 정보를 주고 받는 것을 말한다. 프로세스는 자신에게 할당된 메모리 내의 정보만 접근할 수 있는데, 이는 안전성을 위해 운영체제에서 자기 프로세스의 메모리만 접근하도록 강제하고 있다. 

https://charlezz.medium.com/process%EC%99%80-thread-%EC%9D%B4%EC%95%BC%EA%B8%B0-5b96d0d43e37