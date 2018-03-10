
# ICO(Initial Coin Offering)
신규 가상 화폐공개

## 이더리움 ICO
이더리움 ICO는 공개적으로 토큰(코인)을 판매한다.
투자금은 비트코인, 각각의 구매자에게 구매한 양만큼 이더를 지급하는 제네시스 블록을 생성

이더리움 기반 토큰을 `Mist 브라우저`가 인식하게 하려면
name, symbol, decimals 지정하고 transfer() 와 Transfer 이벤트를 만들어야 한다.

# CrowdFund(Contract) - goodjoon
Create(Deploy)
Contract 는 Transaction Receipt 를 통해 Contract Address 를 가져올 수 있다.
캠페인 시작 : Beneficiery, Goal 설정
  Transacion 확인 방법, web3.eth.getTransactionReceipt(), etherscan.io
목표달성여부점검 : cmapaign ID, IsReached, Goal, Current Amount
  캠페인 ID 로 확인
투자하기 : Contribute
목표달성 시 펀드 전달 > SmartContract 스스로 함수를 실행할 수 없다.


Create Contract > Deploy >

## 배포
### python
26 rpc_url = "http://127.0.0.1:7545"
27 w3 = Web3(HTTPProvider(rpc_url))
28 # 혹은 IPC를 통해 연결할 수 있다.
29 # w3 = Web3(IPCProvider("./chain-data/geth.ipc"))
30 # 지갑 주소를 unlock 해준다. 순서대로 지갑 주소, 비밀번호, unlock할 시간 (0은 영원히)
31 # w3.personal.unlockAccount(w3.eth.accounts[0], "test", 0)
32
33
34 compiled_sol = compile_source(contract_source_code)
35 contract_interface = compiled_sol["<stdin>:Greeter"]
36
37 # 배포 준비, 스마트 컨트랙트 껍데기(abi)에 내용물(bin)을 채운다.
38 contract = w3.eth.contract(abi= contract_interface['abi'],
39                            bytecode= contract_interface['bin'],
40                            bytecode_runtime= contract_interface['bin-runtime'])
41
42
43 # 비용을 부담할 주소를 from으로 하는 트랜잭션을 포함해 배포한다.
44 tx_hash = contract.deploy(transaction={'from': w3.eth.accounts[0]})
 45

## block 정보 가져오기
```js
 web3.eth.getBlock($scope.blockId,function(error, result) {
                    if(!error) {
                        deferred.resolve(result);
                    } else {
                        deferred.reject(error);
                    }
                });
```
### block 에 transaction count 가져와 transaction 가져오기
```js
web3.eth.getBlockTransactionCount($scope.blockId, function(error, result){
          var txCount = result

          for (var blockIdx = 0; blockIdx < txCount; blockIdx++) {
            web3.eth.getTransactionFromBlock($scope.blockId, blockIdx, function(error, result) {

              var transaction = {
                id: result.hash,
                hash: result.hash,
                from: result.from,
                to: result.to,
                gas: result.gas,
                input: result.input,
                value: result.value
              }
              $scope.$apply(
                $scope.transactions.push(transaction)
              )
            })
          }
        })
```


## web3.eth.contract
var MyContract = web3.eth.contract(abiArray);

// instantiate by address
var contractInstance = MyContract.at(address);

// deploy new contract
var contractInstance = MyContract.new([constructorParam1] [, constructorParam2], {data: '0x12345...', from: myAccount, gas: 1000000});

// Get the data to deploy the contract manually
var contractData = MyContract.new.getData([constructorParam1] [, constructorParam2], {data: '0x12345...'});
// contractData = '0x12345643213456000000000023434234'

## contract method
Contract Methods
// Automatically determines the use of call or sendTransaction based on the method type
myContractInstance.myMethod(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly calling this method
myContractInstance.myMethod.call(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly sending a transaction to this method
myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

// Get the call data, so you can call the contract through some other means
var myCallData = myContractInstance.myMethod.getData(param1 [, param2, ...]);
// myCallData = '0x45ff3ff6000000000004545345345345..'

## get contract address
web3.eth.getTransactionReceipt('{txn_id}') 여기서 읽기는 하는데..
block 뒤지기
```
var filter = eth.filter({fromBlock:0, toBlock:'latest', address: "0x.."});
filter.get(function (err, transactions) {
  transactions.forEach(function (tx) {
    var txInfo = eth.getTransactionReceipt(tx.transactionHash);
    /* Here you have
    txInfo.contractAddress;
    txInfo.from;
    txInfo.input;
    */
  });
});
```
[How to find contract's address?](https://ethereum.stackexchange.com/questions/1871/how-to-find-contracts-address)


[qtumexplorer](https://qtumexplorer.io/contracts)
[github/qtum-explorer](https://github.com/qtumproject/qtum-explorer)

## 참고
[Ethereum 응용 개발 - Smart Contract 의 응용 예시 (2/2)](http://goodjoon.tistory.com/263)
[mist 브라우저](https://github.com/ethereum/mist)
[blockcypher api](https://www.blockcypher.com/dev/ethereum/#introduction)
[crowdsale 구현](https://ethereum.org/crowdsale)
