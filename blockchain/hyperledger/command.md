
# chaincode list 확인
```sh
// local
docker exec -e "CORE_PEER_LOCALMSPID=MallOrgMSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/crypto/user/Admin@mall/msp" peer3.customs.com peer chaincode list --installed
docker exec -e "CORE_PEER_LOCALMSPID=CustomsOrgMSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/crypto/user/Admin@customs/msp" peer3.customs.com peer chaincode list --installed
// aws 
docker exec -e "CORE_PEER_LOCALMSPID=StoreOrgMSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/users/Admin@store/msp" peer2.store.com peer chaincode list --installed
```

# instantate 
```sh

docker exec -e "CORE_PEER_LOALMSPID=MallOrgMSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/mall/users/Admin@mall/msp" -e "CORE_PEER_ADDRESS=peer0.mall.com:7051" cli peer chaincode instantiate -o orderer.customs.com:7050 -C customs -n chain2 -v v1.0 -c '{"Args":["init"]}'

-P "OR('MallOrgMSP.member', 'CjOrgMSP.member', 'CustomsOrgMSP.member', 'StoreOrgMSP.member', 'OrdererOrgMSP.member')"
```