
# eth_accounts
```sh
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}' localhost:8545
```

# eth_getBalance
Parameters
DATA, 20 Bytes - address to check for balance.
QUANTITY|TAG - integer block number, or the string "latest", "earliest" or "pending", see the default block parameter
```sh
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x77930366442e0d4f29a35e96e006c69b162ef18a", "latest"],"id":1}'
```

# sending eather
https://github.com/ethereum/go-ethereum/wiki/Sending-ether


# unlock
personal.unlockAccount('0x907e71cf80248926bf64a32f702fc108ca5a6f58','password',60)
https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts

# newAccount
web3.eth.personal.newAccount("passphrase")

# recover
web3.eth.personal.ecRecover(dataThatWasSigned, signature [, callback])

# coinbase
miner.setEtherbase(eth.accounts[0])

# encrypt
1.0 에 있는 함수
web3.eth.accounts.encrypt(privateKey, password);
return : The encrypted keystore v3 JSON.

# decrypt
web3.eth.accounts.decrypt(keystoreJsonV3, password);

# transaction
web3.eth.getTransaction(transactionHash [, callback])

web3.eth.getTransactionCount(address [, defaultBlock] [, callback])
web3.eth.getTransactionCount("0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe")




# 참고
[web3js](http://web3js.readthedocs.io/en/1.0/web3-eth-personal.html)
[stable version](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethcontract)
[etherchain](https://github.com/gobitfly/etherchain-light)
[erc20](https://github.com/gobitfly/erc20-explorer)
[json-rpc wiki](https://github.com/ethereum/wiki/blob/78990aca283f67ceb26ed70c8eb82008424aaf64/JSON-RPC.md#eth_newfilter)
[ethjsonrpc](https://github.com/ConsenSys/ethjsonrpc)
## Metamask
[web3-stream-provider](https://github.com/kumavis/web3-stream-provider)
[provider-engine](https://github.com/MetaMask/provider-engine)
