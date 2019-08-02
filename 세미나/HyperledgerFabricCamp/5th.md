# api server 기동
실행 후, 로그 보기

로그로 정보가 찍히는데 `NetworkConfig101.js` 가 중요
이 파일로그는 network-config.yaml 설정에서 정보를 읽어와서 셋팅하는 정보들임

`FileKeyValueStore.js` 로 찍히는 정보들은 key 정보들을 로드함
`kv` 폴더에 필요한 인증서 정보들이 없는 경우, 자동으로 복사해옴

# first-network 구동
- `byfn.sh` 하는 일 : 인증서 생성, genesisblock, channel 등등 script
- -m option 은 volume 을 static 하게 생성, 생성되는 파일들을 삭제하지 않음 (인증서 등)
- byfn -m up



# tip
- docker swarm?

- fabric 에서 version up 을 했을 때?
fabric 에서 git pull 받아보니 1.4.2 을 release 함, 요걸 테스트 하고 싶을 때
docker image 도 삭제 docker rmi -f $(docker images -a)
fabric 소스코드에서 `scripts/bootstrap.sh' 로 바이너리와 docker iamge 다운로드
fabric-samples 에도 반영이 필요함, git pull (log 를 통해 1.4.2 확인)
fabric 에서 바이너리 생성 => make native
fabric-samples 에서 byfn.sh 로 실행

- bootstrap.sh 에서 설정에 따라 docker, binary 등 따로 받을 수 있음

- 1.4.2 release note 
    raft 가 안정화가 됨
- fabric, fabric-samples git pull
- docker image 지우고 fabric/scripts/bootstrap.sh 실행


- hyper ledger 상황 (개인적인 의견)
    - 현재 상황은 낙관적이지 않다
    - 원장, 자율적으로 돌아가는 ?
    - 유튜브에서 하이퍼 레저로 ICO를 하는 방송을 봄, 위험?..
    - 토큰이 개발 되고, 경제쪽과 연관이 되면 좀 더 괜찮아 지지 않을까? 

- proposal 에서 예를 들어 3개를 요청했는데 2개는 ok, 1개가 fail 이라면?
    - api 에서는 보통 1개가 fail 이라면 sendTransaction 을 하지 않음
    - 근데 수정해서 orderer 한테 보낼 수 있고 동작함
    - 그렇다면 VSCC는 ?

- chaincode list 조회
peer chaincode list -C mycc --installed