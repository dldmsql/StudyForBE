# What is Docker

> * 목차
> * What is Docker
> * Why to use docker
> * Adventage of use docker

## 1. What is Docker

도커는 2013년도에 등장한 컨테이너 기반 가상화 도구로 DevOps 및 개발자들에게 도움이 되도록 설계된 개방형 어플리케이션 개발 프레임워크이다.

다양한 OS의 응용 프로그램들을 컨테이너에 담아 어디서든 쉽게 실행할 수 있도록 설계되어 있으며, Go언어로 개발되었다.

## 2. Why to use docker

하나의 업무를 하는데 서버를 여러 대 운영하고 있다고 가정하자. 각 서버를 운영하기 위해 운영체제로부터 컴파일러, 설치된 패키지까지 같은 환경으로 구축하고자 한다면 모두 완벽하게 동일할까? 

또한, 윈도우 로컬에서 개발을 완료하고 서버에 올렸을 때, 무조건적으로 코드가 정상적으로 작동할까?

이러한 문제들을 Environment disparity라고 한다. 즉, 개발 환경이 맞지 않아 생기는 문제이다.

## 3. Adventage of use docker

* 구성 단순화

도커는 모든 플랫폼에서 실행할 수 있다. Dockerfile에 전체 운영 환경 변수를 담아 전달이 가능하다. 

* 빠른 배포

컨테이너는 별도의 OS를 부팅하지 않고 어플리케이션을 실행하므로 컨테이너를 빠르게 만들 수 있다.

* 코드 관리

도커는 환경 자체를 배포하기 때문에 개발 및 코딩을 편하게 만들어준다.

* 개발 생산성 향상

CentOs 환경의 도커 컨테이너를 실행하는 경우에도 shared Volume 기능을 통해 윈도우 로컬 환경에서 에디터를 통해 소스코드 수정이 가능하다.

* 어플리케이션 격리

웹 서버와 연결된 API 서버들을 격리해야 하는 경우들이 있다. 이 경우, 다른 컨테이너를 통해 API 서버를 실행시킬 수 있다. 

[도커란 무엇인가?](https://artistdata.tistory.com/3)