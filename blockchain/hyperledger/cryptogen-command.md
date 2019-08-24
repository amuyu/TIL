command

```sh
cryptogen generate --config=crypto-config.yaml

configtxgen -profile TwoOrgsOrdererGenesis -channelID byfn-sys-channel -ouputBlock ./genesis.block

configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel.tx -channelID mychannel 

configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./Org1MSPanchors.tx -channelID mychannel -asOrg Org1MSP

configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./Org2MSPanchors.tx -channelID mychannel -asOrg Org2MSP
```