# What is Classloader

## ClassLoader

Java Complier를 통해서 .class 확장자를 가진 클래스 파일은 하나의 디렉토리에 있는 것이 아니다. 그리고 기본적인 라이브러리의 클래스 파일들은 $JAVAHOME_ 내부 경로에 존재한다. 각각의 클래스 파일들을 찾아서 JVM의 메모리에 탑재해주는 역할을 하는 것이 classloader이다.

classloader는 이외에도 Loading, Linking, Initialization 3가지 역할을 한다. 

* Loading

클래스 파일을 탑재하는 과정

* Linking

클래스 파일을 사용하기 위해 검증하고 기본 값으로 초기화하는 과정

* Initialization

static field의 값들을 정의한 값으로 초기화하는 과정

## 1. Loading

classLoader가 필요한 클래스 파일들을 찾아 탑재하게 된다. 각각의 클래스 파일들이 기본으로 제공받는 클래스 파일인지 혹은 개발자가 정의한 클래스 파일인지와 같은 기준에 의해 classloader를 계층형으로 구분한다.

1. Bootstrap class loader

`rt.jar` 파일을 찾는다.

다른 모든 classloader의 부모가 되는 classloader이다. `rt.jar`를 포함하여 JVM을 구동시키기 위한 가장 필수적인 라이브러리의 클래스들을 JVM에 탑재한다. 가장 상위의 classloader이므로 탑재되는 운영체제에 맞게 네이티브 코드로 쓰여져 있다.

2. Extension class loader

`lib/ext` 파일을 찾는다.

localedata, zipfs 등 다른 표준 핵심 java class의 라이브러리들을 JVM에 탑재하는 역할을 한다. 이 파일들은 $JAVAHOME/jre/lib/ext_에 있다.

3. Application class loader 

`class path` 파일을 찾는다.

System Class loader라고도 한다. classpath에 있는 클래스들을 탑재한다. 개발자가 작성한 코드로 된 클래스 파일들을 JVM에 탑재한다. 만약 개발자가 class loader를 구현하여 사용하게 되면, 기본적으로 application class loader의 자식 형태로 사용자 정의된 (user defined) class loader를 구성하게 된다. 

여기까지 거쳐왔음에도 클래스 파일을 찾지 못할 경우, `ClassNotFoundException`이라는 예외를 던진다. 

Loding 과정에서는 하위 Class loader가 로딩한 클래스 파일은 상위 Class loader가 로딩한 클래스 파일을 볼 수 있다. 즉, 하위에서는 상위 클래스 파일을 볼 수 있지만, 그 반대는 불가능하다.

또한, 한 번 JVM에 탑재된 클래스 파일은 종료될 때까지 JVM에서 제거되지 않는다.

## 2. Linking

로드된 클래스 파일들을 검증하고 사용할 수 있게 준비하는 과정을 의미한다. 

1. Verfication

클래스 파일이 유요한 지 확인하는 과정이다. 클래스 파일이 JVM의 구동 조건대로 구현되지 않았을 경우에는 VerifyError를 던지게 된다.

2. Preparation

클래스 및 인터페이스에 필요한 static field 메모리를 할당하고, 이를 기본값으로 초기화한다. 기본값으로 초기화된 static field 값들은 뒤의 Initialization 과정에서 코드에 작성한 초기값으로 변경된다. 

3. Resolution

symbolic reference 값( 명확히 정의되지 않은 값 )을 JVM의 메모리 구성 요소인 method area의 런타임 환경 풀을 통해 direct reference라는 메모리 주소 값으로 바꾸어 준다. 해당 단계의 영향을 받는 JVM instruction 요소는 `new`와 `instanceof`가 있다.

## 3. Initialization

클래스 파일의 코드를 읽는 과정이다. java 코드에서의 class와 interface의 값들을 지정한 값들로 초기화 및 초기화 메소드를 실행시켜준다. 이때, JVM은 멀티 스레딩으로 작동하며, 같은 시간에 한 번에 초기화를 하는 경우가 있기 때문에 초기화 단계에서도 동시성을 고려해주어야 한다. 

## JAVA 11

java 8까지는 `rt.jar`의 위치를 확인할 수 있었으나 9버전 부터 `rt.jar`과 `ext` 경로가 삭제되고 효율성을 위해서 클래스들은 다른 경로로 이동되었다.

https://tecoble.techcourse.co.kr/post/2021-07-15-jvm-classloader/

https://sas-study.tistory.com/262 -> 이미지 가져올 블로그

https://www.tutorialspoint.com/what-are-the-changes-of-class-loaders-in-java-9 Java9 버전 이후의 변경사항