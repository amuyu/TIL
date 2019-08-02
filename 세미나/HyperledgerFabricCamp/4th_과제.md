# 서버 구성
peer0.org1.example.com
peer1.org1.example.com
peer0.org2.example.com
peer1.org2.example.com
orderer.example.com
cli


# getBalance 호출 과정 살펴보기
- `blockchain/transaction/transaction.js` 를 호춣한다.
- hlf client 에서 adminClient 를 호출하고 channel 정보를 가져온다.
- channel 에 있는 chaincode 를 실행하고 결과를 조회한다.

=> hlf client 에 network 설정이 필요함
=> channel 과 chaincode 설정이 필요한다.
=> amdinClient 는 Org1-connection-profile-path

# TODO
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


invoke 는 에러 발생
Transaction ba5ce66d3af67afa357e56e32f0e0b631739bfcea36671a7164c71fd99f7bbf3 has status of ENDORSEMENT_POLICY_FAILURE in blocl 5


peer chaincode invoke -o orderer.example.com:7050 --tls true --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n mycc --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses peer0.org2.example.com:9051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"Args":["invoke","a","b","10"]}'


```sh

peer chaincode query -C mychannel -n mycc -c '{"Args":["query","a"]}'

docker exec -it cli bash
peer chaincode install -p chaincodedev/chaincode/sacc -n mycc -v 0
peer chaincode instantiate -n mycc -v 0 -c '{"Args":["a","10"]}' -C myc

openssl ec -in e6f545a1422c3fe2e4156b7e9c19316955dea6b639d6e7e2722b4cb6685074f2-priv -pubout > key.pub
read EC key
```






# q&a
configtx.yaml?

