# transaction 어떻게 선택되나 이해하기 (block)
Ethereum 은 블록체인으로서 여러개의 블록으로 구성되며 각 블록은 트랜잭션을 포함하고 체인에서 서로 연결된다.
블록(1433)이 마이닝 된 시점에서, 블록(1434)의 마이닝이 시작된다. 블록(1434)는 공백으로 시작한다.
광부는 마이닝 프로세스를 시작하고 블록(1434)에 삽입할 트랜잭션들을 `Transaction pool` 에서 선택한다.
블록(1434)이 성공적으로 마이닝되면, 블록 체인의 일부를 형성하고 관련 트랜잭션은 `Transaction pool`에서 제거된다.

광부가 `Transaction pool`을 조사하고 동일한 거래를 볼 때, 높은 가스 가격을 가진 거래를 선택한다.
그러면 블록을 채굴할 때 광부에게 더 많은 돈을 지불한다.

# tx 를 가지고 status 확인하기
mined 되기 전 pending 상태의 transaction 은 높은 gasprice transaction 으로 대체될 수 있다.
pending 상태에서 새로운 transaction 이 선택되면 이 전의 transaction 은 없어진다.

그런데 테스트 중 이상한 점...
(1) '0xb3013ae10772f090db9c1ab2208eeee2755b1c13c0731dcb45714fdfb491bfde' tx 발생시킨 후, gasPrice 를 올려서
(2) '0xda7d4cc1baf5e0691f2c1eaef6518c3c94f9b3133f37d4544876d0a37b13d045' 를 발생시켰다.

tx를 조회해보면 (1)은 조회가 안되고 (2)는 조회가 된다.
pending list 를 확인하기 위해서 block 을 조회해보면
```js
web3.eth.getBlock('pending').transactions
```
(1)만 조회가 된고 (2)는 조회가 안된다......
아직 transactions 가 변경되지 않은 상태로 시간이 조금 지나면 (1)은 제거되고 (2)만 조회된다.
그리고 geth console 에서 pendingTransactions 에는 아무것도 없다

# 'pending' block 의 transactions 와 eth.pendingTransactions 다르다.
// https://ethereum.stackexchange.com/questions/6720/eth-pendingtransactions-vs-eth-getblockpending-transactions
eth.pendingTransactions 는 local geth node 에 있는 transactions 이다. 이 transactions 은 block 에 mined 되지 않았다.
eth.getBlock('pending').transactions 은 hypothetical block(가상)에 포함된 transactions 이다.
그래서 순서 상으로 보면 eth.pendingTransactions > eth.getBlock('pending').transactions 이 된다.

send 후 return 받은 tx 가 조회되지 않으면 해당 transaction 은 실패한것임

# 질문?
transactionRoot 는 무엇일까?
byzantiumBlock?
eth.getBlock('pending') 은 아직 mined 되지 않은 block 을 말한다.?
## 비어있는 이전에 mined 된 block 에는 아무것도 들어갈 수 없나?
https://ethereum.stackexchange.com/a/9058
과거 블록은 불변합니다
## gas price 를 매우 적게 하면 어떻게 되는가?
https://ethereum.stackexchange.com/questions/15883/understanding-transaction-broadcast-better/26409
매우 적게 하면 block 에 들어가지 않는다. `eth.pendingTransactions` 에 만 존재한다.
etherscan pending transactions 에서는 볼 수 없다.
## etherscan 에 pending transaction 이 보이지 않는 이유
https://ethereum.stackexchange.com/a/26409
etherscan 에 transaction 이 broadcast 되지 않았다.
etherscan 에서 100% pending transaction 을 보여주지 않는다.
Another limitation is the pending transaction pool of your peers.
Remember the infamous ICOs clogging up the network. If your peers have a full queue they may just drop your transaction.

# sendtransaction 의 data 가 많을 때
hex length 12042, 6922 (성공 5642)
errors.js:38 Uncaught (in promise) Error: gas required exceeds allowance or always failing transaction

# get 으로 가져올 수 있는 data 수
1725 시간만 잇으면 되는듯? 4079, 4751
7000개 정도 일 때,, net::ERR_INSUFFICIENT_RESOURCES

# cancel transaction
https://jakubstefanski.com/post/2017/10/ethereum-cancel-pending-transaction/

# error
insufficient funds for gas * price + value
돈이 없을 때,,

# nonce
nonce update 관련 내용
https://github.com/MetaMask/metamask-extension/issues/1127
## 같은 nonce 에 data 가 다른 경우
replacement transaction underpriced
https://ethereum.stackexchange.com/questions/27256/error-replacement-transaction-underpriced
gasprice 가 크면 에러가 안나고 gasprice 큰게 선택됨
이전 tx 는 선택 안됨
## 같은 nonce 인 data 가 같은 경우
known transaction
## 이전 nonce 인 경우
nonce too low
## 건너뛴 nonce 인 경우
기다리기....
// https://ethereum.stackexchange.com/questions/2808/what-happens-when-a-transaction-nonce-is-too-high


# transaction 가져오기
web3.eth.getTransaction(transactionHash [, callback])
web3.eth.getTransactionCount(address [, defaultBlock] [, callback])
web3.eth.getTransactionCount("0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe")

# Transactinos 가져오기
# 방법 1
block number 를 통해 가져온다 for문 돌리기, 근데 block 이 과연 연속한가?
block 에서 transactions 를 가져와 추가추가추가
[Script To Find Transactions To/From An Account](https://ethereum.stackexchange.com/questions/2531/common-useful-javascript-snippets-for-geth/3478#3478)
[githubissue](https://github.com/ethereum/web3.js/issues/580)

block filter 사용
filter 를 사용해봤는데 바로 리턴 되지 않음
var options = {address: "0xf2cc0eeaaaed313542cb262b0b8c3972425143f0"};
var myfilter= web3.eth.filter(options);
var options = {fromBlock: 0, toBlock: 'latest', address: "0xf2cc0eeaaaed313542cb262b0b8c3972425143f0"};
var myfilter = web3.eth.filter(options);
[githubissue](https://github.com/ethereum/go-ethereum/issues/1897)

# 방법 2
[reddit 질문 답변](https://www.reddit.com/r/ethereum/comments/3sz2n4/get_all_transactions_for_address_as_a_json_object/)

# etherscan site
https://ropsten.etherscan.io/txs?a=0x0318247cb34f134f3cf49e97647227dc2d75abe8&p=2
page 를 넘기는데,,, 이건 조회된거에서 나누어 가져오는 걸 수 있음


# filter
filter.watch 를 사용해서 새로운 block 을 확인
```js
// http://goodjoon.tistory.com/260 [Good Joon]
var blockFilter = web3.eth.filter('latest');
blockFilter.watch(function(error, blockHash) {
    var block = web3.eth.getBlock(blockHash);
    appendLog('New Block('+block.number+')['+block.hash+'] / ' + block.transactions.length + ' TXs');
});
```


# eth 명령
// shows transaction pool
txpool.status
// number of pending txs
eth.getBlockTransactionCount("pending");
// print all pending txs
eth.getBlock("pending", true).transactions
[wiki](https://github.com/expanse-org/go-expanse/wiki/Contracts-and-Transactions)

# etherscan 에서 transactions 결과
To list all regular transactions: https://etherscan.io/txs?a=0xbb9bc244d798123fde783fcc1c72d3bb8c189413
To list all "Internal Transactions": https://etherscan.io/txsInternal?a=0xbb9bc244d798123fde783fcc1c72d3bb8c189413

# ehterchain-light
parity node 여야 함
routes 폴더에 view에 대한 기능들이 있음
address length로 block(<22), tx(==66), address(==42) 구분한다.
light.etherchain.org

# explorer (project)
block exlorer
transaction, block, adress 에 대한 구분이 가능하다.
address 조회하면 balance 를 확인할 수 있다.
block 내 transaction 을 조회 가능하다.
Supported Ethereum backend nodes: Parity (Geth is currently not supported as it does not allow account and received/sent tx enumeration)
https://github.com/gobitfly/etherchain-light

# ehterscan apis
getList api
http://api.etherscan.io/api?module=account&action=txlist&address=0xbeef281b81d383336aca8b2b067a526227638087&sort=asc

# 참고
[etherscan-api](https://etherscan.io/apis)
[npm-etherscan](https://www.npmjs.com/package/etherscan-api)
[explorer](https://github.com/etherparty/explorer)
[etherscan-api](https://sebs.github.io/etherscan-api/)
[etherchain-node](https://github.com/sgsshankar/etherchain-node)


# gettransactionsaccount
```
function getTransactionsByAccount(myaccount, startBlockNumber, endBlockNumber) {
  if (endBlockNumber == null) {
    endBlockNumber = eth.blockNumber;
    console.log("Using endBlockNumber: " + endBlockNumber);
  }
  if (startBlockNumber == null) {
    startBlockNumber = endBlockNumber - 1000;
    console.log("Using startBlockNumber: " + startBlockNumber);
  }
  console.log("Searching for transactions to/from account \"" + myaccount + "\" within blocks "  + startBlockNumber + " and " + endBlockNumber);

  for (var i = startBlockNumber; i <= endBlockNumber; i++) {
    if (i % 1000 == 0) {
      console.log("Searching block " + i);
    }
    var block = eth.getBlock(i, true);
    if (block != null && block.transactions != null) {
      block.transactions.forEach( function(e) {
        if (myaccount == "*" || myaccount == e.from || myaccount == e.to) {
          console.log("  tx hash          : " + e.hash + "\n"
            + "   nonce           : " + e.nonce + "\n"
            + "   blockHash       : " + e.blockHash + "\n"
            + "   blockNumber     : " + e.blockNumber + "\n"
            + "   transactionIndex: " + e.transactionIndex + "\n"
            + "   from            : " + e.from + "\n"
            + "   to              : " + e.to + "\n"
            + "   value           : " + e.value + "\n"
            + "   time            : " + block.timestamp + " " + new Date(block.timestamp * 1000).toGMTString() + "\n"
            + "   gasPrice        : " + e.gasPrice + "\n"
            + "   gas             : " + e.gas + "\n"
            + "   input           : " + e.input);
        }
      })
    }
  }
}
```

https://ethereum.stackexchange.com/questions/2531/common-useful-javascript-snippets-for-geth/3478#3478

# filter
```
eth.filter({fromBlock:0, address:"0x58a78198d6e052299b79a613c18cd5975b8a095d"}).get()
```
eth_getLogs

# filter id 사용
eth_newFilter, eth_getFilterLogs 사용
https://ethereum.stackexchange.com/questions/8069/why-use-filters-eth-getlogs-vs-eth-newfilter

# check transaction status
https://ethereum.stackexchange.com/questions/28077/how-do-i-detect-a-failed-transaction-after-the-byzantium-fork-as-the-revert-opco
byzantiumBlock 'genesis.json'

https://ethereum.stackexchange.com/questions/6002/transaction-status
The .blockNumber field will be null until the transaction is included into a mined block.

# error
## infura unsupported methods
> INFURA returns 405 for unsupported methods, which include the filter methods. At the moment we don't support creating filters, which store state in the Ethereum client, for our public infrastructure.

filters are not supported on Infura. (https://docs.web3j.io/filters.html)


https://ethereum.stackexchange.com/questions/16174/retrieving-logs-using-filter-is-not-working



# 참고
[Checking or Replacing a TX After it's Been Sent](https://myetherwallet.github.io/knowledge-base/transactions/check-status-of-ethereum-transaction.html)
[gas 사용 정보](https://ethgasstation.info/)
[Releasing Stuck Ethereum Transactions](https://medium.com/@jgm.orinoco/releasing-stuck-ethereum-transactions-1390149f297d)
[Life cycle transaction](https://medium.com/blockchannel/life-cycle-of-an-ethereum-transaction-e5c66bae0f6e)
  - tip : There are many services like Etherscan and Infura you can use to broadcast your signed transaction to the network.
Another safe solution is to use a hardware wallet such as Ledger or Trezor. These wallets store your private key and the code to sign the transaction is programmed in to the hardware itself.
[what is gas](https://myetherwallet.github.io/knowledge-base/gas/what-is-gas-ethereum.html)
