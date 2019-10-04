## chaincode encryption
enccc example 을 위해서는 아래 명령으로 패키지 추가를 해야 한다.
[Enccc example](https://github.com/hyperledger/fabric/tree/release-1.4/examples/chaincode/go/enccc_example) 을 참고한다.
`encryptAndPutState`, `getStateAndDecrypt` 등을 사용할 수 있다.
encc 를 돌려보기 위해 필요한 명령들
```bash
# chaincode path 에서
govendor init
govendor add +external
# 네트워크 up 하고 chaincode 실행 할 때,
CORE_PEER_ADDRESS=peer:7052 CORE_CHAINCODE_ID_NAME=encc:0 ./enccc_example
# chaincode 실행 후, cli 에서 명령 실행 시
peer chaincode install -p chaincodedev/chaincode/enccc_example -n encc -v 0
peer chaincode instantiate -n encc -v 0 -c '{"Args":[]}' -C myc
# chaincode 실행할 때
peer chaincode invoke -n encc -C my-ch -c '{"Args":["ENCRYPT","key2","value2"]}' --transient "{\"ENCKEY\":\"$ENCKEY\",\"IV\":\"$IV\"}"
```
