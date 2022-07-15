# What is difference between java and kotlin

> 목차
> * What is difference between java and kotlin

## 1. What is difference between java and kotlin

java는 코드에서 포착되고 선언된 확인된 예외를 지원한다. 
> 우리가 개발할 때, thorw new Exception()하는 것처럼
> 혹은 try-catch 처럼

java에는 primitive data tye이 있다. 
> int, byte, short, double, float, boolean, char, long

java는 type 지정자에 물음표를 통해 와일드 카드 유형을 사용할 수 있다.

java의 3항 연산자는 더 짧고 읽기 쉬우며 명확한 코드를 제공한다.

java는 정적 멤버를 지원한다. 

java는 암묵적으로 확장 변환을 지원한다. 
> int -> double

java는 non-private 혹은 public 필드로의 접근을 허용한다.

<hr>

kotlin은 data class를 지원한다. 컴파일러는 data class에 대한 getter/setter/constructor를 자동으로 생성한다.

kotlin은 문자열 리터럴을 지원한다. 

kotlin은 primary constructor를 지원한다. 즉, 기본 생성자 외에도 보조 생성자를 가질 수 있다.

kotlin은 싱글톤 객체의 선언을 쉽게 해준다. 

kotlin은 범위 표현 함수를 지원하며, 이는 가독성을 높여준다.

kotlin은 type에 대해 미리 정의된 연산자 집합에 대한 사용자 정의 구현을 정의함으로써 연산자 오버로딩을 지원한다.

kotlin은 상속 설계 패턴에 대한 구성을 활용하여 암무적 위임을 지원한다. 이는 여러 개의 상속을 가질 수 있는 가능성을 제공한다.

kotlin은 람다식을 사용할 때, it 이라는 변수를 제공하여 하나의 파라미터를 사용한다.

kotlin은 단일 매개변수가 있는 함수는 infix 함수로 선언할 수 있다. 

```` bash

val map1 = mapOf(Pair("key1", "value1"), Pair("key2", "value2"))
val map2 = mapOf("key1" to "value1", "key2" to "value2")

````

`to` 키워드를 통해 infix 함수를 사용할 수 있다.

kotlin은 메소드 오버로딩에 대한 대안을 제공한다.

kotlin은 lazy 초기화를 지원한다.

<hr/>

## 차이점

1. 짧은 코드

2. Null-safety

java에서는 객체에 null 값을 할당할 수 있어 NPE를 마주할 수 있다.

kotlin은 기본적으로 null을 허용하지 않는다. 따라서 안정성이 높다.

3. 확장성

java에서는 메소드를 확장하려면 클래스를 상속하고 재정의해야 한다. 

kotlin은 반드시 클래스 상속을 필요로 하지 않는다. 

4. Stric vs. static typing

java는 엄격하게 타입이 지정된 언어이다. 즉, 모든 변수는 생성될 때 타입이 필요하다.

kotlin은 대입 값의 따라 변수의 타입을 결정할 수 있다.

5. 타입 캐스팅

java는 개발자가 변수 타입을 확인하고 타입 캐스팅해야 한다.

kotlin은 컴파일러가 자동으로 관리한다.

6. 함수형 프로그래밍

java는 객체 지향 프로그래밍 언어이다.

kotlin은 객체 지향 프로그래밍과 함수형 프로그래밍 모두 지원한다.

7. 코루틴

java를 사용하면 백그라운드에서 여러 thread를 만들어 장시간 실행되는 cpu 집약적인 작업을 실행할 수 있다. 그러나 여러 개의 thread를 관리하는 것은 복잡한 작업이다

kotlin은 특정 지점에서 블록킹 thread의 실행을 중단할 수 있는 코루틴을 지원한다.

8. 컴파일 시간

java가 kotlin보다 15-20% 빠르다.

9. 플랫폼 호환성

java는 jvm용 바이트코드로 컴파일되며 , Nashorn 및 Rhino JS 엔진을 사용하여 Javascript로도 컴파일된다.

kotlin은 바이트코드와 javascript 뿐만 아니라 네이티브 코드 컴파일도 지원한다.

[영어로 된 사이트](https://www.baeldung.com/kotlin/java-vs-kotlin)