# Java Data Structure

### 자료구조란?
> 데이터 단위와 데이터 자체 사이의 물리적 또는 논리적인 관계 </br>
데이터 단위란? 데이터를 구성하는 한 단위 -> 자료구조 : 자료를 효율적으로 이용할 수 있도록 저장하는 방법

### 선형 자료구조 : 한 줄로 자료를 관리

- 배열 (Array) : 선형으로 자료를 관리, 정해진 크기의 메모리를 먼저 할당받아 사용하고, 자료의 물리적 위치와 논리적 위치가 같음, jdk 클래스 : ArrayList, Vector
- 연결 리스트 (LinkedList) : 선형으로 자료를 관리, 자료가 추가될 때마다 메모리를 할당 받고, 자료는 링크로 연결됨. 자료의 물리적 위치와 논리적 위치가 다를 수 있음, jdk 클래스 : LinkedList
- 스택 (Stack) : 가장 나중에 입력 된 자료가 가장 먼저 출력되는 자료 구조 (Last In First OUt), jdk 클래스 : Stack
- 큐 (Queue) : 가장 먼저 입력 된 자료가 가장 먼저 출력되는 자료 구조 (First In First Out), jdk 클래스 : ArrayList

### 트리 (Tree) : 부모 노드와 자식 노드간의 연결로 이루어진 자료 구조
- 힙(heap) : Priority queue를 구현 (우선 큐) -> 우선순위 큐는 우선순위가 높은 순으로 꺼내므로 힙을 이용해서 구현 (최대힙이 부모노드가 자식노드보다 항상 크기떄문 반대인 최소힙으로도 가능.)
  - Max heap : 부모 노드는 자식 노드보다 항상 크거나 같은 값을 갖는 경우
  - Min heap : 부모 노드는 자식 노드보다 항상 작거나 같은 값을 갖는 경우
  - heap정렬에 활용 할 수 있음
- 이진 트리 (binary tree) : 부모노드에 자식노드가 두 개 이하인 트리
- 이진 검색 트리 (binary search tree
- jdk 클래스 : TreeSet, TreeMap (Tree로 시작되는 클래스는 정렬을 지원 함)

### 그래프 (Graph) : 정점과 간선들의 유한 집합 G = (V,E)
-> 네비게이션, 구글맵 이런게 다 그래프에 기반해서 구현.
- 정점(vertex) : 여러 특성을 가지는 객체, 노드(node)
- 간선(edge) : 이 객체들을 연결 관계를 나타냄. 링크(link)
- 간선은 방향성이 있는 경우와 없는 경우가 있음(노드와 노드 연결하는걸 edge나 link)
- 그래프를 구현하는 방법 : 인접 행렬(adjacency matrix), 인접 리스트(adjacency list)
- 그래프를 탐색하는 방법 : BFS(bread first search), DFS(depth first search)

### 해싱 (Hashing) : 자료를 검색하기 위한 자료 구조
- 검색을 위한 자료 구조
- 키(key)에 대한 자료를 검색하기 위한 사전(dictionary) 개념의 자료 구조
- key는 유일하고 이에 대한 value를 쌍으로 저장
- index = h(key) : 해시 함수가 key에 대한 인덱스를 반환해줌 해당 인덱스 위치에 자료를 저장하거나 검색하게 됨
- 해시 함수에 의해 인덱스 연산이 산술적으로 가능 O(1)
- 저장되는 메모리 구조를 해시테이블이라 함
- jdk 클래스 : HashMap, Properties
- 키는 중복 되지 않음. 

### 컬레션 프레임워크
- 프로그램 구현에 필요한 자료구조(Data Structure)를 구현해 놓은 JDK 라이브러리
- java.util 패키지에 구현되어 있음
- 개발에 소요되는 시간을 절약하면서 최적화 된 알고리즘을 사용할 수 있음
- 여러 구현 클래스와 인터페이스의 활용에 대한 이해가 필요함

#### List 
- 객체를 순서에 따라 저장하고 관리하는데 필요한 메서드가 선언된 인터페이스
- 자료구조 리스트 (배열, 연결리스트)의 구현을 위한 인터페이스
- 중복을 허용함
- ArrayList, Vector, LinkedList, Stack, Queue 등…

#### Set 
- 순서와 관계없이 중복을 허용하지 않고 유일한 값을 관리하는데 필요한 메서드가 선언됨
- 아이디, 주민번호, 사번등을 관리하는데 유용
- 저장된 순서와 출력되는 순서는 다를 수 있음
- HashSet, TreeSet등…

#### Queue
- 맨 앞(front) 에서 자료를 꺼내거나 삭제하고, 맨 뒤(rear)에서 자료를 추가 함
- Fist In First Out (선입선출) 구조
- 일상 생활에서 일렬로 줄 서 있는 모양
- 순차적으로 입력된 자료를 순서대로 처리하는데 많이 사용 되는 자료구조
- 콜센터에 들어온 문의 전화, 메세지 큐 등에 활용됨

### Map 인터페이스
- 쌍(pair)로 이루어진 객체를 관리하는데 사용하는 메서드들이 선언된 인터페이스
- 객체는 key-value의 쌍으로 이루어짐
- key는 중복을 허용하지 않음
- HashTable, HashMap, Properties, TreeMap 등이 Map 인터페이스를 구현 함

## 참고자료
https://soliloquiess.github.io/study/2021/03/20/java_%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0.html</br>
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ppuagirls&logNo=221560996691