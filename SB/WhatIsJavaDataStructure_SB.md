# 자바 자료구조

## Collection

- 객체의 그룹을 조작하고 저장할 수 있는 자료구조를 표현하는 인터페이스
- Iterator 인터페이스를 상속한 인터페이스

![https://static.javatpoint.com/images/java-collection-hierarchy.png](https://static.javatpoint.com/images/java-collection-hierarchy.png)

## List

- **순서(인덱스)**를 가지는 원소들의 모임
- **중복값**을 가질 수 있는 자료구조
- ArrayList, LinkedList, Vector 등

### ArrayList

- 배열과 사용법이 유사하게 데이터들을 인덱스로 접근하는 클래스
- 초기에 크기를 정해줘야하는 배열과 달리 저장되는 데이터의 갯수에 따라 유동적으로 크기가 변경된다는 장점이 있음
- 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 성능이 뛰어남

### LinkedList

- 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용함
- 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰임

<aside>
👩🏻‍💻 **LinkedList를 사용하는 이유?**

ArrayList에서 빈번하게 중간에서 데이터의 삽입, 삭제가 발생할 경우 인덱스 조정을 위해 그 뒤의 원소들을 하나씩 옮겨줘야 하는 작업이 번거로울 수 있는데, LinkedList를 사용하면 데이터가 삽입되고 삭제될 때 바로 앞에 있는 데이터의 링크 값만 변경해주면 되기 때문에 성능이 더 좋음

**ArrayList vs LinkedList**
인덱스를 가지고 데이터에 접근하는 연산은 ArrayList의 성능이 더 좋고
중간에 데이터의 삽입, 삭제가 빈번히 일어나는 경우에는 LinkedList의 성능이 더 좋음

</aside>

### Vector

- 과거에 대용량 처리를 위해 사용함
- 내부에서 자동으로 동기화처리가 일어나 비교적으로 성능이 좋지 않고 무거워 잘 쓰이지 않음

## Queue

- 데이터를 처리하기 전에 잠시 저장하고 있는 FIFO(First In First Out)의 자료구조
- Tail에서 데이터 추가, Head에서 데이터 삭제
- Queue, PriorityQueue, ArrayDeque 등

## Set

- 데이터 간에 **순서가 없고 중복값을 허용하지 않음**
- HashSet, TreeSet, LinkedHashSet 등

### HashSet

- 해쉬 테이블에 데이터 저장
- 성능 면에서 가장 우수함 (속도 빠름)
- 순서를 예측할 수 없음

### TreeSet

- 레드-블랙 트리에 데이터 저장
- 정렬 방법을 지정할 수 있음
- HashSet보다는 성능이 느림

### LinkedHashSet

- HashSet의 문제점인 순서의 불명확성의 단점 제거

## Map

- Collection 인터페이스와 별개이지만 json 형식과 유사한 key-value 자료구조로 자주 쓰이는 구현체
- 사전(Dictionary)과 같은 자료구조
- 순서가 없고, 키는 유일성을 가지며 값의 중복은 허용함
- Hashtable, HashHashMap, TreeMap, LinkedHashMap 등

### HashTable

- HashMap 보다는 느리지만 동기화 지원
- null 값이 올 수 없음

### HashMap

- 해싱 테이블에 데이터 저장
- 중복과 순서가 허용되지 않음
- null 값이 올 수 있음

### TreeMap

- 탐색 트리에 데이터 저장
- 검색이 빠름

[https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ppuagirls&logNo=221560996691](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ppuagirls&logNo=221560996691)

[https://bangu4.tistory.com/194](https://bangu4.tistory.com/194)