# hyperledger-study
패스트 캠퍼스 강의용 예제


## kv store Setting
config.js 에 `connection-profile-path` 경로를 적어두고 해당 파일을 읽어들인다.
network 정보와 org 정보들을 셋팅한다.

## hlf-client.setClient
### _setFSKeyValueStore
`_initCredentialStores`
credentialStore 를 셋팅한다.
`client.newDefaultKeyValueStore` 를 호출한다. (호출하면 알아서 kv 폴더에 write 한다.)
org 의 credentialStore 정보를 읽어와서 keyValueStore 객체를 생성한다. (FileKeyValueStore)
이 때, 해당 path 에 경로 생성

### _checkAdminClient
`_setupAdminAccount`
member.createAdmin 을 호출하고 이때 파일로 저장된다.

### member.createAdmin
Org1의 mspId 로 userContext 를 호출하고
`client.getUserContext` 를 호출하고 User 정보가 없으면
`client.createUser` 를 호출해서 user 를 추가한다.
여기서 필요한 정보가
client : Sdk
mspId : org msp id
cryptoContent {
    adminKey : admin user 의 private key
    signedCert : msp 의 signcert
}

