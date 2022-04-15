

# compile
컴파일 하면 artifacts 디렉토리에 저장된다.
artifacts/build-info 에서 contract 의 build 정보를 볼 수 있다.
```
npx hardhat compile
```

# accounts
`hardhat.config.js` 에서 network별로 local 설정 가능, privateKey, HDWallet
```
network: {
    accounts: {
        mnemonic: "", 
    },
    accounts: [privateKey1, privateKey2, ...]
}

hardhat: {
    chainId: 1337,
    accounts: {
    mnemonic: "test test test test test test test test test test test junk",
    initialIndex: 0,
    accountsBalance: "10000000000000000000000",
    },
},
```
test account
20 accounts with 10000 ETH each, generated with the mnemonic 
"test test test test test test test test test test test junk".


# task
https://hardhat.org/guides/create-task.html
`hardhat.config.js` 에 작성
ex
```
task("balance", "Prints an account's balance").setAction(async () => {});
```

# hardhat network
built-in 되어 있는 harhat network 실행
```
npx hardhat node
```

# command
```
# compile : be saved in the artifacts/ dir
npx hardhat compile

# test
npx hardhat test

# deploy
npx hardhat run scripts/sample-script.js
npx hardhat run scripts/sample-script.js --network localhost
npx hardhat run scripts/sample-script.js --network baobab 

# connecting hardhat network, start
npx hardhat node
```

# deploy 에러 관련
https://github.com/mjkim/hardhat-klaytn-patch

# hardware-wallet
https://github.com/nomiclabs/hardhat/issues/1159#issuecomment-849648283

