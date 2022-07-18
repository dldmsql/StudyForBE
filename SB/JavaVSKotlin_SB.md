# Java vs Kotlin

<aside>
🤔 안드로이드 개발에 있어서 Java 대신 Kotlin을 사용하는 이유는?

</aside>

## Java

- 객체 지향 프로그래밍 언어
- 여러 플랫폼 및 시스템 / 장치에서 동일한 프로그램을 실행할 수 있는 JVM(Java Virtual Machine)에서 실행됨
- 포인터 개념 X, 자동으로 메모리 관리
- 멀티 스레드 구현 가능
- 필요한 시점에 구현한 클래스를 동적으로 로딩 가능

## Kotlin

- 최신 기능을 처리하는 새로운 프로그래밍 언어
- JVM의 바이트 코드로 실행되며 자바와 100% 호환됨
- 객체지향 프로그래밍 + 함수형 프로그래밍 언어
- 서버 측 응용 프로그램 개발에 적합
- 간결하고 표현력 있는 코드 작성 가능
- 모든 타입이 클래스 타입

## 변수 / 상수

### Java

```java
String strVar = ""; //변수
final String strVal = ""; //상수
```

- 변수 : final 사용 X
- 상수 : final 사용

### Kotlin

```kotlin
var strVar = "" //변수
val strVal = "" //상수
```

- 변수 : var 사용
- 상수 : val 사용
- IOS 개발에서 사용하는 Swift와 비슷

## View 사용

- activity_main.xml의 btn_tes라는 id를 가진 button 사용 시

### Java

```java
btnTest = findViewById(R.id.btn_test); 
// 객체를 만들어 findViewById() 함수로 객체에 할당
btnTest.setText("Test");
```

- findViewById() 함수로 Button 객체에 할당

### Kotlin

```kotlin
btn_test.text = "Test" // xml에서 정의한 id 값으로 view 사용
```

- xml에서 정의한 id 값 그대로 사용

- Java에서는 view의 id를 객체로 할당하는 과정 필요, Kotlin에서는 view의 id 그대로 사용 가능

## NULL 안정성

### Java

```java
@Nullable String strNullable = null;
@NonNull String strNonNull = "";
```

- @(Annotation)을 사용하여 Nullable과 NonNull 구분 가능

### Kotlin

```kotlin
var strNullable: String? = null
var strNonNull: String = ""
```

- ?(Optional)을 사용하여 Nullable과 NonNull 구분 가능
- ?을 사용하기 위해서 변수 뒤에 자료형 필요

- Java에서는 Annotation을, Kotlin에서는 Optional 사용

### Null Control

### Java

```java
strNullable.split("/"); // NPE 발생 가능

if (strNullable != null) {
    strNullable.split("/");
}
```

- null 체크를 위하여 if 문 사용

### Kotlin

```kotlin
strNullable.split("/"); // NPE 발생 가능

strNullable?.split("/")
```

- ?(Optional)을 사용하여 strNullable 변수가 null일 경우 함수 실행 X → null을 안전하게 사용 가੯‧̀͡u\

## 객체 초기화

### Java

```java
Intent testIntent = new Intent(this, SecondActivity.class); 
// 일반적인 객체 초기화 및 초기 작업
testIntent.putExtra("ext1", 1);
testIntent.putExtra("ext2", 2);
testIntent.putExtra("ext3", "3");
testIntent.putExtra("ext4", "4");
testIntent.putExtra("ext5", false);
```

- 객체를 new로 초기화하고 초기화된 객체를 이용하여 초기 작업 수행

### Kotlin

```kotlin
val testIntent = Intent(this, SecondActivity::class.java).apply { 
// 객체 초기화 시 초기 작업 수행

    putExtra("ext1", 1)
    putExtra("ext2", 2)
    putExtra("ext3", "3")
    putExtra("ext4", "4")
    putExtra("ext5", false)
}
```

- apply block을 이용하여 초기화 작업 수행, 블록 안에서는 초기화된 객체 자신을 this로 사용하여 객체를 따로 명시해주지 않아도 바로 객체 함수 사용 가능 → 코드 관리 수월함

## Data Class

### Java

```java
public class JavaData {

    JavaData(String s, int i, boolean b) {
        this.s = s;
        this.i = i;
        this.b = b;
    }

    private String s;
    private int i;
    private boolean b;

    public String getS() {
        return s;
    }

    public void setS(String s) {
        this.s = s;
    }

    public int getI() {
        return i;
    }

    public void setI(int i) {
        this.i = i;
    }

    public boolean isB() {
        return b;
    }

    public void setB(boolean b) {
        this.b = b;
    }
}
```

```java
JavaData data1 = new JavaData("", 0, true); // 생성자로 초기화

JavaData data2 = new JavaData(); 
// 빈 생성자로 생성 후 set 함수로 초기화
data2.setS("");
data2.setI(0);
data2.setB(true);
```

- DTO를 만들기 위해 변수에 대한 get/set 함수 정의, 생성자를 만들어 변수 초기화

### Kotlin

```kotlin
data class KotlinData(var s: String?,
                      var i: Int,
                      var b: Boolean)
```

```kotlin
val data1 = KotlinData("hi", 1, false) // 생성자로 초기화
```

- class를 data class로 만들어 생성자에 인자를 var로 정의해주면 DTO로 사용 가능, data class의 생성자에 초기값을 미리 주면 비어있는 객체도 만들 수 있음
- 비어있는 객체는 위의 intent 초기화와 같이 apply block을 이용하여 초기화 시킬 수도 있음

[https://dev-imaec.tistory.com/m/36](https://dev-imaec.tistory.com/m/36)

[https://little-habit.tistory.com/2](https://little-habit.tistory.com/2)

[https://bbaktaeho-95.tistory.com/50](https://bbaktaeho-95.tistory.com/50)

[https://mondayless.tistory.com/25](https://mondayless.tistory.com/25)