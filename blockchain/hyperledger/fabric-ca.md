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

## command
### register
다음은 admin2 를 등록하는 명령이다.
```bash
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca/clients/admin
fabric-ca-client register --id.name admin2 --id.affiliation org1.department1 --id.attrs 'hf.Revoker=true,admin=true:ecert'
```
다음과 같이 config 파일 을 사용해서 default 정보들을 셋팅할 수도 있다.
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

### enroll
```bash
export FABRIC_CA_CLIENT_HOME=$HOME/fabric-ca/clients/peer1
fabric-ca-client enroll -u http://peer1:peer1pw@localhost:7054 -M $FABRIC_CA_CLIENT_HOME/msp
```



# SDK 사용
CA 서버로부터 인증서를 받아옴 기본적으로 인증서 발급 등의 권한을 갖고 있는 계정이 필요함
balance-transfer 샘플의 경우 `fabric-client-kv-org1` 등의 이름으로 인증서를 파일로 떨굼

# Q
CA 와 MSP 는 다르다?
fabcar 는 뭐지?
server 에는 pk 를 저장하지 않는다고 하는데?
Identity Mixer credential?


# ref
[하이퍼레저 패브릭 vs CORDA](https://hamait.tistory.com/983)
[Fabric CA User's Guide](https://hyperledger-fabric-ca.readthedocs.io/en/latest/users-guide.html)
[옛날버전 fabric](https://rezamtfabric.readthedocs.io/en/stable/home.html)