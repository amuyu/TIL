# first-network 구동
fabric-samples.git 의 first-network 구동
## 서버 구성
서버구성은 다음을 참고, `fabric-samples/first-network/docker-compose-cli.yaml`
- peer0.org1.example.com
- peer1.org1.example.com
- peer0.org2.example.com
- peer1.org2.example.com
- orderer.example.com
- cli

## 서버 실행
### binary & image 파일 다운로드
hyperledger/fabric에서 bootstrap.sh을 실행
### 네트워크 실행 및 테스튼
./byfn.sh -m up 

# first-network 설정 (Api 서버 적용)
## crypto-config 복사
`./byfn.sh -m up` 실행 후, 생성된 `channel-artifacts` 와 `crypto-config` 파일 복사
api server 프로젝트의 `common/connection-profile` 에 `crypt-config` 를 복사

## network-config 설정
`network-config-{network_name}.yaml` 과 `org-{network.name}.yaml` 을 설정한다.
`configtx` 나 `docker-compose` 파일을 참고한다.
network 이름은 `.env` 에서 설정한다.

## config.js 설정
config.js 에 org를 두개 설정하도록 수정한다.
if (env=='first_network') {
    hfc.~ org2개 생성
else
    hfc.~ org1개 생성 
}

--------------------------------------------------------------------------------
# first-network raft 알고리즘 적용 방법 
raft 를 지원하는 최신 버전을 받는다. (1.4.2. 최신 버전)
cd fabric-samples/first-network 에 byfn.sh 밑에 보면
/etcd에 대한 부분을 확인하여 kafka인지 확인할수 있다. 
```sh
// etcdraft 권한으로 변경해서 실행
./byfn.sh -m up -o etcdraft 
```
raft 부분의 채널이 2개 어플리케이션, 시스템 채널을 구동함 . 
mychannel / byfn-sys-channel
(peer-base.yaml 파일에 FAEROC_LOOGING_SPEC=DEBUG  mode on) 
logs를 보면 leader값이나 현재 채널의 상태 및 현상을 볼수 있다.

네트워크 재 시작 필요 
 (시스템들 간의 채널을 통한 통신이 필요하기 때문)
* 시스템 버전 업데이트 하고 네트워크 적용 후 구동을 하여야 한다. 
[code 단에서 fabric/orderer/conesnsus/etcdraft/consernter.go 부분 확인 하여 raft 알고리즘이 연동되는 부분 자세히 설명]

-------------------------------------------------------------------------------

1. 인증서를 생성하고 docker-compose 에 적용하는것 인가.
2. api 서버에서도 바뀐 인증서를 적용시키기 

[프로비저닝을 하기 위한 네트워크 구성하기]
git pull 
mkdir fastnet
crypto-config.yaml 파일로 인증서를 생성하고, configtx.yaml파일로 제네시스 블록를 생성한다.
vi crypto-config.yaml 파일 
orderer 중의 peer 갯수 수정 (org count : peer 개수)
crypto-config 적용시키는 sh이
vi cryptogen.sh
 cryptogen generate --config=./crypto-config.yaml 
이때 crypto-config.yaml 파일을 수정 후에 cryptogen.sh을 실행시키면  cryptogen-config 파일이 생성됨

인증서 폴더 확인
org2.example.com에 대한 인증서가 생성된다. 
인증서 추가 후 peer 노드를 하나 추가 하고 싶을때 
docker-compose.yaml을 통해 peer에 대한 생성 내용과 인증서 위치 등 정보를 추가 하면 됨 
tools/network에서 git pull 하면 fastnet이라는 폴더가 clone 됨 
(orderer Template 에 count-> peer 개수 , Users: org에 접근할 수 있는 사용자 )
/org 추가 후 channel 추가 후 chaincode 추가 하여 api 호출은 readme 해주심
managementapi 에서 genesis.block이랑 channel.tx가 쓰이게 된다. 
(직접 orderer한테 channel 만들고 할수 있다.)
docker-compose.yaml에서 cli 컨테이너에서 필요하다는것을 알수 있음 
(관리 목적의 컨테이너)