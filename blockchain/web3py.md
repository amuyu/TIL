
# web3 설치
```
pip install web3
# TestRPCProvider 클래스 dependency
pip install eth-testrpc
```



# web3 설치 중 error 발생
....
scrypt-1.2.0/libcperciva/crypto/crypto_aes.c:6:10: fatal error: 'openssl/aes.h' file not found
#include <openssl/aes.h>
^
1 error generated.
error: command '/usr/bin/clang' failed with exit status 1

```
export LDFLAGS="-L/usr/local/opt/openssl/lib"
export CPPFLAGS="-I/usr/local/opt/openssl/include"
```
or
```
env LDFLAGS="-L$(brew --prefix openssl)/lib" CFLAGS="-I$(brew --prefix openssl)/include" pip install scrypt
```

# solidity
brew update
brew upgrade
brew tap ethereum/ethereum
brew install solidity
brew linkapps solidity
pip install py-solc


# API
web3.eth.getTransaction
Eth.coinbase
web3.fromwei
web3.towei
Eth.sign(account, data)
## sendTransaction
web3.eth.sendTransaction({'to': '0xd3cda913deb6f67967b99d67acdfa1712c293601', 'from': web3.eth.coinbase, 'value': 12345})

personal.account 와 eth.account 차이는?
eth.account 는 지갑? personal.account 계정?

## 참고
[web3py document](https://web3py.readthedocs.io/en/stable/quickstart.html)
[파이썬으로 스마트 컨트랙트 개발하기](https://winterj.me/smart-contract-with-python/)
