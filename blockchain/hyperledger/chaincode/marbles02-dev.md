# cmd
```sh
CORE_PEER_ADDRESS=peer:7052 CORE_CHAINCODE_ID_NAME=marble:0 ./marble

peer chaincode install -p chaincodedev/chaincode/marble -n marble -v 0
peer chaincode instantiate -n marble -v 0 -c '{"Args":[]}' -C myc

peer chaincode invoke -n mycc -c '{"Args":["set", "a", "20"]}' -C myc
peer chaincode query -n mycc -c '{"Args":["query","a"]}' -C myc

peer chaincode invoke -C myc -n marble -c '{"Args":["initMarble","marble1","blue","35","tom"]}'
peer chaincode invoke -C myc -n marble -c '{"Args":["initMarble","marble2","red","50","tom"]}'
peer chaincode invoke -C myc -n marble -c '{"Args":["initMarble","marble3","blue","70","tom"]}'
peer chaincode invoke -C myc -n marble -c '{"Args":["transferMarble","marble2","jerry"]}'
peer chaincode invoke -C myc -n marble -c '{"Args":["transferMarblesBasedOnColor","blue","jerry"]}'
peer chaincode invoke -C myc -n marble -c '{"Args":["delete","marble1"]}'


peer chaincode query -C myc -n marble -c '{"Args":["readMarble","marble1"]}'
peer chaincode query -C myc -n marble -c '{"Args":["getMarblesByRange","marble1","marble3"]}'
peer chaincode query -C myc -n marble -c '{"Args":["getHistoryForMarble","marble1"]}'

# Rich Query (Only supported if CouchDB is used as state database):
peer chaincode query -C myc -n marble -c '{"Args":["queryMarblesByOwner","tom"]}'
peer chaincode query -C myc -n marble -c '{"Args":["queryMarbles","{\"selector\":{\"owner\":\"tom\"}}"]}'

# Rich Query with Pagination (Only supported if CouchDB is used as state database):
peer chaincode query -C myc -n marble -c '{"Args":["queryMarblesWithPagination","{\"selector\":{\"owner\":\"tom\"}}","3",""]}'

```

# http api
http://localhost:5984/myc_marble/_design/indexOwnerDoc/_view/indexOwner