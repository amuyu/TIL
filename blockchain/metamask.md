## block 조회
### RecentBlocksController
block 정보를 조회한다.

## check pending
`PendingTransaction` 을 사용해서 transaction status 를 업데이트 한다.
## PendingTransaction
### checkForTxInBlock
block 에서 transaction 을 조회한다
'block' 이라는 이벤트가 발생했을 때(transactions.js) 호출되는데
'block' 이벤트를 emit 하는 위치는 AccountTracker 밖에 없다.
(blockTracker.on('block') 으로 이벤트를 수신하는데 이름이 안 맞음, )


## transaction 하는 방법
실제 send는 어디서?
transactions.js, pending-tx-tracker.js
transactions 에서 생성한 ethquery 를 pending-tx-tracker에 넘기고
pending-tx-tracker에서 resubmit.

transactions.js publishTransaction 에서 send
```
publishTransaction (txId, rawTx)
```
transaction 과 관련된 데이터
txId, rawTx, txMeta, txHash

updateAndApproveTransaction > approveTransaction > publishTransaction



## SWcontroller
ui.js, proxy.js 에서 사용

## ui/app/components/pending-tx/confirm-send-ether.js
index.js(ui/app/components/pending-tx) 호출
sendTransaction < pending-tx/index.js < conf-tx.js/sendTransaction
  action.updateAndApproveTx 호출


## reducers/metamsk.js
store state 변경은 이곳에서
unapprovedTxs 는 app.js 에서 관리

## FullTxList
store 에 저장된 transactions 를 호출한다. metamask에서 발생한 transaction 을 말함
```js
// tx-state-manager
getFullTxList () {
    return this.store.getState().transactions
  }
```
store txlist 를 저장한다. 호출 위치는 addTx, updateTx, wipeTransactions
```js
// tx-state-manager
_saveTxList (transactions) {
    this.store.updateState({ transactions })
  }
```

### updateTransaction
actions.updateTransaction 를 호출해서 txlist 를 갱신
action 관리는 'send-v2-container.js' 에서 함, send-v2.js 가 있음
metamask 화면에서 send 버튼 선택일 듯

## tx-list-item 화면 tx-list-item.js
props 에서 transactionStatus 가 transaction 의 status를 표시함
tx-list 에서 renderTransactionListItem() 이 다시 그리기
tx-list 에서 state, props 로 txsToRender 관리

## reload
getTransaction 을 사용해서 호출?
txhash list 를 가지고 있고 그걸 계속 refresh 하는듯
### transaction status
```js
let txParams
    try {
      txParams = await this.query.getTransactionByHash(txHash)
      if (!txParams) return
      if (txParams.blockNumber) {
        this.emit('tx:confirmed', txId)
      }
    } catch (err) {
      txMeta.warning = {
        error: err.message,
        message: 'There was a problem loading this transaction.',
      }
      this.emit('tx:warning', txMeta, err)
    }
```


## Metamask inject
```
window.addEventListener('load', function() {

  // Checking if Web3 has been injected by the browser (Mist/MetaMask)
  if (typeof web3 !== 'undefined') {
    // Use Mist/MetaMask's provider
    web3js = new Web3(web3.currentProvider);
  } else {
    console.log('No web3? You should consider trying MetaMask!')
    // fallback - use your fallback strategy (local node / hosted node + in-dapp id mgmt / fail)
    web3js = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
  }

  // Now you can start your app & access web3 freely:
  startApp()

})
```

## MetaMask sendrawtransaciton
metamask 는 tx 에 chainId 를 넣는다
ui 를 200m 마다 update한다.
```js
// metamask-controller
this.sendUpdate = debounce(this.privateSendUpdate.bind(this), 200)
```
# MetaMask-Extension
privateSendUpdate
## AccountTracker - account-tracker
updateForBlock 은 blockTracker 에 'block' 이벤트가 들어오면 호출 됨
updateForBlock 호출 해서
```js
this.emit('block', this.store.getState())
```
에서 'block' event 전달
## BalanceController
## TransactionController - transactions.js
Transaction Controller is an aggregate of sub-controllers and trackers composing them in a way to be exposed to the metamask controller
- txStateManager
  responsible for the state of a transaction and
  storing the transaction
- pendingTxTracker
  watching blocks for transactions to be include
  and emitting confirmed events
- txGasUtil
  gas calculations and safety buffering
- nonceTracker
  calculating nonces
blockTracker 에 'block' 이벤트가 오면 pendingTxTracker 에서 tx를 체크한다.
newUnapprovedTransaction
## TransactionStateManger - tx-state-manager.js
## PendingTransactionTracker - pending-tx-tracker.js
### queryPendingTxs
blockTracker 에서 sync 이벤트 발생할 때, 호출 (transactions.js 에서 발생)
### checkPendingTxs
// checks the network for signed txs and
// if confirmed sets the tx status as 'confirmed'
```js
txParams = await this.query.getTransactionByHash(txHash)
```
transaction 정보를 가져오고 blockNumber 가 있으면 tx:confirmed 를 날린다
## event-proxy.js
blockTracker
## blockTracker ?
NetowrkController 에서 생성 - event0
## NonceTracker - nonce-tracker.js
getCurrentBlock
getNetworkNextNonce


## tx-helper
unapprovedTxs 에 대한 log 를 남긴다.

## ui
### account-detail
### actions
setCurrentCurrency : update currency


## 참고
[metamask/faq](https://github.com/MetaMask/faq/blob/master/DEVELOPERS.md)
[eth-query](https://github.com/ethereumjs/eth-query)
  - minimal rpc wrapper
[web3-stream-provider](https://github.com/kumavis/web3-stream-provider)
[provider-engine](https://github.com/MetaMask/provider-engine)
[ObservableStore](https://github.com/kumavis/obs-store)
[web3-stream-provider](https://github.com/kumavis/web3-stream-provider)
[eth-token-tracker](https://github.com/MetaMask/eth-token-tracker)
[csw-readyevent](https://github.com/frankiebee/csw-readyevent)
[depp-extend](https://github.com/unclechu/node-deep-extend)
  : deep copy and concat
