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
