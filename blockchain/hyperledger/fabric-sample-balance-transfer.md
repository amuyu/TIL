# balance-transfer 예제
sample Node.js app fabric-client & fabric-ca-client Node.js SDK APIs
이 예제가 fabric-ca 를 사용하는 예제

패브릭 블록체인의 컴포넌트들의 물리적 및 논리적 구성을 확인
작성된 체인코드를 피어에 설치 및 초기화 후 호출하는 방법
블록 및 트랜잭션의 세세한 부분 확인

> 노트북에서 로컬로 테스트할 때, 계속 실패함 : ssl handsake 가 안됨
> 해당 문제는 crypto material 때문에 발생
> 샘플에 있는 artifacts/channel/crypto-config 를 다시 만들어서 네트워크를 올리면 해결됨
> crypto-config 와 genesis.block, channel.tx 등을 만드는 과정은 기본으로 알고 있어야 할 듯

## Network 기본 구성
- 2 CAs
- A SOLO orderer
- 4 peers (2 peers per Org)

# How to run
1. Launch the network using docker-compose
```sh
docker-compose -f artifacts/docker-compose.lyaml up
```

2. Install the fabric-client and fabric-ca-client node modules
```sh
npm install
```

3. Start the node app on PORT 4000
```sh
PORT=4000 node app
```

# Check
testAPIs 를 실행해서 정상 동작하는지 확인할 수 있음
```bash
./testAPIs.sh
```


# blockchain-explorer 와 연동
다행히? blockchain-explorer 에 balance-transfer 네트워크 정보가 미리 셋팅되어 있다.
인증서 정보도 동일할 듯? 동일하다 그냥 사용하면 될 듯

first-network 연동과 동일하게 balance-transfer.json 파일에서 crypto 관련 설정들만 경로에 맞게 설정을
변경하면 연동이 된다.

https://github.com/hyperledger/blockchain-explorer/blob/master/CONFIG-BALANCE-TRANSFER-HLEXPLORER.md
를 참조하자

## operation service 와 연동?
prometeus 와 grafana 연동
balance-trasnfer 샘플에
app/platform/fabric/artifacts/fabric-config-samples/balance-transfer/docker-compose.yaml 을 수정한다.
```yaml
# orderer.example.com#environment
- ORDERER_OPERATIONS_LISTENADDRESS=orderer.example.com:9443
- ORDERER_METRICS_PROVIDER=prometheus

# orderer.example.com#ports
- 9443:9443

# peer.org.example.com
- CORE_OPERATIONS_LISTENADDRESS=peer.org.example.com:9443
- CORE_METRIS_PROVIDER=prometheus

# peer.org.example.com#ports
# 외부 port 는 설정하기 나름
- 9100:9443
```
docker 에서 실행하기 때문에 network 도 하나로 묶어 준다.
```yaml
networks:
    - byfn
```

네트워크를 up 한 후, prometeus 설치 후, 설정 변경 실행 그리고 grafana 연동하면 된다.


-----------------


# Sample REST APIs Requests
프로젝트에서 blockain 과 연동하는 부분은 app/helper.js 에 구현되어 있다.

1. 멤버와 조직 등록 (keyvalue 저장)
peer 를 조작할 member 계정을 만든다. ca 서버에 등록함
org1 에 jim 이라는 계정을 추가한다.
```bash
curl -s -X POST http://localhost:4000/users -H "content-type: application/x-www-form-urlencoded" -d 'username=Jim&orgName=Org1'
```
org 를 가져오고 (config 정보에서)
Jim usercontext 를 찾는다. store 에 있는지,, 있으면 가져온다. 없으면 fa 로부터 가져와야 한다.
fa admin 계정 정보를 사용해서 마찬가지로 UserContext 를 찾는다.

- helper.getClinetForOrg
config setting 으로 부터 `network-connection-profile-path` 정보를 읽어온다.
해당 path 는 config.js 에서 각각 셋팅한다.
hfc.loadFromConfig('') 를 호출해서 client 를 가져온다.
```js
let client = hfc.loadFromConfig(hfc.getConfigSetting('network-connection-profile-path'));
```

loadFromConfig 를 보면 정보를 읽은 후, config 가 있으면 다음의 정보들을 셋팅한다.
```js
if (this._network_config.hasClient()) {
    this._setAdminFromConfig();
    this._setMspidFromConfig();
    this._addConnectionOptionsFromConfig();
}
```


그 다음 loadFromConfig 를 호출해 org1 정보를 load 한다.
```js
clinet.loadFromConfig(hfc.getConfigSetting('Org1-connection-profile-path'));
```

- client.initCredentialStores
config 정보로부터 state 와 crypto suite 를 셋팅한다.
이 때, client_config.credentialStore 정보를 읽어와서 
해당 path (ex : ./fabric-client-kv-org1) 등을 setting 한다
```js
await client.initCredentialStores();
```

- client.getUserContext
org 의 client 를 사용해서 username 으로 usercontext 를 가져온다.
name 의 userContext 를 return 한다. checkPersistence 가 false 이면 memory 에서만 확인하고
true 이면, store 에 있는지 확인한다.
`loadUserFromStateStore` 를 호출해서 정보를 호출한다. store 에 저장되어 있으면 User 객체를 return 한다.
```js
var user = await client.getUserContext(username, true);
```
user 가 있고나 등록되어 있으면 return 하고, 아니면 user 정보를 setting 한다.

- hfc.getConfigSetting('admins')
config.json 에 설정했던 fabric-ca admin 계정 정보를 읽어온다.

- client.setUserContext({username, password}) - CA 계정
client 에 admin userContext 를 셋팅한다.
admin 계정으로 userContext 를 가져온다. store 에 저장되어있는지를 보고 없으면 ca server 에 요청해서 받아온다. 

내부 코드를 보면 _setUserFromConfig 를 호출하는데 해당 이름으로 usercontext 를 가져오는 듯하다.
어디서?

- client._setUserFromConfig() - CA 계정
network 의 client 정보를 가져온다. (org1.yaml)
다시 network 설정정보의 organizations 정보를 가져온다. 이때, 인증서 pk 등이 필요함 (network-conifg.yaml) 
_adminCertPEM, _adminPrivateKeyPEM 등이 필요함

- client.getCertificateAuthority - CA 계정
CertificateAuthority 구현체를 가져온다. 
connection profile 과 client configuration 정보를 기반으로 가져온다.

- CertificateAuthority 
ca 설정정보들을 가지고 있고 그 정보를 토대로 fabricCAServices 와 통신하는 메소드를 제공하는 object 이다.

- client_buildCAfromConfig - CA 계정
url, tlsCACerts, caName, cryptoSuite 로 fabric-cs-service 객체를 생성한다.
그리고 CertificateAuthority.setFabricCAServices 를 호출해서 set 한다.

- CertificateAuthority.enroll - CA 계정
사용자의 인증서를 리턴해준다. username 과 mspid cryptoContent 를 가지고
user 정보를 등록한다. (메모리, store)

- client.getCertificateAuthority() - CertificateAuthority
를 호출하면 caClient 를 가져오게 되고
fabricCAServices.register 를 호출해 새로운 user 를 등록할 수 있다.

- CertificateAuthority.register - 사용자 계정
fabric-ca 등록에 성공하면 계정 password 를 준다.

- clinet.setUserContext() - 사용자 계정
usercontext 등록한다.
이 때는 파일이 안생김 흠

- ca_service.enroll - 사용자 계정
enrollment 받는다. (ca, root ca)
이 정보들을 다시 privateKeyPEM, signedCertPEM, importedKey 등으로 뽑아낸다.

- clinet.setUserContext - 사용자계정
여기서 skipPersistence 가 false 일 때, `saveUserToStateStore` 를 호출해서
파일로 떨군다.


--------

2. install chaincode
peers, chaincodeeName, chaincodePath, chaincodeVersion, chaincodeType, username, org_name

- helper.setupChaincodeDeploy
GOPATH 를 설정한다. config.json 에 CC_SRC_PATH 로 설정하도록 되어있고
`artifacts` 폴더를 보게 되어있다.

- helper.getClinetForOrg(org_name, username)
config 정보로부터 (connection-profile-path) 정보를 읽어와 셋팅한후, 
username 과 일치하는 usercontext 를 호춣
이때, Usercontext 를 호출하기 위한 파일이 kv 폴더에 있어야 함

- clinet.installChaincode(request)
입력한 peer 들에 요청,,, peer 들에 요청 권한만 있으면 가능하다?




-----------

3. instantiate

client.instantiate

------------


4. sendTransactionProposal

channel.sendTransactionProposal



- client.newDefaultKeyValueStore 
client.newDefaultKeyValueStore 를 호출한다.
org 에 대한 store 정보를 셋팅한다.
keyValueStore 를 return 한다.

파라미터로 path 를 받고 path 에 사용할 키 정보를 저장한다.
파라미터로 입력하는 path 정보는 `client.getConfigSetting('keyValueStore')_{orgName}` 을 넣는다.
keyValueStore 로 셋팅된 config 정보를 읽어오고 config 정보는 memory, cmd arg, env, files 등으로 설정한 정보들로부터 읽어온다.
여기서는 app_config.json 에서 셋팅하고 있는 데 `/tmp/fabric-client-kvs` 로 경로가 되어있다.

Client 에서 addConfigFile 을 호출해서 config 정보들을 추가할 수 있고,
다음과 같이 설정 정보를 추가하고 있다. `nconf` 모듈을 사용해서 env 를 셋팅한다.
```js
Client.addCOnfigFile(path.join(__dirname, '../app_config.json'));
```

- client.setStateStore(store)
생성한 store 정보를 client 에 setting 한다.
이 메소드는 optional 이고, certificate 와 privateKey 와 같은 heavy-weight objects 들을 persising 하기 위해 제공한다.
여기서는 file-based implementation 을 사용한다.

- client.getUserContext(name, checkPersistence)
name 의 userContext 를 return 한다. checkPersistence 가 false 이면 memory 에서만 확인하고
true 이면, store 에 있는지 확인한다.

`loadUserFromStateStore` 를 호출해서 정보를 호출한다. store 에 저장되어 있으면 User 객체를 return 한다.


yaml 셋팅은 뭐로 하는거지?








# ref
[github](https://github.com/hyperledger/fabric-samples/tree/release-1.4/balance-transfer)