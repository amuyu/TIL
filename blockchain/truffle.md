# configuration
configuration file 은 `tuffle.js` 로, 프로젝트의 root 에 위치한다.

module.exports 객체를 사용해서 설정해야 한다.
```js
module.exports = {
  networks: {
    development: {
      host: "127.0.0.1",
      port: 8545,
      network_id: "*" // Match any network id
    }
  }
};
```

## option
### network
gas : deploy 를 위한 gas limit, default 는 4712388
gasPrice : deploy 를 위한 gas price, default 는 100000000000
from : migration 하는 동안 사용하는 from address, default 는 ethereum 첫 번째 이용 가능한 account
provider : default web3 provider, `new Web3.providers.HttpProvider("http://<host>:<port>")`
### contracts_build_directory
contracts_build_directory: "./output",

##


# metacoin
truffle 에서 제공하는 webpack 프로젝트를 사용해서 contract 에 대해서 살펴본다.

`truffle-init-webpack` 을 설치한다.
```sh
truffle unbox webpack
```

프로젝트 `/contracts` 에 보면 MetaCoin.sol 있는데, 이 contract 를 통해 `Metacoin` 을 생성한다.
balance 를 조회하고 coin 을 전달하는 기능이 구현되어 있다.
이 Contract 를 테스트 하기 위해서 `ganache` 라는 블록체인을 설치하고 실행한다.

`ganache` 는 http://truffleframework.com/ganache/  다운받아 설치할 수 있다.
설치 후, 실행을 하면 로컬로 블록체인이 돌아가는 상태가 된다.

이제 MetaCoin 을 deploy 해보자
다음의 명령을 실행해 contracts 소스를 compile 하고 migrate 한다.
```sh
truffle compile
truffle migrate
```

`ganache` 을 보면 MetaCoin을 depoly 하는 transactions 을 볼 수 있다.
coin 거래를 위한 준비는 다 되었고 샘플앱을 실행한다.

```sh
npm run dev
```

실행 후, 브라우저에서 `localhost:8080` 으로 접속해보면 생성한 MetaCoin 에 대한 정보를
확인하고 블록체인의 Accounts 에 coin 을 전달할 수 있다.

## 참고
[Getting started with Ganache: your personal blockchain for Ethereum development](https://steemit.com/utopian-io/@icaro/getting-started-with-ganache-your-personal-blockchain-for-ethereum-development)


# tutorialToken
```sh
truffle unbox tutorialtoken
npm install zeppelin-solidity
```
[openzepplin](https://openzeppelin.org)
tutorialtoken
npm install -g truffle
npm install -g ethereumjs-testrpc

```solidity
contract TutorialToken is StandardToken {

    string public name = 'TutorialToken';
    string public symbol = 'TT';
    uint8 public decimals = 2;
    uint public INITIAL_SUPPLY = 12000;
}

```
truffle unbox {name}
truffle compile
truffle migrate
truffle migrate --reset
npm run dev

http://truffleframework.com/tutorials/robust-smart-contracts-with-openzeppelin

## balance 확인
tutorialTokenInstance.balanceOf(account)

## transfer
tutorialTokenInstance.transfer(toAddress, amount, {from: account});

# error
## exceeds block gas limit
`exceeds block gas limit` 에러가 발생했을 때, truffle.js 에 network 정보에 gas 값을 추가해줌
```
module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // for more about customizing your Truffle configuration!
  networks: {
    development: {
      host: "127.0.0.1",
      port: 8545,
      network_id: "1012", // Match any network id,
      gas: 2000000
    }
  }
};
```


# How To Create Token and Initial Coin Offering Contracts Using Truffle + OpenZeppelin


[tutorial](https://blog.zeppelin.solutions/how-to-create-token-and-initial-coin-offering-contracts-using-truffle-openzeppelin-1b7a5dae99b6)
