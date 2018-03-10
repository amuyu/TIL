
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


# filtervar blockFilter = web3.eth.filter('latest');
watch 를 사용해서 새로운 block 을 확인
blockFilter.watch(function(error, blockHash) {
    var block = web3.eth.getBlock(blockHash);
    appendLog('New Block('+block.number+')['+block.hash+'] / ' + block.transactions.length + ' TXs');
});
출처: http://goodjoon.tistory.com/260 [Good Joon]


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

## 11
코인마켓 https://coinmarketcap.com
api

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


# error
## infura unsupported methods
> INFURA returns 405 for unsupported methods, which include the filter methods. At the moment we don't support creating filters, which store state in the Ethereum client, for our public infrastructure.

filters are not supported on Infura. (https://docs.web3j.io/filters.html)


https://ethereum.stackexchange.com/questions/16174/retrieving-logs-using-filter-is-not-working
