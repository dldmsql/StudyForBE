# Java vs Kotlin

### Java
- 객체 지향 프로그래밍 언어
- 이식성이 높음
- 메모리 자동 관리
- 멀티 스레드 구현 쉬움
- 필요한 시점에 클래스를 동적으로 로딩 가능
- 분산환경 지원
- 오픈소스 라이브러리 풍부

### Kotlin
- 자바 플랫폼에서 돌아가는 새로운 프로그래밍 언어
- 객체지향 프로그래밍 + 함수형 프로그래밍 언어

    - 자바 코드 - 객체지향 프로그래밍 : 클래스 내부에 있는 함수에서만 로직을 작성
    ```
    class Hello {
        public static void main(String args[]) {
       System.out.print("Hello World");
        }
    }
    ```
    - 코틀린 코드 - 함수형 프로그래밍 : 제한 없이 어디에서나 작성 -> 간결한 코드, 강력한 추상화, 코드 중복
    ```
    System.out.print("Hello World");
    ```
- 자바와 상호운용
- 간결하고 실용적인 코드 작성 가능
- 안정성(null 체크, 타입 검사와 캐스트)
- 정적 타입 지정 언어 + 타입 추론

### 코틀린 컴파일 과정
> Java 코드와 Kotlin 코드가 함께 있는 프로젝트


- Kotlin 컴파일러가 Kotlin 코드를 컴파일해 .class 파일을 생성
- 이 과정에서 Kotlin 코드가 참조하는 Java 코드가 함께 로딩되어 사용
- Java 컴파일러가 Java 코드를 컴파일해 .class 파일을 생성
- 이때 이미 Kotlin이 컴파일한 .class 파일의 경로를 클래스 패스에 추가해 컴파일

### Java와 Kotlin 차이
1. 변수 선언 </br>
코틀린은 자바와 다르게 변수 타입 생략 가능 (컴파일러가 타입을 정해주는 타입추론 기능)
2. 변수 null 여부</br>
자바는 객체에 null 값을 할당 가능  </br>
코틀린은 기본적으로 null을 허용x
3. 접근 제한자
- 자바
    - public    : 모든 접근을 허용
    - protected : 같은 패키지에 있는 객체와 상속관계의 객체들만 허용
    - default   : 같은 패키지(폴더)에 있는 객체들만 허용
    - private   : 현재 객체 내에서만 허용
- 코틀린 
    - public		: JAVA와 동일
    - private		: JAVA와 동일
    - protected	: JAVA와 동일
    - internal	: 같은 모듈 내에서 어디서든 접근 가능
4. 함수 선언
- 자바 </br>
```
public int sum(int a, int b) {
	return a + b
}
```
- 코틀린 </br>
fun으로 선언. 함수명(파라미터): 리턴타입
사용되는 파라미터 (a, b)는 상수인것이 특징
```
fun sum(a: Int, b: Int): Int {
    return a + b
}
```
5. 클래스 선언
- 자바
```
public class Test {
    private String name;

    public Test(String name) {
        this.name = name;
    }
    
    public String getName() {
    	return name;
    }
}
```
- 코틀린
```
public class Test {
    private String name;

    public Test(String name) {
        this.name = name;
    }
    
    public String getName() {
    	return name;
    }
}
```

## 참고자료
https://helloworld-88.tistory.com/3</br>
https://www.hanbit.co.kr/channel/category/category_view.html?cms_code=CMS7811735294</br>
https://ktko.tistory.com/entry/%EC%BD%94%ED%8B%80%EB%A6%B0%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%9C%EA%B0%80 </br>
https://diqmwl-programming.tistory.com/115
 