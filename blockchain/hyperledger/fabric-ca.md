# fabric-ca
CA == Certificate Authority (CA)

## 하는 일
각 조직, 서비스 등에 대한 신원 등록, LDAP 으로 관리?
Enrollment 인증서 발급 (ECerts)
인증서 갱신 및 폐지

## Overview
Fabric CA server 는 REST APIs 이다.
전체 architecutre 를 보면, peer 에서 ca-server 로 직접 붙지는 않고
fabric-SDK 에서 fabric ca server 와 peer 붙어 통신하는 형태로 구성되어 있다.

# fabric-ca-server

## start
docker 로 시작하려면 다음과 같이 셋팅한다.
docker-compose.yaml
```yaml
fabric-ca-server:
  image: hyperledger/fabric-ca:x86_64-1.0.0-beta
  container_name: fabric-ca-server
  ports:
    - "7054:7054"
  environment:
    - FABRIC_CA_HOME=/etc/hyperledger/fabric-ca-server
  volumes:
    - "./fabric-ca-server:/etc/hyperledger/fabric-ca-server"
  command: sh -c 'fabric-ca-server start -b admin:adminpw'
```

## initializing the server
server 설정 파일에 ca.certfile 과 ca.keyfile 이 있는데 이 파일들이 root ca 가 되는 듯 하다.

> balance-transfer 예제를 보면 ca 는 docker 띄우는데, 
> 초기에 org 의 ca 와 tls 인증서를 각각 설정하도록 되어 있다.
> FABRIC_CA_SERVER_CA_CERTFILE, FABRIC_CA_SERVER_CA_KEYFILE, FABRIC_CA_SERVER_TLS_CERTFILE, FABRIC_CA_SERVER_TLS_KEYFILE


# CA 계층도
RootCA 에서부터 가지쳐서 내려오는 구조
- Enrollment CA 는 새로운 user 를 네트워크에 등록하는데 사용하는 CA
- Transaction CA 는 등록된 사용자가 트랜잭션을 요청하는 것에 사용되는 CA
- TLS CA 는 사용자들이 원격피어와 통신을 할 때 보안 채널로써 사용

# 체인코드에서의 MSP 사용
어떤 조직에 속 할 경우 에만 어떤 체인코드의 메소드를 호출 할 수 있는 권한을 부여한다.
```go
// 조직의 전용 mspID 와 certCN 을 통해서 확인하여, 호출 할 수 있는 조직을 경우에만 호출하게 제한한다. 
func authenticateExportingEntityOrg(mspID string, certCN string) bool {
return (mspID == "ExportingEntityOrgMSP") && (certCN == "ca.exportingentityorg.trade.com")
}
```

# fabric-ca-client
## home directory
* -home 을 사용한 경우 해당 경로로 지정
* FABRIC_CA_CLIENT_HOME
* FABRIC_CA_HOME
* CA_CFG_PATH
* otherwise, $HOME/.fabric-cal-client

## Enrolling the bootstrap identity
CSR
```
csr:
  cn: <<enrollment ID>>
  key:
    algo: ecdsa
    size: 256
  names:
    - C: US
      ST: North Carolina
      L:
      O: Hyperledger Fabric
      OU: Fabric CA
  hosts:
   - <<hostname of the fabric-ca-client>>
  ca:
    pathlen:
    pathlenzero:
    expiry:
```

## command
### register
요청하는 ID 는 등록되어 있어야 하며, 등록 권한이 있어야 한다.
Server 에서는 다음 세 가지를 확인한다.
1. registrar 는 hf.Registar.Roles 속성이 있어야 한다.
2. The affiliation of the registrar (소속) 은 등록할 신원의 소속과 같거나 접두사가 같아야한다. 예를 들어 “a.b”의 소속이 있는 등록자는 “a.b.c” 소속으로 ID 를 등록할 수 있지만 “a.c” 소속으로 ID 를 등록할 수 없습니다.
3. Registrar 는 다음 조건을 모두 만족하는 경우, ID 를 등록할 수 있습니다.
* Registrar 는 접두사가 hf 인 속성을 등록할 수 있다. 등록자가 속성을 소유하고 hf.Registar.Attributes 속성 값의 일부인 경우 해당한다. list 유형인 경우 등록중인 속성 값은 등록자가 보유한 값의 일부이거나 같아야한다.
* 사용자 정의 속성 (hf. 로 시작하지 않는 속성) 을 등록하려면 등록 기관에 등록중인 속성 또는 패턴의 값이 있는 hf.Registar.Attributes 속성이 있어야한다. 유일하게 지원되는 패턴은 끝에 * 가 있는 문자열
  예를 들어 "ab *" 는 "ab" 로 시작하는 모든 속성고 일치, 
  hf.Registrar.Attributes=orgAdmin 이 있는 경우 registrar 가 ID 에서 추가하거나 제거할 수 있는 속성은 orgAdmin 속성이다.
* 요청된 속성 이름이 "hf.Registrar.Attributes" 인 경우 등록자 값의 서브세트워 같은지 확인하는 추가 검사가 수행된다.
  예를 들어, hf.Registrar.Attributes 의 등록자 값이 'ab *, xyz' 이고 요청된 속성 값이 'abc,xyz'인 경우 'abc'는 패턴과 일치하므로 유효한다.


다음은 admin2 를 등록하는 명령이다.
ID 가 'admin2', 이고 소속은 'org1.department1' 이고 속성은 'hf.Revoker' 과 'admin' true 라는 값으로 생성한다.
":ecert" 라는 접미사는  'admin' 속성과 값이 ecert 에 등록되어 접근제어 결정에 사용될 수 있다는 의미이다.
```bash
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca/clients/admin
fabric-ca-client register --id.name admin2 --id.affiliation org1.department1 --id.attrs 'hf.Revoker=true,admin=true:ecert'
```
register 하게 되면 enroll 하는데 필요한 Password 가 리턴된다. 혹은 Secret 을 입력할 수도 있다.

다음과 같이 config 파일 을 사용해서 default 정보들을 셋팅할 수도 있다.
`$FABRIC_CA_CLIENT_HOME/fabric-ca-client-config.yaml` 설정 파일 경로
```yaml
id:
  name:
  type: client
  affiliation: org1.department1
  maxenrollments: -1
  attributes:
    - name: hf.Revoker
      value: true
    - name: anotherAttrName
      value: anotherAttrValue
```

affiliations 는 non-leaf affiliations 를 제외하고는 대소문자를 구분한다.
```
affiliations:
  BU1:
    Department1:
      - Team1
  BU2:
    - Department2
    - Department3
```
위에서 BU1, Department1, BU2 는 다 소문자로 저장된다.

### enroll
enroll command 는 ECert 를 저장할 수 있다.
Fabric CA client's 의 msp directory 에 pk 와 PEM 파일들이 저장된다.
'-M' option 을 사용해 MSP 경로를 설정할 수 있다.
peer 의 경우 core.yaml 에 `mspConfigPath` 설정, orederer 인 경우, 
orderer.yaml 의 `LocalMSPDir` 설정으로 받으면 된다.
```bash
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca/clients/peer1
fabric-ca-client enroll -u http://peer1:peer1pw@localhost:7054 -M $FABRIC_CA_CLIENT_HOME/msp
```

Fabric-ca-server 에서 발급한 ecert 는 다음의 organizational units 을 갖는다.
1. OU 의 root 는 identity type 과 동일
2. identity의 affiliation 에 OU 가 추가된다.
예를 들어, peer type 에 affiliation 이 department1.team1 이면 OU 계층은 다음과 같다.
OU=team1, OU=department1, OU=peer

### Getting Identity Information
```sh
fabric-ca-client identity list --id user1
fabric-ca-client identity list
```

### Adding an identity
identity 를 추가할 수도 있다.
```sh
fabric-ca-client identity add user1 --json '{"secret": "user1pw", "type": "client", "affiliation": "org1", "max_enrollments": 1, "attrs": [{"name": "hf.Revoker", "value": "true"}]}'

```

## Attribute-Based Access Control
app1 애플리케이션을 개발 중이고 app1 관리자만 특정 체인코드 조작에 액세스 할 수 있어야 한다면
체인코드는 호출자의 인증서에 app1Admin 이라는 속성이 true 값을 포함하는지 확인할 수 있다.




# SDK 사용
CA 서버로부터 인증서를 받아옴 기본적으로 인증서 발급 등의 권한을 갖고 있는 계정이 필요함
balance-transfer 샘플의 경우 `fabric-client-kv-org1` 등의 이름으로 인증서를 파일로 떨굼

# Q
CA 와 MSP 는 다르다?
fabcar 는 뭐지?
server 에는 pk 를 저장하지 않는다고 하는데?
Identity Mixer credential?
enroll? 이 뭘까요?

# enroll 이 뭘까요?
`FabricCAClient` 의 enroll 을 보면 다음과 같이 설명하고 있다.
> Enroll a registered user in order to receive a signed X509 certificate

Enroll 하면 해당 사용자의 키정보를 가져온다.

# ref
[하이퍼레저 패브릭 vs CORDA](https://hamait.tistory.com/983)
[Fabric CA User's Guide](https://hyperledger-fabric-ca.readthedocs.io/en/latest/users-guide.html)
[옛날버전 fabric](https://rezamtfabric.readthedocs.io/en/stable/home.html)