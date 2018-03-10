# install
brew install geth
path : /usr/local/Cellar/geth/1.7.3

# 초기화
geth 초기화한다.
```sh
geth 초기화
geth init CustomGenesis.json --datadir chain-data
```

# json 파일 예제
CustomGenesis.json
https://ethereum.stackexchange.com/questions/2376/what-does-each-genesis-json-parameter-mean/2377#2377
```json
{
    "nonce": "0x0000000000000042",
    "timestamp": "0x0",
    "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "extraData": "0x0",
    "gasLimit": "0x8000000",
    "difficulty": "0x400",
    "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "coinbase": "0x3333333333333333333333333333333333333333",
    "alloc": {
    }
}
```

# 실행
```sh
# geth private 실행 명령 best
geth --identity ether-private --datadir /ethereum/data --port 30303 --rpc --rpcaddr 0.0.0.0 --rpcport 8545 --rpccorsdomain "*" --nodiscover --networkid 2018 --mine --minerthreads=1 --etherbase '0x907e71cf80248926bf64a32f702fc108ca5a6f58' --rpcapi "db,eth,net,web3,minor,personal" console 2>> geth.log

# geth 
geth --identity ether-private --datadir /ethereum/data --port 30303 --rpc --rpcaddr 0.0.0.0 --rpcport 8545 --rpccorsdomain "*" --nodiscover --networkid 2018 --mine --minerthreads=1 --etherbase '0x907e71cf80248926bf64a32f702fc108ca5a6f58' --rpcapi "db,eth,net,web3,minor,personal" --targetgaslimit '9999999999' console 2>> geth.log

geth --identity ether-private --datadir ./data --port 30303 --ipcpath ~/.local/share/io.parity.ethereum/jsonrpc.ipc --rpc --rpcaddr 0.0.0.0 --rpcport 8545 --rpccorsdomain "*" --nodiscover --networkid 2018 --mine --minerthreads=1 --etherbase '0x907e71cf80248926bf64a32f702fc108ca5a6f58' --rpcapi "db,eth,net,web3,minor,personal" console 2>> geth.log


geth --datadir=path/to/bc_dir \
     --ipcpath ~/Library/Ethereum/geth.ipc \
     --networkid 2061 \
     --rpc --rpcapi "db,eth,net,web3,personal,admin,miner,debug,txpool"

geth --ipcpath ~/Library/Ethereum/geth.ipc \
     --networkid 2061 \
     --rpc --rpcapi "db,eth,net,web3,personal,admin,miner,debug,txpool"
```

## option
‘ — datadir’ path to the private blockchain location, note other info is also stored here.
‘ — ipcpath’ added as i wanted to use the Ethereum Wallet Executable but could not override where it looked for the geth.ipc file, hence it failed to start, so i used the ipcpath flag to place geth.ipc in the location it was looking for. By default geth was creating geth.ipc in the datadir.
‘ — rpc’ support the rpc protocol, by default will be on localhost:8545
‘ — mine’ and ‘ — minerthreads 1’ started mining manually later from the geth console so did not add flags.
‘’ — networkid 2061’ gave a specific id
### Networking option
- nodiscover : Disables the peer discovery mechanism
### Miner Option
- port value : Network listening port (default: 30303)
- mine : Enable mining
- minerthreads value : mining 하기 위한 cpu threads number (default:8)
- etherbase value : block mining 보상을 받을 address 설정 (default:first account created)
- targetgaslimit value : mine 을 위한 gas floor 을 설정한다. (default: 4712388)
- extradata value : Block extra data set by the miner (default = client version)

# console 접속
geth attach http://127.0.0.1:8545
geth 를 실행할 때 `console` 을 입력해도 된다.

# 계정 생성
personal.newAccount()
eth.accounts

### docker quick start
```sh
docker run -d --name ethereum-node -v /Users/amuyu/ethereum:/root \
           -p 8545:8545 -p 30303:30303 \
           ethereum/client-go --fast --cache=512
```

# error
## Unable to attach to remote geth


# 참고
[go-ethereum](https://github.com/ethereum/go-ethereum)
[Set up the private chain – miners](http://chainskills.com/2017/03/10/part-3-setup-the-private-chain-miners/)
[Command Line Option](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)
