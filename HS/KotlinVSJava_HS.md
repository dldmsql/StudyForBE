# Kotlin vs. Java
목차
- Kotlin
    - 컴파일 과정
    - 특성
    - 장점
- Kotlin vs. Java

## Kotlin
- 자바 플랫폼(JVM)에서 돌아가는 새로운 프로그래밍 언어, 기존 자바 라이브러리나 프레임워크와 함께 잘 작동하며 성능도 자바와 같은 수준이다.
- 안드로이드의 공식 지원 언어로 채택되었다.

### Kotlin의 컴파일 과정
- Kotlin 컴파일러가 Kotlin 코드를 컴파일해 .class 파일을 생성하고 이 과정에서 Kotlin 코드가 참조하는 Java 코드가 함께 로딩되어 사용된다. 
- Java 컴파일러가 Java 코드를 컴파일해 .class 파일을 생성하고 이때 이미 Kotlin이 컴파일한 .class 파일의 경로를 클래스 패스에 추가해 컴파일한다.

### Kotlin의 특성
1. 대상 플랫폼: 서버, 안드로이드 등 자바가 실행되는 모든 곳
2. 정적 타입 지정 언어
> 정적 타입 지정: 모든 프로그램 구성 요소의 타입을 컴파일 시점에 알 수 있고 프로그램 안에서 객체의 필드나 메소드를 사요할 때마다 컴파일러가 타입을 검증해준다.
3. 타입 추론: 코틀린의 컴파일러가 문맥으로부터 변수 타입을 자동으로 유추할 수 있어서 모든 변수의 타입을 프로그래머가 직접 명시할 필요가 없다.
4. 널이 될 수 있는 타입: null이 될 수 있는 타입을 지원해서 컴파일 시점에 null point exception이 발생할 수 있는지 여부를 검사할 수 있다.
5. 함수형 프로그래밍과 객체지향 프로그래밍

### Kotlin 장점
1. 자바와 호환성: 자바에서 적용하던걸 모두 코틀린에서 할 수 있다. 
2. 실용적: 다른 프로그래밍 언어가 채택해서 이미 성공적으로 검증된 해법과 기능만 담았다.
3. 간결성: getter, setter, 생성자 파라미터를 대입 로직 등 자바에서 번거로웠던 코드를 코틀린에서 묵시적으로 제공한다. 
4. 안전성: JVM에서 실행하기 때문에 상당한 안전성을 보장한다. 
> JVM 사용: 메모리 안전성 보장, 버퍼 오버플로우 방지, 동적 메모리 할당으로 인한 문제 예방

## Kotlin vs. Java
1. 변수/상수   
- Java: 변수-final 사용 x, 상수: final 사용
- Kotlin: 변수-var 사용, 상수: val 사용

2. view 사용
- Java: findViewById() 함수로 Button 객체에 할당
- Kotlin: xml에서 정의한 id 값을 그대로 사용

3. Null 안전성
- Java: @(Annotation)을 사용해서 Nullable(null일 들어올 수 있는 변수, default)과 NonNull(null이 될 수 없는 변수)을 구분할 수 있다.
- Kotiln: ?(Optional)을 사용해서 Nullable과 NonNull(Default)을 구분할 수 있다. 변수 뒤에 자료형을 꼭 붙여야 하고 ?을 붙이면 null이 들어올 수 있고, 붙이지 않으면 null이 될 수 없다. 

4. 객체 초기화
- Java: 객체를 new로 초기화하고 초기화한 객체를 이용해 초기 작업 수행
- Kotlin: apply block을 이용하여 초기화 작업 수행, 이 안에서는 초기화된 객체 자신을 this로 사용하기 때문에 객체를 따로 명시해주지 않아도 바로 객체의 함수를 사용할 수 있다.

5. Data class
- Java: DTO를 만들기 위해선 변수에 대한 get/set 함수를 만들어주고 생성자를 만들어 변수를 초기화하는 작업을 수행할 수 있다.
- Kotlin: class를 data class로 만들어 생성자에 인자를 var로 정의해주면 DTO로 사용할 수 있다. data class의 생성자에 초기값을 미리 주면 비어있는 객체도 만들 수 있다. 비어있는 객체는 apply block을 이용하여 초기화시킬 수 있다. 

## 참고
(https://pearlluck.tistory.com/701)
(https://diqmwl-programming.tistory.com/115)
