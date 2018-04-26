# account
There are two types of accounts: externally owned accounts (EOAs) and contract accounts.
Normal or externally controlled accounts and
contracts, i.e., snippets of code, think a class.

# address
이더리움은 공용키 암호화를 사용  ECDSA (Elliptic Curve Digital Signature Algorithm)
```js
var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('https://ropsten.infura.io/'));
var util = require('ethereumjs-util');
var tx = require('ethereumjs-tx');

var privateKey = '0xc0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0de';
var publicKey = util.bufferToHex (util.privateToPublic (privateKey));
```
개인키와 관련해서 이더리움 주소는 공개 키 의 SHA3-256 (Keccak) 해시의 마지막 160 비트입니다 .
pirvateKey ----> (ECDSA) ----> publicKey ----> (sha3-256) ----> address

# property
nonce : transactoin count
balance
storageRoot
codeHash : 값이 있으면 contract account

## 참고
[Inside ethereum transaction](https://medium.com/@codetractio/inside-an-ethereum-transaction-fa94ffca912f)
[account management](http://ethdocs.org/en/latest/account-management.html)
[account-unique](https://ethereum.stackexchange.com/questions/4299/account-uniqueness-guaranteed)
