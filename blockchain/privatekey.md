# 계정 생성
geth account 명령으로 계정을 생성해서 address와 privatekey 를 받는게 아니라
local 에서 address와 privatekey 를 생성함

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

## 인증


## MyEtherWallet
### 생성
Keystore.tsx
DownloadWallet.tsx 에서
setWallet 으로 생성
### unlock
id="selectedUploadKey" 사용
keystore.tsx - KeystoreDecrypt 가 맞는듯
WalletDecrypt.tsx - 'keystore-file' (144)
common/segas/wallet.ts - (209) - unlock
helpers.ts - (70)

actionType
```js
export interface KeystoreUnlockParams {
  file: string;
  password: string;
}
```
action
```js
export interface UnlockKeystoreAction {
  type: TypeKeys.WALLET_UNLOCK_KEYSTORE;
  payload: KeystoreUnlockParams;
}
```


`keythereum`
There are three main steps to get from private -> address:
Create a random private key (64 (hex) characters / 256 bits / 32 bytes)
Derive the public key from this private key (128 (hex) characters / 512 bits / 64 bytes)
Derive the address from this public key. (40 (hex) characters / 160 bits / 20 bytes)



https://ethereum.stackexchange.com/questions/11166/how-to-generate-a-keystore-utc-file-from-the-raw-private-key

> var Wallet = require('ethereumjs-wallet');
> var key = Buffer.from('efca4cdd31923b50f4214af5d2ae10e7ac45a5019e9431cc195482d707485378', 'hex');
> var wallet = Wallet.fromPrivateKey(key);
> wallet.toV3String('password');
'{"version":3,"id":"467233bf-45ec-423b-9548-bdc4a42aa099","address":"b14ab53e38da1c172f877dbc6d65e4a1b0474c3c","crypto":{"ciphertext":"17886b7ff355219dd20900543b9592fcd4dc6fe7d8f776f1a4d1c63993112181","cipherparams":{"iv":"434e4e71d2013a2d84e86a6e89efbb0b"},"cipher":"aes-128-ctr","kdf":"scrypt","kdfparams":{"dklen":32,"salt":"7a785ab75fa906734788d85ff43a2c8e704af41881dd50a2d52abe08092f07ec","n":262144,"r":8,"p":1},"mac":"98d9a76960dcef22a5fd28a6bf47e5c68a71b30bcf353eccbf5a6555abec78a1"}}'


```json
{"address":"acc2468eba6486b0b723bab6c4e93575b0737fd7","crypto":{"cipher":"aes-128-ctr","ciphertext":"8eb1c4651b67bbf748324f4239ae772247d258b0e73f5ebffaf86508726a2857","cipherparams":{"iv":"776b7eeb89790452e834289a369f51d7"},"kdf":"scrypt","kdfparams":{"dklen":32,"n":262144,"p":1,"r":8,"salt":"b9d622a4333d134a7919c07aa685d86f7dbc3f093e1297bcdefb52359acc814a"},"mac":"38d98579a95d6b4b17498f132ffde171eb8890cb023419af268d540c0711ec40"},
"id":"6f6880b0-e5e2-4813-8553-49831d110a85","version":3}
```

# wallet Construct
generate(icap)
// MyEtherWallet, N_FACTOR = 1024
const keystore = wallet.toV3(password, { n: N_FACTOR });
keystore.address = toChecksumAddress(keystore.address);
toChecksumAddress

https://theethereum.wiki/w/index.php/Accounts,_Addresses,_Public_And_Private_Keys,_And_Tokens
https://github.com/ethereumjs/ethereumjs-wallet

# icap
Inter exchange Client Address Protocol
https://github.com/ethereum/wiki/wiki/ICAP:-Inter-exchange-Client-Address-Protocol

ethereumjs-tx?

# 참고
[전자서명](http://terms.naver.com/entry.nhn?docId=3432015&cid=58437&categoryId=58437)
