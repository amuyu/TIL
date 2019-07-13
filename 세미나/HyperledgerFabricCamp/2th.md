# 실습
linux, node, go, docker

# fabric 로드맵

# 실습환경
- golang
- docker/docker-compose
- ssh 접속 프로그램

hfc 개발을 위해서는 node 나 java 도 설치해야...
이번 강의에서는 나중에 node 진행..

## 실습환경 구성
sudo apt update
sudo apt upgrade
sudo apt install golang-go
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
ps -ef | grep docker
sudo usermod -aG docker $USER   // docker group 에 내 계정 추가
sudo apt install docker-compose
docker-compose

## Fabric 인프라 이론과 실습
네트워크 토폴로지 설정과 키 발급, 인증서 생성
Genesis 블록을 위한 설정 및 생성
채널 아티팩트 생성
채널에 속한 Org별 Anchor 정보 업데이트
도커 컨테이너 설정 및 컨테이너 기동
채널 아티팩트를 이용한 채널 구성 트랜잭션 (Orderer)
채널 구성을 위한 멤버 peer join
채널 구성원간 통신을 위한 Anchor 정보 update


[sample](https://github.com/yhkim4fc/fabric-samples)