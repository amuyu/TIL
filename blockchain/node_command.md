var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('http://127.0.0.1:8545'));
var web3 = new Web3(new Web3.providers.HttpProvider('http://52.78.119.26:8545'));

var web3 = new Web3('http://127.0.0.1:8545');
//
web3.eth.accounts[0]

web3.personal.newAccount('password');
> 0xacc2468eba6486b0b723bab6c4e93575b0737fd7

web3.eth.accounts.encrypt('0xacc2468eba6486b0b723bab6c4e93575b0737fd7', 'password')

## 계정 생성
node
```js
var Wallet = require('ethereumjs-wallet');
var wallet = Wallet.generate();

// keystore
wallet.toV3('password',{ n: 1024});
// or
wallet.toV3String('password');
```
