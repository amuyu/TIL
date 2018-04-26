컨트랙트 생성 가능
# private blockchain 과 mist 연동
geth 실행 시 default ipc path 를 지정하고
mist 실행 시 rpc 옵션을 준다.
```sh
// geth 실행
geth --dev --ipcpath chain-data/geth.ipc --rpc --rpccorsdomain "*" --rpcapi "db,eth,net,web3,personal,admin,miner,debug,txpool" --networkid 1012

geth --ipcpath chain-data/geth.ipc --rpc --rpccorsdomain "*" --rpcapi "db,eth,net,web3,personal,admin,miner,debug,txpool" --networkid 1012

// mist 실행
/Applications/Ethereum Wallet.app/Contents/MacOS/Ethereum\ Wallet --rpc ~/Library/Ethereum/geth.ipc
```
## 더 쉬운 실행
```
geth --dev --rpc --mine
./Ethereum\ Wallet --rpc http://localhost:8545
/Applications/Ethereum Wallet.app/Contents/MacOS/Ethereum\ Wallet --rpc http://localhost:8545
/Applications/Ethereum Wallet.app/Contents/MacOS/Ethereum\ Wallet --rpc http://52.78.119.26:8545
```

## Option
- rpc : path to node IPC socket file or http rpc host port
- reset-tabs : reset mist tabs to their default settings


# RichToken 테스트
Contract 배포
contract invest 실행
투자를 받고,,,, 투자 종료되면
Token 등록
contract 정보 가져오기
```js
var contract = web3.eth.contract({json.format}).at(contractAddress);
// contract 에 등록된 함수로부터 정보 호출
contract.getStart()
contract.getDeadline()
contract.getNow()
contract.salesStatus()  // 발행한 화폐 수
web3.eth.getBalance(contractAddress); // 투자받은 ether
```


## error
Fatal: Error starting protocol stack: listen udp :30303: bind: address already in use

When this pops up using Mist on Mac while opening your private network, then you have to specify the exact path to the ipc file. The exact path is printed when geth is started in the command line!

For GETH:

geth attach ipc:/path/to/the/file/geth.ipc

For MIST:

/Applications/Mist.app/Contents/MacOS/Mist --rpc <path to chaindata>/geth.ipc



현재



## error
err="waiting for transactions"
dev 로 실행했을 때, transactions 이 발생해야 block 을 체굴?
In dev mode, the node only mines if there are transactions. Make a transaction and the node will mine it.
