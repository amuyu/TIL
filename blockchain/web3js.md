# web3.version.getNetwork((err, netId) => {
  switch (netId) {
    case "1":
      console.log('This is mainnet')
      break
    case "2":
      console.log('This is the deprecated Morden test network.')
      break
    case "3":
      console.log('This is the ropsten test network.')
      break
    default:
      console.log('This is an unknown network.')
  }
})

# api
https://myetherwallet.github.io/knowledge-base/diving-deeper/does-myetherwallet-have-an-api.html


# testnet 접속
var web3 = new Web3(new Web3.providers.HttpProvider('https://ropsten.infura.io/xxx'));
// xxx 는 토큰
https://ropsten.infura.io/2SedeZIXQJR45V7MCQMu

# myetherapi 사용
myetherapi 는 parity node 에 접속
https://www.myetherapi.com

# providers
EtherscanProvider
여러 Provider 를 사용해서 api 를 호출 할 수 있도록 구현한 라이브러리
[ethers.js](https://docs.ethers.io/ethers.js/html/api-providers.html)

# account
web3.eth.register("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
web3.personal.unlockAccount('0xAcc2468ebA6486b0B723bAB6c4e93575b0737fd7','password');
web3.personal.importRawKey("<Private Key>","<New Password>")  // geth console

# balance

# getTransactionCount
web3.eth.getTransactionCount(addressHexString [, defaultBlock] [, callback])
var number = web3.eth.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1


# sendTransaction
web3.eth.sendTransaction

web3.eth.sendTransaction(transactionObject [, callback])
Sends a transaction to the network.

Parameters

Object - The transaction object to send:
from: String - The address for the sending account. Uses the web3.eth.defaultAccount property, if not specified.
to: String - (optional) The destination address of the message, left undefined for a contract-creation transaction.
value: Number|String|BigNumber - (optional) The value transferred for the transaction in Wei, also the endowment if it's a contract-creation transaction.
gas: Number|String|BigNumber - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
gasPrice: Number|String|BigNumber - (optional, default: To-Be-Determined) The price of gas for this transaction in wei, defaults to the mean network gas price.
data: String - (optional) Either a byte string containing the associated data of the message, or in the case of a contract-creation transaction, the initialisation code.
nonce: Number - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
Function - (optional) If you pass a callback the HTTP request is made asynchronous. See this note for details.

# web3.eth.sendRawTransaction
돈을 송금할 때는 value
```js
var Tx = require('ethereumjs-tx');
var privateKey = new Buffer('e331b6d69882b4cb4ea581d88e0b604039a3de5967688d3dcffdd2270c0fd109', 'hex')

var rawTx = {
  nonce: '0x00',
  gasPrice: '0x09184e72a000',
  gasLimit: '0x2710',
  to: '0x0000000000000000000000000000000000000000',
  value: '0x00',
  data: '0x7f7465737432000000000000000000000000000000000000000000000000000000600057'
}

var tx = new Tx(rawTx);
tx.sign(privateKey);

var serializedTx = tx.serialize();

//console.log(serializedTx.toString('hex'));
//f889808609184e72a00082271094000000000000000000000000000000000000000080a47f74657374320000000000000000000000000000000000000000000000000000006000571ca08a8bbf888cfa37bbf0bb965423625641fc956967b81d12e23709cead01446075a01ce999b56a8a88504be365442ea61239198e23d1fce7d00fcfc5cd3b44b7215f

web3.eth.sendRawTransaction('0x' + serializedTx.toString('hex'), function(err, hash) {
  if (!err)
    console.log(hash); // "0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385"
});
```
### stackoverflow 답변 https://ethereum.stackexchange.com/a/27209
https://github.com/trufflesuite/ganache-cli/issues/344
contract 호출
```js
const Web3 = require('web3')  
let web3 = new Web3()  
web3.providers.HttpProvider('https://ropsten.infura.io/my_access_token_here'))  
let contract = web3.eth.contract(abi).at(address)  
var coder = require('web3/lib/solidity/coder')  
var CryptoJS = require('crypto-js')  
var privateKey = new Buffer(myPrivateKey, 'hex')  

var functionName = 'addRecord'  
var types = ['uint','bytes32','bytes20','bytes5','bytes']  
var args = [1, 'fjdnjsnkjnsd', '03:00:21 12-12-12', 'true', '']  
var fullName = functionName + '(' + types.join() + ')'  
var signature = CryptoJS.SHA3(fullName,{outputLength:256}).toString(CryptoJS.enc.Hex).slice(0, 8)  
var dataHex = signature + coder.encodeParams(types, args)  
var data = '0x'+dataHex  

var nonce = web3.toHex(web3.eth.getTransactionCount(account))  
var gasPrice = web3.toHex(web3.eth.gasPrice)  
var gasLimitHex = web3.toHex(300000) (user defined)  
var rawTx = { 'nonce': nonce, 'gasPrice': gasPrice, 'gasLimit': gasLimitHex, 'from': account, 'to': address, 'data': data}  
var tx = new Tx(rawTx)  
tx.sign(privateKey)  
var serializedTx = '0x'+tx.serialize().toString('hex')  
web3.eth.sendRawTransaction(serializedTx, function(err, txHash){ console.log(err, txHash) })
```
iconex
```js
// https://ethereum.stackexchange.com/questions/12054/can-not-send-eth-on-ropsten-using-infura-node
export function eth_sendCoinApi(privKey, data) {
  return new Promise(resolve => {
    const sendAmount = window.web3.toWei(new BigNumber(data.value), "ether");
    getTxFee(data, true).then((gas) => {
      const rawTx = {
        nonce: window.web3.toHex(window.web3.eth.getTransactionCount(check0xPrefix(data.from))),
        from: check0xPrefix(data.from),
        to: check0xPrefix(data.to),
        gasPrice: window.web3.toHex(gas[0]),
        gasLimit: window.web3.toHex(gas[1]),
        // EIP 155 chainId - mainnet: 1, ropsten: 3
        chainId: 3,
        value: window.web3.toHex(sendAmount),
      }
      const privateKey = new Buffer(privKey, 'hex');
      const transaction = new tx(rawTx);
      transaction.sign(privateKey);
      const serializedTx = transaction.serialize().toString('hex');
      window.web3.eth.sendRawTransaction(
          check0xPrefix(serializedTx), function(err, result) {
              if(err) {
                  console.log('error');
                  console.log(err);
                  resolve([false, err]);
              } else {
                  console.log('success');
                  console.log(result)
                  resolve([true, result]);
              }
          }
      );
    });
  });
}
```


# getTransactionFromBlock
getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])
var transaction = web3.eth.getTransactionFromBlock('0x4534534534', 2);
console.log(transaction); // see web3.eth.getTransaction

# 참고
[Using Infura with web3j](https://web3j.readthedocs.io/en/latest/infura.html)
[signup infura](https://infura.io/signup)
[github/javascriptapi](https://github.com/ethereum/wiki/wiki/JavaScript-API)
