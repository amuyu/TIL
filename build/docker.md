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
ocker ps
```




# 참고
[초보를 위한 도커 안내서 - 도커란 무엇인가? ](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
[docker 설명 good!!!](http://pyrasis.com/docker.html)
[내가 docker를 시작했던 방법](http://realignist.me/code/2017/06/14/docker-my-usecase.html)


docker 깔고
docker-machine 깔고
docker-machine-parallels 깔고
docker-compose 깔고
docker-machine으로 vm 만들고
docker로 centos 실행했어요

docker hub
docket-toolbox
