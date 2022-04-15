# QuickStart
baob 기준으로 설명되어 있음


# faucet
[baobab-faucet](https://baobab.wallet.klaytn.com/access?next=faucet)

# launch an EN
## binary file 설치
[download](https://docs.klaytn.com/node/download/v1.0.0)
```bash
$ tar zxf ken-baobab-vX.X.X-X-darwin-amd64.tar.gz
$ export PATH=$PATH:$PWD/ken-darwin-amd64/bin
```
## blockchain data directory
`kend_home` 이 base directory 인듯.. 변경 가능하겠지? 밑에 config file 에서 `DATA_DIR` 로 설정
```bash
$ mkdir -p ~/kend_home
```

## genesis.json
download genesis.json
```sh
curl -X GET https://packages.klaytn.net/baobab/genesis.json -o genesis.json
```
e. Before starting an EN, it is necessary to initialize the genesis block using ken and genesis.json
```sh
ken init --datadir ~/kend_home genesis.json
```

# Configuration EN
configuration file : `ken-xxxxx-amd64/conf/kend.conf`
```conf
# cypress, baobab is only available if you don't specify NETWORK_ID.
NETWORK="baobab"
# if you specify NETWORK_ID, a private network is created.
NETWORK_ID=
...
RPC_API="klay,net" # net module should be opened for truffle later on.
...
DATA_DIR=~/kend_home
```

## launching EN
```bash
$ kend start
 Starting kend: OK

// check status
$ kend status
 kend is running

// check log
$ tail -f ~/kend_home/logs/kend.out
```

# Top up your Account
## Attaching to the Console
Klaytn Endpoint Node comes with JavaScript console.
```bash
$ ken attach ~/kend_home/klay.ipc
```
Type klay or personal to get the list of available functions.

## Create a new account
```bash
> personal.newAccount()
Passphrase:  # enter your passphrase
Repeat passphrase:
"0x75a59b94889a05c03c66c3c84e9d2f8308ca4abd" # created account address
```
생성한 account 는  `~/kend_home/keystore/` 에 저장된다.

## Unlocking the account
아마 ethereum 에서 계정 풀어주는거랑 똑같은듯?
address 는 0x 를 붙여도 됨
```bash
> personal.unlockAccount('75a59b94889a05c03c66c3c84e9d2f8308ca4abd') # account address to unlock
Unlock account 75a59b94889a05c03c66c3c84e9d2f8308ca4abd
Passphrase: # enter your passphrase
true
```

## Getting testnet KLAY from the baobab faucet
https://baobab.wallet.klaytn.com 에 접속해서 baobab 5 KLAY 를 얻을 수 있다.
Go to "KLAY Faucet" from the left pane menu, and click the "Run Faucet" button to get 5 KLAY.
You can run the KLAY Faucet once every 24 hours.

## Checking balance
Sync 는 하지 않아서 로컬에서 조회하면 위 wallet 에서 받은 klay 가 조회 안됨...
```bash
> klay.getBalance('75a59b94889a05c03c66c3c84e9d2f8308ca4abd') # enter your account address
1e+21  # 1000 KLAY
```

# Installing caver-js
프로젝트 추천하는 경로
```bash
mkdir $HOME/klaytn
```
caver-js 는 JSON RPC framework 임 web3.js와 같은 라이브러리
프로젝트에서 다음과 같이 설치한다.
```bash
$ npm init # initialize npm at the klaytn project directory
$ npm install caver-js
# If you already installed caver-js, please update it to the latest version.
$ npm cache clean --force # initialize npm cache
$ npm install caver-js@latest # update caver-js to the latest version
# version update 할 때, 에러가 발생하면 
$ rm /Users/username/klaytn/node_modules/websocket/.git
```

web3.js 가 `web3.eth...` 인 것처럼 `caver.klay...` 로 호출한다.

# Installing Truffle
현재 klaytn 는 truffle 4.1.15 까지 지원한다.

- Truffle repository - https://github.com/trufflesuite/truffle
- Truffle documents - https://truffleframework.com/docs

globally install truffle
```sh
$ sudo npm install -g truffle@4.1.15
$ cd /usr/local/lib/node_modules/truffle
$ sudo npm install solc@0.4.24
$ cd -
```

locally install trueffle
```sh
# Assuming you are in $HOME/klaytn/.
$ npm install truffle@4.1.15
$ cd node_modules/truffle
$ npm install solc@0.4.24
$ cd -
$ ln -s node_modules/truffle/build/cli.bundled.js truffle
$ export PATH=`pwd`:$PATH
```

# Installing vvisp
vvisp is an easy-to-use cli tool/framework for developing smart contracts, provided by HEACHI LABS.

- vvisp repository - https://github.com/HAECHI-LABS/vvisp
- vvisp document - https://github.com/HAECHI-LABS/vvisp/blob/dev/README_KLAYTN.md

```sh
$ npm install -g @haechi-labs/vvisp
# or if you use yarn
$ yarn global add @haechi-labs/vvisp
```

# EN
EN의 상태를 확인하는 유용한 API는 다음과 같습니다.
```
klay.blockNumber (최신 블록 번호를 가져옵니다)
net.peerCount (현재 연결된 Klaytn 노드의 수를 가져옵니다)
```
addPeer
```
admin.addPeer("kni://0412e1fe709512a2ae7387eafba0036c07195a8b63921ce8bd708b1c02db21366efcc690bc50801eb6c74facad42f8d801db289ea3882fe37e56829ad73e8a81@54.180.16.123:32323?ntype=en")
```


# ref
[install guide](https://docs.klaytn.com/node/en/installation/linux/enlinuxconfig)
[endpoint 실행](https://ko.docs.klaytn.com/getting-started/quick-start/launch-an-en#download-and-initialize-an-endpoint-node-en)