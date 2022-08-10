# Build

- 소스 코드 파일을 컴퓨터에서 실행할 수 있는 독립적인 형태로 변환하는 과정과 그 결과
- 컴파일 된 코드를 실제 실행할 수 있는 상태로 만드는 일
- 컴파일을 포함해 war, jar 등 실행 가능한 파일을 뽑아내기까지의 과정

<aside>
💡 개발자가 작성한 java 파일과 여러 가지 resource에 대하여
소스코드를 컴파일해서 .class로 변환하고, resource를 .class에서
참조할 수 있는 적절한 위치로 옮기고 META-INF와 MANIFEST.MF들을
하나로 압축하는 과정임

META-INF/MANIFEST.MF
- 패키징된 jar 파일의 사용매뉴얼이나 스펙을 가지고 있는 사용 설명서인 manifest 파일을 담는 폴더로 활용됨 - 한마디로 메타데이터

</aside>

## Build Tool(빌드 툴)

- 빌드 과정을 도와주는 도구
- 전처리, 컴파일, 패키징, 테스팅, 배포 등의 기능 포함
- ex. Ant, Maven, Gradle 등

### Compile

- 사용자가 작성한 코드를 컴퓨터가 이해할 수 있는 언어로 번역하는 일

[https://choseongho93.tistory.com/296](https://choseongho93.tistory.com/296)

# Deploy

- 빌드가 완성된 실행 가능한 파일을 사용자가 접근할 수 있는 환경에 배치시키는 일

<aside>
💡 `ex.`

- 코드를 짜고 run 버튼을 눌러 코드 실행 (컴파일 + 실행)
- 정상적으로 실행되면 이것을 war 파일로 뽑아서 (빌드) 웹 서버에 올리거나 (배포)
- JSmooth 등을 사용해 exe, jar 파일로 뽑아(빌드) 사용자에게 줌(배포)
</aside>

[https://itholic.github.io/qa-compile-build-deploy/](https://itholic.github.io/qa-compile-build-deploy/)