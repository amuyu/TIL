sdk 에서 사용하는 object 나 Method 에 대한 정보를 적는다.

# config setting 정보 저장
json 이나 yaml 로 저장된 파일 정보를 읽어온다
```js
hfc.setConfigSetting(name, path);
hfc.addConfigFile(path);
```
이렇게 셋팅한 정보는 
```js
var value = hfc.getConfigSetting(name);
```

# client.loadFromConfig(path)
설정 파일(network-connection-profile)로 부터 설정을 읽어온 후, 이를 들고있는 client 객체를 return 한다. (static)
client 객체에 path 의 설정을 추가한다.


# Client.createUser
username, mspid, cryptoContent 를 받아서 User 객체를 return 
cryptoContent 를 CryptoSuite 라이브러리를 사용해서 key object 를 추가한다. user 정보가 셋팅되면 `setUserContext` 로 context 업데이트

# Client.getUserContext(name, checkPersistence)
name 과 일치하는 context 를 가져온다.
checkPersistence 가 true 이면 메모리 외에 파일 등에도 있는지 확인한다.

# Client.getCertificateAuthority(name)
name 은 Option 으로 없으면 등록된 정보 중에 첫번째 정보를 읽어와서 setting 한다.
network config 에서 org 정보를 가져오고 거기서 다시 certificateAuthorities 정보를 가져온다. 가져온 ca 의 정보를 가지고 FabricCAServices 객체를 build 해서 return 한다.

# Client.setUserContext(user)
입력한 user 정보를 토대로 User 객체를 생성한다.
이 과정에서 enroll 여부를 확인하고 enroll 까지 진행한다.
enroll 하게 되면 enrollment 를 리턴받게 되고, 이 정보를 토대로 cryptoContent 를 만들어서 User 객체를 생성한다.
이 때, org의 credentialStore 의 path 에 user의 정보가 저장된다.


# Client.installChaincode(request, timeout)
request == ChaincodeInstallRequest
chaincode 를 install 한다.



--------------

# CA.register(req, registar)
req 는 enrollmentID, enrollmentSecret(option), role(option), affiliation 정보를 담은 object 이다.
registar 는 user 객체이다.
CA server 에 해당 정보로 등록을 요청한다.


--------------

# CryptoContent
privateKey 와 signedCert 를 들고있는 object
PEM, obj, key 중 하나만 있으면 된다.
예제들의 경우, 파일 형태로 저장된 PEM 키를 사용한다.

- privateKey - the PEM file path for the private key
- privateKeyPEM - the PEM string for the private key 
- privateKeyObj - private key object 
- signedCert - the PEM file path for the certificate
- signedCertPEM - the PEM string for the certificate 

# UserNamePasswordObject
- username : user 의 이름
- password : user 의 password
- caName : ca 이름

# enrollment
- certificate : enrollmentCert
- rootCertificate : caCertChain

# ChaincodeInstallRequest
- Peer[] : target peer (optional?), channelname 을 쓰면 channel 에 정의된 target peer 들에게 install 하나요 (org로 묶어서 사용하는듯)
- chaincodePath : 
- metadataPath : Optional
- chaincodeId : chaincode 의 이름
- chaincodeVersion : chaincode version
- chaincodeType : Optional, 'golang', 'node', 'java'
- channelNames : Optional, targets 없을 때

# ChaincodeQueryRequest
 
