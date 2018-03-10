# docker 란?
컨테이너 기반의 오픈소스 가상화 플랫폼
다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램 배포 및 관리를 단순하게 해준다.

컨테이너는 격리된 공간에서 프로세스가 동작하는 기술

하나의 서버에 여러개의 컨테이너를 실행하면 서로 영향을 미치지 않고 독립적을 실행되어 마치 가벼운 VM을 사용하는 느낌을 준다.

## 이미지
도커에서 가장 중요한 개념은 컨테이너와 함께 이미지라는 개념이다.
컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있다.
컨테이너는 이미지를 실행한 상태라고 볼 수 있다.
같은 이미지에서 여러개의 컨테이너를 생성할 수 있다.

도커는 이미지를 만들기 위해 `Dockerfile` 이라는 파일에 자체 DSL 언어를 이용하여
이미지 생성 과정을 적는다.



# 설치
mac 은
[docker store](https://store.docker.com/editions/community/docker-ce-desktop-mac) 에서
다운로드 받는다.

## docker-machine-parallels
parallesls 에서 사용 하려면 (driver 역할)
```sh
brew install docker-machine-parallels
```
을 설치한다.

# 실행

```sh
docker run ubuntu:16.04
```

# docker-machine
docker-machine create
docker-machine start default  
docker-machine env prl-dev
eval $(docker-machine env prl-dev)


# 흐름
## docker 설치
docker toolbox
## docker 이미지 찾기
docker search {name}
docker pull {이미지이름}:{태그}
## 이미지 확인
docker images
## 컨테이너 생성
docker run -i -t --name helloCentos centos /bin/bash
-i (interactive), -t (tty)
실행 된 bash 셸에 입력 및 출력을 할 수 있음
이미지에서 쉘을 실행하는 명령
## 컨테이너 목록 확인
docker ps -a
## 컨테이너 재시작 하기
docker restart helloCentos
docker start <container>
## 컨테이너 접속하기
docker attach helloCentos
컨테이너 접속 상태에서  Ctrl+P, Ctrl+Q 를 입력하고 빠져나오면 컨테이너가 종료되지 않습니다.
## 외부에서 컨테이너 명령 실행하기
docker exec hello echo "Hello World"
## 컨테이너 정지하기
docker stop helloCentos
## 컨테이너 삭제하기
docker rm helloCentos
## 이미지 삭제하기
docker rmi <이미지 이름>:<태그>


## port 설정
docker add-host

# docker-machine
```sh
docker-machine create --driver=parallels prl-dev
docker-machine env prl-dev
eval $(docker-machine env prl-dev)
docker ps
```

# docker-machine 환경 변수 제거
eval "$(docker-machine env -u)"

# docker container stop, rm
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

# docker 빠져나오기
ctrl + p + q


# 참고
[초보를 위한 도커 안내서 - 도커란 무엇인가? ](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
  - 단계별로 설명이 잘 되어 있어요, 개념 설명에서부터 실전까지
[docker 설명 good!!!](http://pyrasis.com/docker.html)
[docker hub](https://hub.docker.com)
[Docker Machine을 이용하여 Mac에서 docker 운영하기](http://blog.saltfactory.net/running-docker-on-mac-using-with-docker-machine/)
[Docket 활용법 - 개발환경 구성하기](http://raccoonyy.github.io/docker-usages-for-dev-environment-setup/)
[Django 샘플 프로젝트](https://github.com/raccoonyy/django-sample-for-docker-compose)


docker 깔고
docker-machine 깔고
docker-machine-parallels 깔고
docker-compose 깔고
docker-machine으로 vm 만들고
docker로 centos 실행했어요

docker hub
docket-toolbox
