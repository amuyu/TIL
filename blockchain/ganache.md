# Ganache
Ganache 는 이더리움 개발을 위한 개인 blockchain 이다.

# 시작하기
[Ganache](http://truffleframework.com/ganache/) website 에서 다운로드해서 사용할 수 있다.


# Ganache-cli
Ganache CLI 는 Truffle 에서 개발한 이더리움 개발 툴이다.
Ganache 의 command line 버전으로 이더리움 개발을 위한 개인 blockchain 이다.

Ganache CLI 는 ethereumjs 를 사용한다. client 동작을 시뮬레이션하고
이더리움 Application 빠르고 쉽고 안전하게 만들 수 있다.

# 설치
`ganache-cli` 는 npm 로 배포된다.
```sh
npm install -g ganache-cli
```
# Methods
bzz_hive (stub)
bzz_info (stub)
debug_traceTransaction
eth_accounts
eth_blockNumber
eth_call
eth_coinbase
eth_estimateGas
eth_gasPrice
eth_getBalance
eth_getBlockByNumber
eth_getBlockByHash
eth_getBlockTransactionCountByHash
eth_getBlockTransactionCountByNumber
eth_getCode (only supports block number “latest”)
eth_getCompilers
eth_getFilterChanges
eth_getFilterLogs
eth_getLogs
eth_getStorageAt
eth_getTransactionByHash
eth_getTransactionByBlockHashAndIndex
eth_getTransactionByBlockNumberAndIndex
eth_getTransactionCount
eth_getTransactionReceipt
eth_hashrate
eth_mining
eth_newBlockFilter
eth_newFilter (includes log/event filters)
eth_protocolVersion
eth_sendTransaction
eth_sendRawTransaction
eth_sign
eth_syncing
eth_uninstallFilter
net_listening
net_peerCount
net_version
miner_start
miner_stop
personal_listAccounts
personal_lockAccount
personal_newAccount
personal_unlockAccount
personal_sendTransaction
shh_version
rpc_modules
web3_clientVersion
web3_sha3


# docker
docker run -d -p 8545:8545 trufflesuite/ganache-cli:latest


## 참고
[testrpc](https://github.com/trufflesuite/ganache-cli)
