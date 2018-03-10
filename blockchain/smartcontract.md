
# contracts with Mist
Mist 에서 간단한 Contracts 배포해보자.
먼저 다음 Solidity 컨트랙트 코드를 Mist를 사용해 설치한다.
```solidity
pragma solidity ^0.4.0;
contract Hello {
    string helloMsg;

    function Hello(string msgArg) public {
        helloMsg = msgArg;
    }

    function sayHello() public constant returns (string) {
        return helloMsg;
    }
}
```
코드를 입력하고 설치 버튼을 누르면 Contract 가 생성되고
Contract 주소를 알 수 있다.

# json format은 뭘까?
json 과 contract address 를 가지고 token instance 를 얻음.


# contract 정보 호출
Contract 컴파일
deploy 하면 Contract 가 생성된다.
tx receipt 로부터 contract 주소를 가져올 수 있다.
  deploy 하면서 발생한 tx_hash 로 w3.eth.getTransactionReceipt(tx_hash)
    var tx_receipt = web3.eth.getTransactionReceipt(tx_hash);
  txcontract 주소를 얻어온다.
    contract_address = tx_receipt['contractAddress']
var contract = web3.eth.contract(jsonArray);    
var instance = contract.at(contract_address);
var owner = instance.owner.call();

# contract
var MyContract = web3.eth.contract(abiArray);

// instantiate by address
var contractInstance = MyContract.at(address);

// deploy new contract
var contractInstance = MyContract.new([constructorParam1] [, constructorParam2], {data: '0x12345...', from: myAccount, gas: 1000000});

// Get the data to deploy the contract manually
var contractData = MyContract.new.getData([constructorParam1] [, constructorParam2], {data: '0x12345...'});
// contractData = '0x12345643213456000000000023434234'

# token
TokenTracker Summary
Total Supply:	767,756.6953 SPIN
Value per Token:	$0.00
Token Holders:	3 addresses
No.Of.Transfers: 	18
 	Reputation UNKNOWN
Contract Address:	0x14a1aefd29dbe7581971bc90e7714b316409b353
Token Decimals: 	18
Official Links:	Not Available, Update ?
Search/Filter By:

# transactions
rpc method 호출
trace_replayTransaction, debug_traceTransaction

ethereumjs-vm 사용
[how to get contract internal transactions](https://ethereum.stackexchange.com/questions/3417/how-to-get-contract-internal-transactions)

# Contract Call

# truffle 없이 컴파일하기

[Ethereum DApps without truffle: compile; deploy; use it :)](https://medium.com/@doart3/ethereum-dapps-without-truffle-compile-deploy-use-it-e6daeefcf919)




# 강의
[Etherscan contract source code verification](https://www.youtube.com/watch?v=HM45gMq-HP8)
This video demonstrates how to verify and publish your solidity source code on https://etherscan.io

verify 등록 할 때, 입력하는 정보인가 봄
verify and publish 를 하기 전에는 binary code만 존재함
web 에서 필요한 정보를 입력해서 etherscan site 에서 볼 수 있게 함
verify 한 이후에는 사이트에서 볼 수 있음
constructor_arguments_in_abi.j

Demo.sol.txt
[Compile and deploy solidity code](https://youtu.be/nnVX6fQUu4o)


# interactions between contract
contract 에서 contract 호출
[interactions between contract](https://dappsforbeginners.wordpress.com/tutorials/interactions-between-contracts/)

# Call function from deployed contract
```
contract ECVerify {
    function ecverify(bytes32 hash, bytes sig, address signer) returns (bool);
}

contract Foo {
    ECVerify ecv = ECVerify(0x3bbb367afe5075e0461f535d6ed2a640822edb1c);

    function callEcv(bytes32 hash, bytes sig, address signer) {
        bool b = ecv.ecverify(hash, sig, signer);
        // handle b
    }
}
```
[Calling function from deployed contract](https://ethereum.stackexchange.com/questions/11743/how-to-call-a-library-contract)


# Contract storage
> Contract storage is a key of 32 bytes and a value of 32 bytes, so the maximum a single contract can store is around 1.46 GB (32^32).

https://ethereum.stackexchange.com/questions/1038/is-there-a-theoretical-limit-for-amount-of-data-that-a-contract-can-store


# Tutorial
```solidity
contract test {

      uint _multiplier;

      function test(uint multiplier){
           _multiplier = multiplier;
      }

      function multiply(uint a) returns(uint d) {
           return a * _multiplier;
      }
}
```
web3 호출
```js
var senderAddress = "0x12890d2cce102216644c59daE5baed380d84830c";
var password = "password";
var abi = @"[{""constant"":false,""inputs"":[{""name"":""val"",""type"":""int256""}],""name"":""multiply"",""outputs"":[{""name"":""d"",""type"":""int256""}],""type"":""function""},{""inputs"":[{""name"":""multiplier"",""type"":""int256""}],""type"":""constructor""}]";
var byteCode =
    "0x60606040526040516020806052833950608060405251600081905550602b8060276000396000f3606060405260e060020a60003504631df4f1448114601a575b005b600054600435026060908152602090f3";

var multiplier = 7;

var web3 = new Web3.Web3();
var unlockAccountResult =
    await web3.Personal.UnlockAccount.SendRequestAsync(senderAddress, password, 120);
Assert.True(unlockAccountResult);

var transactionHash =
    await web3.Eth.DeployContract.SendRequestAsync(abi, byteCode, senderAddress, multiplier);

var mineResult = await web3.Miner.Start.SendRequestAsync(6);

Assert.True(mineResult);

var receipt = await web3.Eth.Transactions.GetTransactionReceipt.SendRequestAsync(transactionHash);

while (receipt == null)
{
    Thread.Sleep(5000);
    receipt = await web3.Eth.Transactions.GetTransactionReceipt.SendRequestAsync(transactionHash);
}

mineResult = await web3.Miner.Stop.SendRequestAsync();
Assert.True(mineResult);

var contractAddress = receipt.ContractAddress;

var contract = web3.Eth.GetContract(abi, contractAddress);

var multiplyFunction = contract.GetFunction("multiply");

var result = await multiplyFunction.CallAsync<int>(7);

Assert.Equal(49, result);

```
[nethereum](https://nethereum.readthedocs.io/en/latest/contracts/deploying/)

## crowd sale
[linda-crowdsale](https://github.com/LindaHealthcareICO/linda-crowdsale)
### CappedCrowdsale
cap
### Crowdsale

## set 호출
```js
simpleStorage.set(12345, function(e,r){console.log(r);});
simpleStorage.set.sendTransaction(12345, function(e,r){console.log(r);});  
```


## helloworld vote
[dapp-tutorial](https://medium.com/@mvmurthy/full-stack-hello-world-voting-ethereum-dapp-tutorial-part-1-40d2d0d807c2)
contract 호출
비동기 호출인 경우, callback 을 해야 한다.
```js
contractInstance.voteForCandidate(candidateName, {from: web3.eth.accounts[0]}, function() {
    let div_id = candidates[candidateName];
    $("#" + div_id).html(contractInstance.totalVotesFor.call(candidateName).toString());
  });
```

## javascript github wiki
```js
// contract abi
var abi = [{
     name: 'myConstantMethod',
     type: 'function',
     constant: true,
     inputs: [{ name: 'a', type: 'string' }],
     outputs: [{name: 'd', type: 'string' }]
}, {
     name: 'myStateChangingMethod',
     type: 'function',
     constant: false,
     inputs: [{ name: 'a', type: 'string' }, { name: 'b', type: 'int' }],
     outputs: []
}, {
     name: 'myEvent',
     type: 'event',
     inputs: [{name: 'a', type: 'int', indexed: true},{name: 'b', type: 'bool', indexed: false}]
}];

// creation of contract object
var MyContract = web3.eth.contract(abi);

// initiate contract for an address
var myContractInstance = MyContract.at('0xc4abd0339eb8d57087278718986382264244252f');

// call constant function
var result = myContractInstance.myConstantMethod('myParam');
console.log(result) // '0x25434534534'

// send a transaction to a function
myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000});

// short hand style
web3.eth.contract(abi).at(address).myAwesomeMethod(...);

// create filter
var filter = myContractInstance.myEvent({a: 5}, function (error, result) {
  if (!error)
    console.log(result);
    /*
    {
        address: '0x8718986382264244252fc4abd0339eb8d5708727',
        topics: "0x12345678901234567890123456789012", "0x0000000000000000000000000000000000000000000000000000000000000005",
        data: "0x0000000000000000000000000000000000000000000000000000000000000001",
        ...
    }
    */
});
```

## contract Methods
```js
// Automatically determines the use of call or sendTransaction based on the method type
myContractInstance.myMethod(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly calling this method
myContractInstance.myMethod.call(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly sending a transaction to this method
myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

// Get the call data, so you can call the contract through some other means
var myCallData = myContractInstance.myMethod.getData(param1 [, param2, ...]);
// myCallData = '0x45ff3ff6000000000004545345345345..'
```

## transaction object
```js
Object - The transaction object to send:
from: String - The address for the sending account. Uses the web3.eth.defaultAccount property, if not specified.
to: String - (optional) The destination address of the message, left undefined for a contract-creation transaction.
value: Number|String|BigNumber - (optional) The value transferred for the transaction in Wei, also the endowment if its a contract-creation transaction.
gas: Number|String|BigNumber - (optional, default: To-Be-Determined) The amount of gas to use for the transaction (unused gas is refunded).
gasPrice: Number|String|BigNumber - (optional, default: To-Be-Determined) The price of gas for this transaction in wei, defaults to the mean network gas price.
data: String - (optional) Either a byte string containing the associated data of the message, or in the case of a contract-creation transaction, the initialisation code.
nonce: Number - (optional) Integer of a nonce. This allows to overwrite your own pending transactions that use the same nonce.
```

## transaction 가져오기
// https://ethereum.stackexchange.com/questions/3763/iterating-over-all-public-values-of-a-mapping-in-web3
```js
function getAddressesSentToAccount(myaccount, startBlockNumber, endBlockNumber) {
  if (endBlockNumber == null) {
    endBlockNumber = eth.blockNumber;
    console.log("Using endBlockNumber: " + endBlockNumber);
  }
  if (startBlockNumber == null) {
    startBlockNumber = endBlockNumber - 1000;
    console.log("Using startBlockNumber: " + startBlockNumber);
  }
  console.log("Searching for transactions to account \"" + myaccount + "\" within blocks "  + startBlockNumber + " and " + endBlockNumber + "\"");

  for (var i = startBlockNumber; i <= endBlockNumber; i++) {
    if (i % 1000 == 0) {
      console.log("Searching block " + i);
    }
    var block = eth.getBlock(i, true);
    if (block != null && block.transactions != null) {
      block.transactions.forEach( function(e) {
        if (myaccount == e.to) {
          console.log(e.blockNumber + " " + e.from + " " + web3.fromWei(e.value, "ether"));
        }
      })
    }
  }
}
```

## array storage get bytes
int, address, string array 있음요
// https://medium.com/@tmyjoe/dapps-how-to-get-elements-of-array-in-a-contract-c61b16b6c438
```
// Integer array storage.
contract IntArrayStorage {

    uint256[] arr;

    function getArrLength()
    returns (uint256)
    {
        return arr.length;
    }

    function setIntArr(uint256 _index, uint256 _value)
    {
        arr[_index] = _value;
    }


    // Convenient function for out of evm world.
    function getAsBytes(uint256 _from, uint256 _to)
    public
    constant
    returns (bytes)
    {
        require(_from >= 0 && _to >= _from && arr.length >= _to);

        // Size of bytes
        uint256 size = 32 * (_to - _from + 1);
        uint256 counter = 0;
        bytes memory b = new bytes(size);
        for (uint256 x = _from; x < _to + 1; x++) {
            uint256 elem = arr[x];
            for (uint y = 0; y < 32; y++) {
                b[counter] = byte(uint8(elem / (2 ** (8 * (31 - y)))));
                counter++;
            }
        }
        return b;
    }
}
// address array storage.
contract AddressArrayStorage {

    address[] arr;

    function getArrLength()
    returns (uint256)
    {
        return arr.length;
    }

    function setIntArr(uint256 _index, address _value)
    {
        arr[_index] = _value;
    }


    // Convenient function for out of evm world.
    function getAsBytes(uint256 _from, uint256 _to)
    public
    constant
    returns (bytes)
    {
   	require(_from >= 0 && _to >= _from && arr.length >= _to);

        // Size of bytes
        uint256 size = 20 * (_to - _from + 1);
        uint256 counter = 0;
        bytes memory b = new bytes(size);
        for (uint256 x = _from; x < _to + 1; x++) {
            bytes memory elem = toBytes(arr[x]);
            for (uint y = 0; y < 20; y++) {
                b[counter] = elem[y];
                counter++;
            }
        }
        return b;
    }

    // from: https://ethereum.stackexchange.com/questions/884/how-to-convert-an-address-to-bytes-in-solidity
    function toBytes(address a) constant returns (bytes b){
        assembly {
            let m := mload(0x40)
            mstore(add(m, 20), xor(0x140000000000000000000000000000000000000000, a))
            mstore(0x40, add(m, 52))
            b := m
        }
    }
}
```

## struct to bytes
```solidity
pragma solidity ^0.4.0;

contract StructSerialization
{
    function StructSerialization()
    {
    }

    event exactUserStructEvent(uint32 id, string name);

    //Use only fixed size simple (uint,int) types!
    struct ExactUserStruct
    {
        uint32 id;
        string name;
    }

    function showStruct(ExactUserStruct u) private
    {
        exactUserStructEvent(u.id, u.name);
    }


    function exactUserStructToBytes(ExactUserStruct u) private
    returns (bytes data)
    {
        // _size = "sizeof" u.id + "sizeof" u.name
        uint _size = 4 + bytes(u.name).length;
        bytes memory _data = new bytes(_size);

        uint counter=0;
        for (uint i=0;i<4;i++)
        {
            _data[counter]=byte(u.id>>(8*i)&uint32(255));
            counter++;
        }

        for (i=0;i<bytes(u.name).length;i++)
        {
            _data[counter]=bytes(u.name)[i];
            counter++;
        }

        return (_data);
    }


    function exactUserStructFromBytes(bytes data) private
    returns (ExactUserStruct u)
    {
        for (uint i=0;i<4;i++)
        {
            uint32 temp = uint32(data[i]);
            temp<<=8*i;
            u.id^=temp;
        }

        bytes memory str = new bytes(data.length-4);

        for (i=0;i<data.length-4;i++)
        {
            str[i]=data[i+4];
        }

        u.name=string(str);
     }

    function test()
    {
        //Create and  show struct
        ExactUserStruct memory struct_1=ExactUserStruct(1234567,"abcdef");
        showStruct(struct_1);

        //Serializing struct
        bytes memory serialized_struct_1 = exactUserStructToBytes(struct_1);

        //Deserializing struct
        ExactUserStruct memory struct_2 = exactUserStructFromBytes(serialized_struct_1);

        //Show deserealized struct
        showStruct(struct_2);
    }
}
```
https://ethereum.stackexchange.com/questions/11246/convert-struct-to-bytes-in-solidity/13507

## getTxFee
gas fee 가져오기
```
function getTxFee(data, isSendTransaction = false) {
  return new Promise(resolve => {
    window.web3.eth.getGasPrice((e, gasPrice) => {
      const obj = { chainId: 3 };
      console.log(data)
      if (!data['tokenAddress']) {
        const sendAmount = window.web3.toWei(new BigNumber(data.value), "ether");
        obj['from'] = check0xPrefix(data['from']);
        const recipientAddressError = validateRecipientAddressError({ recipientAddress: data['to'] });
        obj['to'] = !recipientAddressError ? check0xPrefix(data['to']) : '';
        obj['value'] = window.web3.toHex(sendAmount) || window.web3.toHex(window.web3.toWei(new BigNumber(0), "ether"));
      } else {
        let token = window.web3.eth.contract(erc20Abi).at(check0xPrefix(data.from));
        const valueDiv = customValueToTokenValue(data.value, data.tokenDefaultDecimal, data.tokenDecimal);
        const sendAmount = window.web3.toWei(new BigNumber(valueDiv), "ether");
        const recipientAddressError = validateRecipientAddressError({ recipientAddress: data['to'] });
        const dataObj = !recipientAddressError ? token.transfer.getData(check0xPrefix(data.to), sendAmount) : token.transfer.getData(check0xPrefix(''), sendAmount)
        obj['from'] = check0xPrefix(data['from']);
        obj['to'] = check0xPrefix(data['tokenAddress']) || '';
        obj['value'] = window.web3.toHex(window.web3.toWei(new BigNumber(0), "ether"));
        obj['data'] = dataObj;
      }

      for (let i=0; i<Object.keys(obj).length; i++) {
        if(!Object.values(obj)[i]) {
          delete obj[Object.keys(obj)[i]]
        }
      }

      console.log(obj)

      const gasLimit = !obj['to'] ? 21000 : window.web3.eth.estimateGas(obj);
      console.log(gasLimit)

      if (isSendTransaction) {
        resolve([gasPrice, gasLimit]);
      } else {
        const txFee = gasPrice * gasLimit;
        const txFeeEther = window.web3.fromWei(txFee, 'ether');
        resolve(txFeeEther);
      }
    })
  });
}

```

# image
https://medium.com/@Dave_Appleton/adding-an-image-to-your-token-on-parity-516fc8b51cab
metadata, url hash

To attach a SiteURL and Logo (28x28px PNG format),
https://etherscan.io/token-search

Token Registry
https://github.com/paritytech/parity/wiki/Token-Registry

# gasLimit
auto 인 경우, tx 를 가지고 estimateGas 를 호출해 limit 을 예측한다.
수동 입력으로 값을 입력한다. lower, upper 로 상한선을 정한다.
export const GAS_LIMIT_LOWER_BOUND = 21000;
export const GAS_LIMIT_UPPER_BOUND = 8000000;

what is gas?
https://myetherwallet.github.io/knowledge-base/gas/what-is-gas-ethereum.html


# nonce
nonce update 관련 내용
https://github.com/MetaMask/metamask-extension/issues/1127
## 같은 nonce 에 data 가 다른 경우
replacement transaction underpriced
https://ethereum.stackexchange.com/questions/27256/error-replacement-transaction-underpriced
## 같은 nonce 인 data 가 같은 경우
known transaction
## 이전 nonce 인 경우
nonce too low
## 건너뛴 nonce 인 경우
기다리기....

# web3 와 gas
http://astrocket.com/blog/레일즈-개발자의-이더리움-개발하기-2/


# 참고
[metacoin 사용](https://steemit.com/utopian-io/@icaro/getting-started-with-ganache-your-personal-blockchain-for-ethereum-development)
[mist](https://github.com/ethereum/mist)
[mist private network connect](https://gist.github.com/evertonfraga/9d65a9f3ea399ac138b3e40641accf23)
[Build Your First Ethereum Smart Contract with Solidity — Tutorial](https://codeburst.io/build-your-first-ethereum-smart-contract-with-solidity-tutorial-94171d6b1c4b)
[Programming Ethereum smart contract transactions in JavaScript](https://tokenmarket.net/blog/creating-ethereum-smart-contract-transactions-in-client-side-javascript/)
  - ethereum-scan io api 호출도 나와있음
[webjs와 nodejs 를 사용하 Dapp 개발](https://www.slideshare.net/jaehyun/5-web3js-nodejs-dapp-81891817)
[예제로 배우는 스마트 컨트랙트 프로그래밍](https://www.slideshare.net/jaehyun/4-81886176)
[truffle-contract](https://github.com/trufflesuite/truffle-contract)
[dapp-tutorial](https://medium.com/@mvmurthy/full-stack-hello-world-voting-ethereum-dapp-tutorial-part-1-40d2d0d807c2)
[dapp for beginner](https://dappsforbeginners.wordpress.com)
[deploy contract from contract](https://ethereum.stackexchange.com/questions/18936/deploying-contract-from-contract)
[contract 실행 후, filter 로 contract address 확인?](https://github.com/ethereum/web3.js/issues/416)
