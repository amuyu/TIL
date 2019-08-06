# Klaytn
* network
* accounts
* transactions
* smart contract

## SDK
Caver-java
Caver-js


## Network
* CCN (Core Cell Network)
Transaction 실행 및 검증
블록을 생성하고 전파하는 역할을 한다.
* ENN (Endpoint Node Network)
트랜잭션을 생성하고 RPC API 요청을 다루는 역할을 수행한다.
* SCN (Service Chain Network)
BApp 에서 독립적으로 작동하는 보조 블록체인으로 구성된 Klaytn subnetwork
ENN 에 연결되서 Klaytn network 에 데이터를 요청한다.

좀더 자세히
* CCN
CCN 은 CC(Core Cell)로 구성되며, 1개의 CN(Consensus node)과 2개의 PN(Proxy node) 으로 구성된다.
* CNN (Consensus node network)
CN 은 CNN 이라는 full-mesh network 를 형성한다.
BFT 를 적용 및 수행한다.
* PNN (Proxy node network)
PN 은 서로 연결되어 PNN 을 형성한다.
* ENN (End point node network)
서로 연결된 EN과 다수의 PN 으로 구성된다. EN 들은 rpc api 요청 및 데이터를 처리하는 Klaytn 네트워크 끝점 역할을 한다.
* Boot Node
새로 가입한 노드가 네트워크에 등록하고 연결할 다른 노드를 검색하는 노드


## Account
사람의 잔액과 smart contract 정보가 포함된 데이터 구조
20 byte address 를 사용한다.

### 주소에서 키 쌍 분리
Klaytn 은 사용자가 address 와 key pair 를 선택할 수 있다.
Key pair 는 하나거나 그 이상이어도 된다. Key pair 마다 다른 역할을 가질 수 있다.
[Accounts - Klaytn Docs](https://docs.klaytn.com/klaytn/design/account#multiple-key-pairs-and-role-based-keys)

### HRA
사람이 읽을 수 있는 address
기능 개발 중,

### Klaytn Wallet key format
0x{private key}0x{type}0x{address in hex}
Type == 00
#### Klaytn Account Types
EOAS, SCAS
* EOAS (Externally Owned Accounts)
Type, nonce, balance, humanReadable, key
* SCAS (Smart Contract Accounts)
Type, nonce, balance, humanReadable, key, codeHash, storageRoot, codeFormat

### Account Key
* AccountKeyNil
Empty key
* AccountKeyLegacy
Key pair 에서 파생된 주소가 있는 경우에 사용한다.
Account 가 AccountKeyLegacy 인 경우, transaction 검증은 다음과 같다.
Txhash 와 txsig 에서 PublicKey 를 가져오고 public key 에서 address 를 가져온다 address 는 sender 이다.
* AccountKeyPublic
하나의 공개키가 있는 계정에 사용한다. 검증은 다음과 같다.
Txhash 와 txsig 에서 PublicKey 를 가져오고 일치하는지 확인한다.
* AccountKeyFail
Transaction 검증이 항상 실패하는 account
* AccountKeyWeightedMultiSig
Publickey 와 weight 으로 구성된 목록을 포함한 weightedpublickey 과 임계값이 포함된 account, 트랜잭션이 성공하려면 서명 된 공개 키의 가중 합계가 임계 값보다 커야합니다.
* AccountKeyRoleBased
Role-based key 를 나타낸다. Accountkey 목록을 가지고 있음
* Roles
— RoleTransaction : TxTypeAccountUpdate 이외의 transaction 서명
— RoleAccountUpdate : 이 키가 없으면 RoleTransaction key 를 사용
— RoleFeePayer : tx fee 를 대신 지불하는 사람 선택


## Transaction
Value, to, input, v,r,s, nonce, gas, gasPrice

### Signature Validation of Transactions
From 의 AccountKey 를 가져와서 종류에 따라 서명을 검증한다.

### Fee Delegation
비즈니스 모델의 유연성을 위해 여러가지 수수료 위임 버전을 제공

### SenderTxHash
SenderTxHash 는 fee payer 의 address 와 서명이 없는 transaction 
Sendertxhash 를 얻는 방법은 transaction 유형에 따라 다름

### TxTypeLegacyTransaction
AccountKeyLegacy 로 동작한다.
Account 생성, token 전송, smart contract deploy, 실행 

Sender 서명을 위한 RPL Encoding
Rlp encoding => keccak256 => sign

```
SigRLP = encode([encode([type, nonce, gasPrice, gas, to, value, from]), chainid, 0, 0])
SigHash = keccak256(SigRLP)
Signature = sign(SigHash, <the sender’s private key>)
```


Fee Payer 의 서명
```
SigFeePayerRLP = encode([ encode([type, nonce, gasPrice, gas, to, value, from]), feePayer, chainid, 0, 0 ])
SigFeePayerHash = keccak256(SigFeePayerRLP)
SignatureFeePayer = sign(SigFeePayerHash, <the fee payer's private key>)
```


RLP Encoding for SenderTxHash
```
txSignatures (a single signature) = [[v, r, s]]
txSignatures (two signatures) = [[v1, r1, s1], [v2, r2, s2]]
SenderTxHashRLP = type + encode([nonce, gasPrice, gas, to, value, from, txSignatures])
SenderTxHash = keccak256(SenderTxHashRLP)
```

RLP Encoding for Transaction Hash
```
txSignatures (a single signature) = [[v, r, s]]
txSignatures (two signatures) = [[v1, r1, s1], [v2, r2, s2]]
feePayerSignatures (a single signature) = [[v, r, s]]
feePayerSignatures (two signatures) = [[v1, r1, s1], [v2, r2, s2]]
TxHashRLP = type + encode([nonce, gasPrice, gas, to, value, from, txSignatures, feePayer, feePayerSignatures])`
TxHash = keccak256(TxHashRLP)
```


## Smart contract
Solidity 지원 remix, truffle 상호 운용성 유지
앞으로 Klaytn 은 다양한 프로그래밍 언어로 작성된 contract 를 수용할 예정

[GitHub - ConsenSys/smart-contract-best-practices: A guide to smart contract security best practices](https://github.com/ConsenSys/smart-contract-best-practices)

ERC20 및 ERC721 을 지원

# 궁금한점
ERC721 은 대체할 수 없는 토큰이라고 하는데, 어떤 개념?
Cryptokitties 는 유전 정보가 다른 고양이를 나타내는 대체할 수 없는 토큰을 구현한다.
모든 키티는 독특하고 서로 교환할 수 없으므로 키티 토큰마다 다른 값을 갖는다.
[ERC-721 Non-Fungible Token Standard | Ethereum Improvement Proposals](https://eips.ethereum.org/EIPS/eip-721)
