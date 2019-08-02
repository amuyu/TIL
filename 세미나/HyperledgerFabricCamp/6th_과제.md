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

# 프로비저닝 실습

- tools, 폴더 구조 변경 프로젝트에서 직접 셋팅한다고 할 때, 필요
- server/common/hlf 에 first-network 추가
    - first-network/crypto-config 복사
    - org1-first_network.yaml (basic 복사)
        - kv path 변경 (밑에서 폴더 추가)
    - network-config-first_network.yaml (basic 복사)
        - mychannel 만 남기기
        - Org1 의 adminPrivateKey 변경
        - org1 의 signeCert 변경
        - orderers 의 tlsCACerts 변경
        - peers 의 tlsCACerts 변경
        - certificateAuthorities 의 tlsCACerts 변경
- kv/first-network 추가 (basic-network 복사)
    - Org1MSPAdmin 에 cert 수정 (crypto-config/org1/msp/admin)
    - org1/users/admin/keystore => key 폴더에 추가 priv
    - openssl 명령으로 pub 파일 추가
- Api 변경