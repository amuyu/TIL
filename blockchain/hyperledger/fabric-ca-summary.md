# fabric-ca
Hyperledger Fabric의 인증 기관 

## 기능
- registration of identities, or connects to LDAP as the user registry
- issuance of Enrollment Certificates (ECerts)
- certificate renewal and revocation

----

## docker run
```yaml
fabric-ca-server:
  image: hyperledger/fabric-ca
  container_name: fabric-ca-server-test
  ports:
    - "7054:7054"
  environment:
    - FABRIC_CA_HOME=/etc/hyperledger/fabric-ca-server
  volumes:
    - "./fabric-ca-server:/etc/hyperledger/fabric-ca-server"
  command: sh -c 'fabric-ca-server start -b admin:adminpw
```

----

# csr (Certficate Signing Request)
X.509 서명키 및 인증서와 관련된 정보 설정
```yaml
csr:
   cn: fabric-ca-server # 일반적인 이름
   keyrequest:
     algo: ecdsa
     size: 256
   names:
      - C: US                   # 나라
        ST: "North Carolina"    # 주 
        L:                      # 위치 또는 도시
        O: Hyperledger          # 조직 이름
        OU: Fabric              # 조직 단위
   hosts:
     - 5a7cfcd48854
     - localhost
   ca:
      expiry: 131400h
      pathlength: 1
```

----

## check
https://www.sslshopper.com/certificate-decoder.html
- Common Name: fabric-ca-server
- Organization: Hyperledger
- Organization Unit: Fabric
- State: North Carolina
- Country: US
- Valid From: March 1, 2020
- Valid To: February 26, 2035
- Issuer: fabric-ca-server, Hyperledger
- Serial Number: 0f6e4aa6ff389fad3ee0739c2985fd2b9d733023

----

# db
 The default database is SQLite and the default database file is `fabric-ca-server.db` in the Fabric CA server’s home directory.


# DB 테이블 구조
- affiliations
- certficates
- credentilas
- nonces
- properties
- revocation_authority_info
- users

----

## affiliations
fabric-ca-server-config.yaml
```yaml
affiliations:
   org1:
      - department1
      - department2
   org2:
      - department1
``` 

----


# default admin
속성
```yaml
# Contains identity information which is used when LDAP is disabled
identities:
    - name: admin
    pass: adminpw
    type: client
    affiliation: ""
    attrs:
        hf.Registrar.Roles: "*"
        hf.Registrar.DelegateRoles: "*"
        hf.Revoker: true
        hf.IntermediateCA: true
        hf.GenCRL: true
        hf.Registrar.Attributes: "*"
        hf.AffiliationMgr: true
```

----

# enrolling the bootstrap identity
ecert, 개인키, ca 인증서 및 체인 pem 파일을 저장
default 로 `fabric-ca-client-config.yaml` 파일을 생성함
`fabric-ca-client` 는 이후, 저장된 msp 와 설정 파일의 내용을 토대로 명령을 실행함
```sh
export FABRIC_CA_CLIENT_HOME=/etc/hyperledger/fabric-ca-server/clients/admin
fabric-ca-client enroll -u http://admin:adminpw@localhost:7054
fabric-ca-client enroll -u http://'admin:rhkstpcjd!@'@localhost:7054
```

----

# register
admin 계정으로 register
id.affiliation 은 등록된 정보로만 셋팅 가능
```sh
export FABRIC_CA_CLIENT_HOME=/etc/hyperledger/fabric-ca-server/clients/admin
fabric-ca-client register --id.name peer1 --id.affiliation org1.department1 --id.attrs 'hf.Revoker=true,admin=true:ecert'
```

----

# getcacert
인증서 저장
```sh
fabric-ca-client getcacert -u http://10.0.1.100:7054 -M $FABRIC_CA_CLIENT_HOME/msp

2020/03/06 03:24:53 [INFO] Configuration file location: /etc/hyperledger/fabric-ca-server/clients/admin/fabric-ca-client-config.yaml
2020/03/06 03:24:53 [INFO] Stored root CA certificate at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/msp/cacerts/localhost-7054.pem
2020/03/06 03:24:53 [INFO] Stored Issuer public key at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/msp/IssuerPublicKey
2020/03/06 03:24:53 [INFO] Stored Issuer revocation public key at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/msp/IssuerRevocationPublicKey
r

```
----

# 사용자 역할과 Endorsement
client : transaction 제출, query
peer : transaction 승인, commit
MSP.ROLE = member, admin, client, peer (아마도 member 는 admin, client, peer 를 모두 아우르는..)\
hf.type = user, orderer, client, peer
role : admin, peer, cleint?
// 등록 기관이 관리할 수 있는 역할 목록
hf.Registrar.Roles = client, user, peer, validator, auditor
// 등록자가 'hf.Registrar.Roles' 속성에 대해 등록 트리에 부여할 수 있는 역할 목록?
hf.Registrar.DelegateRoles = client, user, validator, auditor
```java
/**
  * HFCA_TYPE_PEER indicates that an identity is acting as a peer
  */
public static final String HFCA_TYPE_PEER = "peer";
/**
  * HFCA_TYPE_ORDERER indicates that an identity is acting as an orderer
  */
public static final String HFCA_TYPE_ORDERER = "orderer";
/**
  * HFCA_TYPE_CLIENT indicates that an identity is acting as a client
  */
public static final String HFCA_TYPE_CLIENT = "client";
/**
  * HFCA_TYPE_USER indicates that an identity is acting as a user
  */
public static final String HFCA_TYPE_USER = "user";
```

----

# Org 조직 Admin MSP 속성
```yaml
id:
  name: Admin@org0
  type: client
  affiliation: org0
  maxenrollments: 0
  attributes:
   # - name:
   #   value:
    - name: hf.Registrar.Roles
      value: client,peer,user,orderer
    - name: hf.Registrar.DelegateRoles
      value: client,peer,user,orderer
    - name: hf.Registrar.Attributes
      value: "*"
    - name: hf.GenCRL
      value: true
    - name: hf.Revoker
      value: true
    - name: hf.AffiliationMgr
      value: true
    - name: hf.IntermediateCA
      value: true
    - name: role
      value: admin
      ecert: true
```
----

# Orderer Admin MSP 속성
```yaml
id:
  name: Admin@ordererorg0
  type: client
  affiliation: ordererorg0
  maxenrollments: 0
  attributes:
   # - name:
   #   value:
    - name: hf.Registrar.Roles
      value: client,peer,user,orderer
    - name: hf.Registrar.DelegateRoles
      value: client,peer,user,orderer
    - name: hf.Registrar.Attributes
      value: "*"
    - name: hf.GenCRL
      value: true
    - name: hf.Revoker
      value: true
    - name: hf.AffiliationMgr
      value: true
    - name: hf.IntermediateCA
      value: true
    - name: role
      value: admin
      ecert: true
```

----

# Peer MSP 속성
```yaml
id:
  name: peer0
  type: peer
  affiliation: org0
  maxenrollments: 0
  attributes:
    - name: role
      value: peer
      ecert: true
```

----

# Orderer MSP 속성
```yaml
id:
  name: orderer0
  type: orderer
  affiliation: ordereroorg0
  maxenrollments: 0
  attributes:
    - name: role
      value: orderer
      ecert: true
```

----

# enrolling a peer identity
enroll 호출 시, 인증서 새로 생성
```sh
export FABRIC_CA_CLIENT_HOME=/etc/hyperledger/fabric-ca-server/clients/peer1
fabric-ca-client enroll -u http://peer1:gwHbkrjkdNFc@localhost:7054 -M $FABRIC_CA_CLIENT_HOME/msp

2020/03/06 04:39:40 [INFO] Created a default configuration file at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/fabric-ca-client-config.yaml
2020/03/06 04:39:40 [INFO] generating key: &{A:ecdsa S:256}
2020/03/06 04:39:40 [INFO] encoded CSR
2020/03/06 04:39:40 [INFO] Stored client certificate at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/signcerts/cert.pem
2020/03/06 04:39:40 [INFO] Stored root CA certificate at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/cacerts/localhost-7054.pem
2020/03/06 04:39:40 [INFO] Stored Issuer public key at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/IssuerPublicKey
2020/03/06 04:39:40 [INFO] Stored Issuer revocation public key at /etc/hyperledger/fabric-ca-server/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/IssuerRevocationPublicKey
```

----

# revoke 
```sh
export FABRIC_CA_CLIENT_HOME=/etc/hyperledger/fabric-ca-server/clients/admin
fabric-ca-client revoke -e peer1
```

----

# identity
```sh
fabric-ca-client identity list
fabric-ca-client identity modify user1 --type peer
fabric-ca-client identity modify user1 --secret newsecret
fabric-ca-client identity modify Admin@mall --secret 'rhkstpcjd!@'
fabric-ca-client identity remove user1  # starting the fabric-ca-server with the –cfg.identities.allowremove option.
```

----

# affiliation
```sh
fabric-ca-client affiliation list
fabric-ca-client affiliation add org1.dept1
fabric-ca-client affiliation modify org2 --name org3
fabric-ca-client affiliation remove org2
```

----

# certificates
```sh
fabric-ca-client certificate list
```

----

# Attribute-Based Access Control
```sh
fabric-ca-client register --id.name user1 --id.secret user1pw --id.type client --id.affiliation org1 --id.attrs 'hf.Affiliation=org1:ecert'
```

---

[fabric ca user's guide](https://hyperledger-fabric-ca.readthedocs.io/en/release-1.4/users-guide.html)