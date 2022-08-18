# What is collection structure of java

> 목차
> * Collections
> * List
> * Set
> * Map

## 1. Collections

Collection는 데이터의 그룹 집합체라는 의미를 갖는다.

Collection 프레임워크에 속하는 클래스를 지원해주는 다양한 메소드가 있다.

* sort(List l)
* sort(List l, reverseOrder())
* max(List l)
* min(List l)
* shuffle(List l) // 랜덤으로 섞기
* binarySearch(List l, Key) // 오름차순 정렬 리스트에서 이진 검색을 통해 위치 반환
* disjoint(List l, List l2) // 두 리스트의 값이 완전히 다른지 검사

Collection은 크게 List, Set, Map으로 나눌 수 있다. 그 중 List, Set은 Collection 인터페이스를 상속한다. 

## 2. List

index라는 식별자로 순서를 가지며, 데이터의 중복을 허용하는 자료구조이다.

* ArrayList

단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 성능이 뛰어나다.

배열과 달리 초기 크기를 지정하지 않아도 되므로, 삽입과 삭제가 자유롭다.

* LinkedList

양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용하다.

스택, 큐, 양방향 큐 등을 만들기 위한 용도로 씅니다. 

* Vector

ArrayList와 동일한 구조를 가지며 배열의 크기가 늘어나고 줄어듬에 따라 자동으로 크기가 조절이 된다. Vector는 항상 동기화되어 있어 스레드가 아닌 환경에서는 거의 사용되고 있지 않다. 또한, 항상 동기화되므로 스레드 환경에서 안정성은 높지만 ArrayList와 비교하여 추가, 검색, 삭제의 성능은 떨어지는 단점이 있다.

[Vector 예제 포함](https://crazykim2.tistory.com/570)

## 3. Set

순서가 없고, 데이터 중복을 허용하지 않는 자료구조이다.

* HashSet

가장 빠른 임의 접근 속도를 지녔다.

순서를 예측할 수 없다.

* TreeSet

정렬 방법을 지정할 수 있다.

## 4. Map

key-value 로 이루어진 자료구조이다.

순서가 없고, key는 유일성을 가지고 value의 중복은 허용한다.

* HashTable

HashMap 보다는 느리지만 동기화를 지원한다. 

단, null 값은 불가이다.

* HashMap

중복과 순서가 허용되지 않으며 null 값이 올 수 있다.

* TreeMap

정렬된 순서대로 key와 value를 저장하여 검색이 빠르다.

[자바의 자료구조를 한 번에 정리](https://bangu4.tistory.com/194)