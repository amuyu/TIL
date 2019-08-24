# 강의
강의 내용을 토대로 작성

# run
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
- docker-compose -f docker-compose-cli.yaml up -

# 대략적인 순서
1. 네트워크 인증서 생성
cryptogen 을 이용해 인증서 발급
`crypto-config.yaml` 설정 파일 이용
```bash
cryptogen generate --config=crypto-config.yaml
```

2. 제네시스 블록 생성
configtxgen 을 이용해 제네시스 블록 생성
`configtx.yaml-TwoOrgsOrdererGenesis` 파일 참조
genesis.block 생성
> configtx.yaml 파일에 보면 각 peer들의 MSPDir를 설정하고 있는데 Fabric-ca 를 사용할 경우는?
```bash
configtxgen -profile TwoOrgsOrdererGenesis -channelID byfn-sys-channel -ouputBlock ./genesis.block
```
> 여기서 입력하는 channelID 는 뒤에서 생성하는 channelID 와 같지 않게 한다.

3. 채널 설정 트랜잭션 생성
configtxgen 을 이용해 채널 설정 트랜잭션 생성
`configtx.yaml-TwoOrgsChannel` 파일 참조
channel.tx 생성
```bash
configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel.tx -channelID mychannel 
```

4. 각 조직의 앵커 피어 정보 트랜잭션 만들기
configtxgen 을 이용
`configtx.yaml-TwoOrgsChannel` 파일 참조
```bash
configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./Org1MSPanchors.tx -channelID mychannel -asOrg Org1MSP

configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./Org2MSPanchors.tx -channelID mychannel -asOrg Org2MSP
```

5. 네트워크 Up
docker-compose 등을 사용해 이미지 업

6. 채널 만들기
채널 생성 `channel.tx` 사용

7. 채널에 Join
각 피어별 Join
`peer channel list` 조인된 채널의 리스트를 확인할 수 있음

8. 앵커 피어 업데이트
각 조직에 앵커 피어 업데이트

# ref
[가이드](https://hyperledger-fabric.readthedocs.io/en/latest/build_network.html)