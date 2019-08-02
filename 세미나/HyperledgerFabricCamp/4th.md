# 목차
1. Review
1-1. nvm 설치
1-2. gulp 설치
1-3. Hyperledger 라이브러리 설치
1-4. Hyperledger 라이브러리 빌드 / 테스트
1-4-1. 테스트 코드 실행 (fabric-sdk-node)
- network : fabric-sdk-node/gulp docker-ready
- oerderer log : 
    test/fixtures/docker-compose/docker-compose-base.yaml 에서 orderer 의 loglevel debug 변경
    docker logs -f orderer.example.com
- create-channel : node test/integration/e2e/create-channel.js
- join-channel : node test/integration/e2e/join-channel.js
- updateAnchorPeers : node test/integration/e2e/updateAnchorPeers.js
- install-chaincode : node test/integration/e2e/install-chaincode.js
- instantiate : node test/integration/e2e/instantiate-chaincode.js
- invoke : node test/integration/e2e/invoke-transaction.js
- query : node test/integration/e2e/query.js
- upgrade : node test/integration/e2e/upgrade.js

2. HFC 연동
2-1. clone
[소스](https://github.com/simon0210/hyperledger-study02)
2-2. HFC
2-2-1. network (tools/network)
- ./0-network-start.sh (docker network 생성, docker image 삭제 필요, basic-network 구성)
- couchdb 접속 : http://url:5984/_utils/
- ./1-create-channel.sh 
- couchdb 확인 : channel 정보를 확인할 수 있음 (world_state)
- ./2-setup-chaincode.sh balance 0.1 (balance chain code install/instantiate)
    chain code 경로: (tools/chaincode)
- couchdb 확인 : chaincode 정보 확인
- npm run dev
- http://url:4000/ 접속해서 api 확인
- /balance/user 로 사용자 등록하고 transfer 테스트
2-3. hlf
`server/common/hlf/connection-profile` 에 네트워크 정보를 셋팅해야함
peer 나 orderer 의 인증서 정보가 있음
`network-config-basic_network.yaml` 에서 네트워크 디자인을 할 수 있다.
ex) 조직을 동적으로 추가한다고 하면 서버 내리고 yaml 파일을 수정하고 다시 시작해야한다.
미래에는 discovery 를 통해서 네트워크 구성에 대한 정보를 받아서 받은 정보를 기반으로 communication
hlf 설정이 예전에는 config.json 을 사용했는데 이제부터는 반드시 yaml 로 사용해야 한다!!

ex)
org 를 한개로 두고 CA 를 뺀 상태에서
sdk 상에서 admin 인증서로 user 를 생성할 수 있다.

2-4. api 서버 프레임워크 설치 및 api 구현
- server/common/api.yaml
- server/routes.js
- server/api/controller/example/router.js
- server/api/controller/example/controller.js
- server/api/services/example.service.js

3. First Network 실행 후, API 서버와 연동
First Network 실행 순서
- 인증서 생성
- genesis.block 생성
- HLF Network 업
- peer, org, CA
- query, invoke

3-1. First Network 실행
[가이드](https://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)
- ./byfn.sh -m up 
- configtxlator error 발생
- fabric 폴더 이동 fabric buid : make native
- ll .build/bin 로 확인
- ~/.bashrc 에 다음 추가 : PATH="/home/ubuntu/go/src/github.com/hyperledger/fabric/.build/bin":$PATH
- source ~/.bashrc 실행
- ./byfn.sh -m up : 다시 실행 (docker-compose-cli.yaml)
channel.tx, genesis.block, 인증서 없는 상태에서 실행하면 알아서 다 생성됨
정상 구동되는지 확인
- ./byfn.sh -m down (인증서 관련 clear? clear 를 하면 인증서가 없어서 안된다?)
- docker rm -f $(docker ps -aq)
- docker-compose -f docker-compose-cli.yaml up -d

3-2. api server 설정 변경
- api server 가 first-network 와 연동하려면 생성된 `crypt-config`, `channel-artifacts` 를 복사해 와야함
- network-config-first_network.yaml 파일 생성
네트워크 구성을 first-network 에 맞게 수정해야함 - peer, org, orderer, cli, msp, 인증서 설정
msp 는 docker-compose.yaml 과 configtx.yaml 참고
- api server `.env` 파일 수정


# TIP?
- channel 생성은 누구에게 날리나요? orderer 에게 날린다 (권한 정보를 들고있다)
- docker 상태를 안날리면 volume mount 가 필요함
    - ordere : /var/hyperledger/production/
- chaincode 관리를 어떻게 할까요? 업그레이드 했더니 체인코드 container 가 늘어남
- hyperledger network 구성은 계속 바뀌