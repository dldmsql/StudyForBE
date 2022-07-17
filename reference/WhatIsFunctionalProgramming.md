# What is Functional Programming

> 목차
> * lambda Expression with memory 
> * Why to use functional programming

## 1. Lambda Expression with memory 

1. What is Lambda Expression

Stream 연산들은 매개변수로 함수형 인터페이스를 받도록 되어있다. 그리고 Lambda Expression은 반환값으로 함수형 인터페이스를 반환하고 있다.
> Stream은 JAVA 8에서 추가된 것으로, Lambda를 활용할 수 있는 기술 중 하나이다. <br/>
> Stream은 데이터의 흐름으로, 배열 또는 컬렉션 인스턴스에 함수 여러 개를 조합해서 원하는 결과를 얻을 수 있다.<br/>
> Strema은 병렬 처리가 가능하다. 하나의 작업을 둘 이상의 작업으로 잘게 나눠서 동시에 진행하는 것을 병렬 처리라 하는데, Thread를 이용해 많은 요소들을 빠르게 처리할 수 있다.

Lambda Expression은 함수를 하나의 식(Expression)으로 표현한 것을 말한다. 함수를 람다식으로 표현하면 메소드의 이름이 필요 없기 때문에, 람다식은 익명 함수의 한 종류라고 볼 수 있다.

람다식이 필요한 이유는 불필요한 코드를 줄이고, 가독성을 높이기 위함이다. 때문에 람다식에서는 메소드의 이름이 불필요해서 사용하지 않는다. 대신 컴파일러가 문맥을 살펴 타입을 추론한다. 

정리하면, 람다식의 특징은 1) 람다식 내에서 사용되는 지역변수는 final이 붙지 않아도 상수로 간주되며 2) 람다식으로 선언된 변수명은 다른 변수명과 중복될 수 없다는 것이 있다.

람다식의 장점으로는 앞서 Strema에서 설명한 특징과 필요성을 생각하면 된다.

람다식의 단점으로는 람다를 사용하면서 만든 익명함수는 재사용을 할 수 없으며, 디버깅이 어렵다는 점이다. 그리고 람다를 남발하면 되려 비슷한 함수가 중복 생성되어 코드가 지저분해질 수 있고, 재귀로 만들 경우 부적합하다.

> 왜 재귀로 만들 경우 부적합하지??


2. What is Functional Interface 

Java는 기본적으로 객체지향 언어이기 때문에 람다식으로 정의하는 순수 함수와 그렇지 않은 일반 함수를 다르게 취급한다. 이 둘을 구분하기 위해 등장한 것이 함수형 인터페이스이다.
> 순수함수란?<br/>
> 동일한 입력에 대해 항상 동일한 출력을 반환하는 함수를 말한다.<br/>
> 외부의 상태를 변경하거나 영향을 받지 않는 함수를 말한다.

함수형 인터페이스란 함수를 1급 객체처럼 다룰 수 있게 해주는 어노테이션으로, 인터페이스에 선언하여 단 하나의 추상 메소드만을 갖도록 제한하는 역할을 한다. 
> 1급 객체란?<br/>
> First-class Object는 변수에 할당할 수 있으며, 다른 함수를 인자로 전달 받고, 다른 함수의 결과로서 리턴될 수 있다.<br/>
> JS에서 const a = fun (num) { return num * num ;}을 떠올리자

Java에서 제공하는 함수형 인터페이스 4개는 다음과 같다.
* Supplier<T>
매개변수 없이 반환값만을 갖는 함수형 인터페이스이다.
```` bash
Supplier<String> supplier = () -> "Hi, there!";
System.out.print(supplier.get());
````
* Consumer<T>
객체 T를 매개변수로 받아서 사용하며, 반환값은 없는 함수형 인터페이스이다.
`andThen()`함수를 통해 하나의 함수가 끝난 후 다음 Consumer를 연쇄적으로 이용할 수 있다.
```` bash
Consumer<String> consumer = (str) -> System.out.print(str.split(" ")[0]);
consumer.andThen(System.out::print).accept("Hi, there!!");

// Output
Hi,
Hi, there!
````
* Function<T, R>
객체 T를 매개변수로 받아서 처리한 후 R로 반환하는 함수형 인터페이스다.
```` bash
Function<String, Integer> function = str -> str.length();
function.apply("Hi, there!!");

// Output
10
````
* Predicate<T>
객체 T를 매개변수로 받아 처리한 후, Boolean을 반환한다.
````bash
Predicate<String> predicate = (str) -> str.equals("Hi, there!!");
predicate.test("Hello World");

// Output
false
````
이 인터페이스는 그루터기 코드에서도 사용한 적이 있다.

[망나니개발자](https://mangkyu.tistory.com/113)<br/>
[Stream 정리 잘되어 있는 블로그](https://futurecreator.github.io/2018/08/26/java-8-streams/)<br/>
[1급 객체가 무엇인가](https://velog.io/@reveloper-1311/%EC%9D%BC%EA%B8%89-%EA%B0%9D%EC%B2%B4First-Class-Object%EB%9E%80)

3. What is JVM

Java Virtual Machine은 자바와 운영체제 사이에서 중개자 역할을 수행하며, 자바가 운영체제에 구애받지 않고 프로그램을 실행할 수 있도록 도와준다. 또한, GC를 사용하여 메모리를 자동으로 관리하며, 다른 하드웨어와 다르게 Stack 기반으로 동작한다.

**[JAVA 프로그램 실행 단계]**<br/>
자바 컴파일러에 의해 자바 소스 파일은 바이트 코드로 변환된다. 변환된 바이트 코드를 JVM에서 읽어 들인 뒤, 운영체제에 구애 받지 않고 실행할 수 있도록 한다.

**JVM 메모리구조**<br/>
JVM은 Garbage Collector, Execution Engine, Class Loader, Runtime Data Area로 구성되어 있다.

1. 자바 소스 파일을 자바 컴파일러가 Class 파일로 변환한다.
2. Class Loader가 Class 파일을 읽어들이면서 링크를 통해 배치하는 작업을 수행한다. ( 런타임 시에 동적으로 클래스를 로드한다. )
3. Execution Engine은 Class Loader를 통해 JVM 내의 Runtime Data Area에 배치된 바이트 코드들을 명령어 단위로 읽어서 실행한다. ( JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준이 넘어감녀 JIT 컴파일러 방식으로 실행한다. )
> JIT 컴파일러 방식 : 바이트 코드를 어셈블리어 같은 네이티브 코드로 바꿈으로써 실행이 빠르다.<br/>
> 인터프리터 방식 : 읽고 해석하고 실행한다.
4. Garbage Collector는 Heap 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들을 탐색 후 제거하는 역할을 한다. 
5. Runtime Data Area는 JVM의 메모리 영역으로 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역이다. Method, Heap, Stack, PC Register, Native Method Stack으로 나눌 수 있다.

**Runtime Data Area**<br/>
1. Method 

모든 Thread가 공유하는 메모리 영역이다. 클래스, 인터페이스, 메소드, 필드, static 변수 등의 바이트 코드를 보관한다.

2. Heap

모든 Thread가 공유하는 메모리 영역이다. New 키워드로 생성된 객체와 배열이 생성되는 영역이다. method 영역에 로드된 클래스만 생성이 가능하고, GC가 참조되지 않는 메모릴 확인하고 제거하는 영역이다.

3. Stack

메소드 호출 시마다 각각의 Stack Frame을 생성한다. 메소드 안에서 사용되는 값들을 저장하고, 호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값을 임시로 저장한다. 메소드 수행이 끝나면 Frame별로 삭제한다. 

4. PC Register

Thread가 시작될 때 생성되며, 생성될 때마다 함께 생성되는 공간이다. 현재 수행 중인 JVM 명령의 주소를 갖고 있다.

5. Native Method Stack

자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역이다.

[JVM 정리 잘했다.](https://steady-coding.tistory.com/305)

4. Using Regional Variables in Lambda Expression

람다식에서 매개변수나 지역변수의 값을 변경하면 컴파일 에러가 발생한다. 그 이유는 람다식에서 사용되는 변수가 final은 아니지만, final의 속성을 가지기 때문에 값의 재할당이 일어나면 안된다.

람다식의 매개변수가 아닌 외부에서 정의된 변수를 **자유변수**라고 부른다. 람다식 내부에서 자유변수를 참조하는 행위를 **람다캡처링**이라고 한다. 이때, 자유변수가 인스턴스변수이거나 정적 변수일 경우 캡처링을 원할하게 수행할 수 있지만 지역변수일 경우 문제가 발생할 수 있다.

**왜 그럴까??** <br/>
지역변수는 JVM의 Runtime Data Area 중에서도 Stack 영역에 저장되어 있다. 람다는 이 지역변수를 Stack 영역에 직접 접근 하는 것이 아니라 지역변수를 자신의 Stack 영역에 복사하여 사용한다.
> 람다식이 동작하고 있는 Thread는 자신만의 Stack영역을 갖는다.<br/>

때문에 지역변수가 존재하는 Thread가 사라져도 람다는 복사된 값을 참조하기에 에러가 발생하지 않는다. ( 원본이 사라져도 복사한 값을 참조해서 걱정이 없다! )

만약, 멀티 쓰레드 환경에서 여러 개의 쓰레드가 동일한 람다식을 사용한다고 가정해보자. 여러 개의 쓰레드가 람다 캡처링을 하는데, 자유 변수의 값이 계속해서 변한다면 동기화 문제가 발생하게 된다. 때문에 자유변수로 지역변수를 택할 경우, 그 변수는 final 속성을 지녀야 한다.
> 다시 설명하지만, final 속성은 값의 재할당이 일어나면 안되는 것이다.

지역변수가 아닌 인스턴스 변수나 정적 변수를 자유변수로 선택한다면 ?? 이 두 변수는 JVM의 Runtime Data Area 중에서 Heap 영역에 저장된다. 람다식을 사용하는 모든 쓰레드들은 Heap 영역을 공유하기 때문에, 항상 두 변수를 참조할 수 있다. 따라서 자유변수로 인스턴스 변수나 정적 변수를 택할 경우, final 속성을 가질 필요가 없다.

[람다식과 지역변수 with JVM](https://steady-coding.tistory.com/306)

## 2. Why to use functional programming

함수형 프로그래밍은 하나의 패러다임으로, 자료 처리를 수학적 함수의 계산으로 취급하고 상태와 가변 데이터를 멀리하는 프로그래밍 패러다임이다.

함수형 프로그래밍의 특징으로는 1) 순수함수, 2) Stateless, Immutability, 3) 선언형 함수, 4) 일급 객체와 고차함수가 있다.

앞서 설명한 순수함수는 프로그램의 변화 없이 입력 값에 대한 결과를 예상할 수 있어 테스트가 용이하다.
> fun add(a,b) { return a+b; }<br/>
> 함수는 입력과 그에 따른 출력이 있을 뿐이다.

Stateless, Immutability는 데이터의 상태가 변하지 않는 불변성을 유지해야 한다는 특성이다. 그 이유에 대해서는 위에서 JVM을 통해 설명했다.

선언형 함수는 무엇을 할 것인가에 주목한다. if, switch-case, for등의 명령어를 사용하지 않고 함수형 코드를 사용한다.

일급 객체는 위에서 설명하였다. 변수에 할당할 수 있으며, 다른 함수를 인자로 전달 받고, 다른 함수의 결과로서 리턴될 수 있다.

고차함수는 함수를 인자로써 전달할 수 있어야 하며, 함수의 반환 값으로 또 다른 함수를 사용할 수 있다는 특징이 있다.

**장점**<br/>
높은 수준의 추상화를 제공하고, 함수 단위의 코드 재사용이 수월하다. 불변성을 지향하기 때문에 프로그램의 동작을 예측하기 쉬워진다.

**정리!!** <br/>
기존의 명령형 프로그래밍에서 멀티쓰레드를 활용한 동시성 프로그래밍은 데드락에 빠질 위험이 존재하며, 복잡하여 고려해야 할 점들이 많다. 데드락의 발생 주 원인은 쓰레드 간에 공유되는 데이터나 상태 값이 변경 가능하기 때문이다.

하지만, 함수형 프로그래밍에서는 사용하는 모든 데이터가 변경 불가능하고 side-effect가 없다. 때문에, 여러 쓰레드가 동시에 공유 데이터에 접근하더라도 해당 데이터가 변경될 수 없기 때문에 동시성과 관련된 문제를 원천적으로 봉쇄한다.


[대강 flow 이해하기 좋음](https://tecoble.techcourse.co.kr/post/2021-09-30-java8-functional-programming/)<br/>
[여기도 자료가 좋음](https://jongminfire.dev/%ED%95%A8%EC%88%98%ED%98%95-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D%EC%9D%B4%EB%9E%80)<br/>
[함수형프로그래밍 깔끔 자료](http://ruaa.me/why-functional-matters/)