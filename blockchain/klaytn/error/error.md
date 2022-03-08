
# TxTypeLegacyTransaction 에러
https://forum.klaytn.com/t/truffle-txtypelegacytransaction/1722

공식 문서상으로 truffle version은 v5.0.26 지원, v5.1.61 
변경 후, 잘 됨
https://github.com/klaytn/klaytn-contracts 여기에서 truffle version 확인필요
```shell
$ npm install
$ npm install -g truffle@v5.1.61
$ npm install -g ganache-cli@v6.12.1
```


# evm: execution reverted 
truffle로 baobab 에 contract 배포 후, contract 호출하면 다음과 같은 에러 발생
```json
{"jsonrpc":"2.0","id":0,"error":{"code":-32000,"message":"evm: execution reverted"}}
```
https://ko.docs.klaytn.com/smart-contract/porting-ethereum-contract#solidity-support
를 보면 네트워크는 아래와 같이 지원함
Baobab 네트워크는 현재 London Ethereum Virtual Machine(EVM)과 호환 가능합니다.
Cypress 네트워크는 현재 Constantinople Ethereum Virtual Machine(EVM)과 호환 가능합니다.
그래서 각 네트워크별 evm은 아래와 같은 버전으로 컴파일하는 것이 좋음
Baobab: --evm-version london
Cypress: --evm-version constantinople
solidity 버전이 너무 낮은 듯 klaytn ide 에서도 에러가 발생함
> 에러가 발생한 solididty는 petersburg evm을 지원하도록 되어 있음 evm을 변경해야함!
다음과 같이 truffle-config 에서 변경 가능!
```js
compilers: {
solc: {
    version: "0.5.6",    // 컴파일러 버전을 0.5.6로 지정
    settings: {          // See the solidity docs for advice about optimization and evmVersion
    optimizer: {
        enabled: false,
        runs: 200
    },
    evmVersion: "constantinople"
    }
}
```