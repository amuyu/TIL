
# keyring
Klaytn 계정의 주소와 계정에서 사용하는 private key 를 저장하는 구조로 Keyring을 사용한다.


```
// caver-java
SingleKeyring senderKeyring = caver.wallet.keyring.create(senderAddress, senderPrivateKey);
// 
//Read keystore json file.
File file = new File("./keystore.json");

//Decrypt keystore.
ObjectMapper objectMapper = ObjectMapperFactory.getObjectMapper();
KeyStore keyStore = objectMapper.readValue(file, KeyStore.class);
AbstractKeyring keyring = KeyringFactory.decrypt(keyStore, "password");
```

# 분류
- SingleKeyring : 사용자가 개인키 하나로 서명합니다.
- MultipleKeyring : 사용자가 다수의 개인키로 서명합니다.
- RoleBaseKeyring : 사용자가 역할에 따른 개인키로 서명합니다.

## Role
그럼 Role이 뭔데?
- RoleTransaction : trnasaction 서명 (계정 업데이트 트랜잭션 이외에)
- RoleAccountUpdate : 계정 업데이트 트랜잭션 서명
- RoleFeePayer : 트랜잭션 수수료 위임





# Account
넌 뭐니?


# ref
[Keyring 관리](https://ko.docs.klaytn.com/dapp/sdk/caver-js/getting-started#managing-keyrings)