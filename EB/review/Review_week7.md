# Review of Studying week 7

## Docker with spring application architecture

![image](https://user-images.githubusercontent.com/61505572/185344878-2ca44f53-0410-4a4c-a1dd-157b9aa6337d.png)

위의 아키텍처는 아래와 같은 상황을 전제로 한다.

- client-server 구조
- spring framework 기반
- spring security framework 기반
- docker-compose를 이용한 RDB, Redis 사용
- REST API 형식

### Flow

1. client가 GET/user/1 요청을 한다.
2. server의 filter가 url 요청을 먼저 캐치한다.
3. filter는 chiaining 방식으로 올바른 사용자인지 확인한다. ( 인증, 권한 확인 )
> 단, 아키텍처에서도 알 수 있듯이 filter는 spring MVC와는 구분된 영역이다.
4. filter에서 통과한 요청을 dispacther servlet이 받는다.
5. dispachter servlet은 handler mapping에게 해당 요청을 담당하는 controller를 찾을 것을 요청한다.
6. handler mapping은 controller를 찾아서 dispachter servlet에게 알려준다.
7. dispachter serlvet은 contoller에게 url 요청을 위임한다.
8. controller는 해당 요청을 처리하기 위해 관련된 service, repository를 찾는다.
9. DB에 접근해야 할 경우, spring은 docker 위에 동작하는 컨테이너의 ip 주소와 포트 번호를 이용해 도커 엔진 내부 server에게 request한다. 
10. 도커 엔진 내부 server는 해당 컨테이너에 rest api로 데이터를 찾아 spring에 response한다. 

## SSL Flow

![image](https://user-images.githubusercontent.com/61505572/186295428-057b373e-2090-44fe-9c73-bbdeeab8b47a.png)

1. client hello

client가 아래의 정보를 server에게 보낸다.

* client가 생성한 랜덤 데이터
* client가 사용할 수 있는 암호화 방식들

2. server hello

server가 아래의 정보를 client에게 보낸다.

* server 인증서 ( 공개키 포함 )
* server가 생성한 랜덤 데이터
* server가 사용할 암호화 방식

3. client의 server 확인

client는 브라우저에 내장된 CA 인증서 리스트를 통해 CA에 의해 발급된 것인지 확인한다.

client는 client 랜덤 데이터와 server 랜덤 데이터를 조합하여 `pre master secret`을 생성한다. ( 대칭키로 사용 예정 )

4. client의 pre master secret 전송

client는 인증서 내부에 있던 공개키를 이용해 `pre master secret` 값을 암호화하여 서버로 전송한다.

5. server의 pre master secret 확인

server는 자신의 개인키로 `pre master secret` 값을 복호화 한다.

6. 각자 master secret 생성 및 session key 생성

- 데이터 + pre master secret + client random data + server random data를 특정 암호화 알고리즘을 이용해 hash 값으로 만든다.
- pre master secret 값과 hash 값을 특정 암호화 알고리즘을 이용해 hash 값으로 만든다.
- 이렇게 만들어진 hash 값을 조합해 master secret 값을 만든다.
- 데이터 + master secret + client random data + server random data를 특정 암호화 알고리즘을 이용해 hash 값으로 만든다.
- pre master secret 값과 hash 값을 특정 암호화 알고리즘을 이용해 hash 값으로 만든다.
- 이렇게 만들어진 hash 값을 조합해 key material 값을 만든다.
- key material 값은 4가지 종류가 있으며 이들은 session key로 사용된다.

7. session key를 이용해 client 와 server의 암호화 통신

session key는 대칭키로 양쪽 모두가 알고 있는 값이다.

8. 연결 종료

데이터 전송이 끝나면 SSL 통신이 끝났음을 서로에게 알리고 session key를 폐기한다.

## Test code

spring에서 test 자동화 문서 도구로 swagger와 Rest Docs를 많이 사용한다.

각각의 특징은 아래와 같다.

1. Swagger

* 의존성 설정이 쉽다.
* 테스트 코드 작성이 불필요하다.
* 비즈니스 로직에 관련 코드를 작성해야 한다.
* UI를 통해 테스트할 수 있다.

2. Rest Docs

* 의존성 설정이 많다.
* 테스트 코드 작성이 필수적이다.
* 비즈니스 로직에 관련 코드를 작성할 필요가 없다.
* 가독성 높은 문서를 제작할 수 있다. 

두 개 중 하나만 사용할 수도 있고, 둘 다 사용할 수도 있다.

## Collection vs. Collections in java

### Colletion interface

Collection 인터페이스는 하나의 자료를 모아서 관리하는 데 필요한 기능을 제공한다.

* Set

순서가 없고, 중복을 허용하지 않는 자료구조이다.

* List

순서가 있고, 중복을 허용하는 자료구조이다.

* Queue

FIFO의 개념을 가진 선형 자료구조이다.

### Collections class

Collection을 가지고 놀기 위한 클래스이다. 다시 말해 Collection 인터페이스를 리턴하거나 Collection 인터페이스에서 동작하는 메소드를 모아놓은 클래스이다.

## JVM

자바 코드를 실행하면 자바 컴파일러가 자바 바이트 코드로 컴파일한다. 이를 클래스 로더에게 전달한다. 클래스 로더는 동적로딩을 통해 필요한 클래스를 로딩하고 링크하여 런타임 데이터 영역에 올린다. 실행엔진은 런타임 데이터 영역에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다.

### 클래스 로더

- 부트스트랩 클래스 로더

최상위 클래스 로더이다.

JVM이 실행될 때 같이 메모리에 올라간다.

JAVA API들을 로드한다.
> $JAVA_HOME/jre/lib 에 있는 라이브러리를 로드한다.

- 익스텐션 클래스 로더

기본 JAVA API를 제외한 확장 클래스를 로드한다.
> $JAVA_HOME/lib/ext 에 있는 라이브러리를 로드한다.

- 시스템 클래스 로더

어플리케이션의 클래스 자체를 로드한다.

- 사용자 정의 클래스 로더

어플리케이션 사용자가 직접 코드 상에서 생성하여 사용하는 클래스 로더

> 예를 들면, 브라우저는 사용자 정의 클래스 로더를 사용해 웹 사이트로부터 실행 가능한 콘텐츠를 가져와 로딩한다. 

[java-classloader](https://www.baeldung.com/java-classloaders)

클래스 로더는 위임 모델 형식이다.

처음 바이트 코드를 넘겨 받은 클래스 로더가 필요한 클래스를 로드할 때 혹은 실행 엔진에서 명령어 단위로 바이트 코드를 실행하다가 처음으로 참ㅈ하는 클래스에 대해 클래스 로더에게 로드를 요청할 때 로드를 요청받은 클래스 로더는 다음 순서대로 요청 받은 클래스가 있는 지 학인한다.

1. 클래스 로더 캐시
2. 상위 클래스 로더
3. 자기 자신

이전에 로드된 클래스인지 클래스 로더 캐시를 확인한다. 없다면 상위 클래스 로더를 거슬러 올라가는데, 도중에 찾았더라도 부트스트랩 클래스 로더까지 확인을 하고 거기에 있다면 그 클래스를 로드한다. 만약 없다면, 파일 시스템에서 해당 클래스를 찾는다.

클래스 로더는 위임 모델 특징과는 반대로 하위 클래스 로더에 있는 클래스는 확인 불가능한 가시성 제한 특징을 지녔다. 또한, 클래스를 로드하는 것은 가능하지만 언로드 하는 것은 불가능하다.

클래스 로더는 name space라는 공간을 갖는데, 여기에 로드된 클래스를 보관한다. 

### Run time Area 의 힙

가비지 컬렉션의 대상이다. 왜? 사용하지 않는 메모리를 그대로 두면 메모리 부족으로 서버가 요청을 처리하지 못하는 상태가 된다. 그래서 인스턴스 변수를 저장하는 힙 영역에서 사용하지 않는 오브젝트를 정리하여 메모리를 확보한다.
