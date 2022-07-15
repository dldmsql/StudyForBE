# What is Kotlin

> 목차
> * What is Kotlin

## 1. What is Kotlin

코틀린은 JetBrains에 의해 탄생된 언어이다.

**자바와 호환되는 이유**

자바는 자바 컴파일러를 통해 .java를 컴파일하게 된다. 그리고 바이트 코드 .class를 생성한다. 운영체제 마다 각자의 컴파일 머신이 다르기 때문에 java는 jvm을 통해 각 운영체제에 맞는 기계어로 컴파일하게 된다. 

코틀린으로 만들어진 코드는 java 컴파일러를 통해서 컴파일되기 때문에 jvm이 각 운영체제 맞는 기계어로 변환해서 프로그램이 실행되는 것이다.

**코틀린 장점**

1. 코루틴

메인 루틴과 서브 루틴이 서로를 호출하는 형태로 한 개의 코드로 여러 개의 작업이 가능하다.

```` bash
print("Start Main Thread")
GlobalScope.launch {
    delay(3000)
    print("in Coroutine ...")
}
print("End Main Thread")

// output
// Start main Thread
// End Main Thread
// in Coroutine ...
````
코루틴은 thread와 기능적으로 같지만, thread에 비교하면 좀 더 가볍고 유연하며 한 단계 더 진화된 병렬 프로그래밍을 위한 기술이다.

[코루틴 예제](http://www.gisdeveloper.co.kr/?p=10279)

2. null safety

런타임 도중 null 참조 에러가 발생하는 버그를 잡아주는 기능이다. 컴파일은 정상적으로 되지만 프로그램 실행하는 과정에서 발생하는 럼타임 에러를 잡아준다.

3. Js 호환

js로토 컴파일이 가능하다. 코틀린 코드를 js로 컴파일해서 리액트 js 프로젝트에도 사용 가능하다.

4. data science 활용

data science 시각화 라이브러리들이 조금씩 등장하면서 파이썬과 같이 작용할 것으로 기대한다.

[코틀린 정리](https://incomeplus.tistory.com/343)