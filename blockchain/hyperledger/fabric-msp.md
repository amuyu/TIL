# MSP
Membership Service Provider
peer 와 org 의 digital identities 를 생성

## 종류
Local MSP 는 해당 노드를 제어할 때 사용, 
Network/channel MSP 는 Global MSP 로서 해당 MSP 는 모든 구성원들을 식별할 때 사용
Global MSP 는 genesis block 과 channel.tx 에 포함
원장에 블록체인 네트워크에 참여하는 조직 정보와 채널에 참여하는 조직의 정보 기록
- Network MSP : 블록체인 네트워크 관리 권한
- Channel MSP : 특정 채널에 참여하는 조직 식별
- Local MSP(Peer,Orderer) : 
    + peer msp : peer 의 파일 시스템에 탑재, 속한 조직의 노드 식별, contract 설치 검증
    + orderer msp : orderer 노트에 탑재, 신뢰할 수 있는 노드 식별

> genesis block 에 네트워크 참여 조직 정보를 기록하면 조직의 변동에 대한 내역은 어떻게?
> governance score 처럼 별도로 관리하는 내용이 존재하나?


## MSP 구조
- admincerts
- cacerts
- intermediatecerts (optional)
- config.yaml (optional)
- crls (optional)
- keystore
- signcerts
- tlscacerts (optional)
- tlsintermediatecerts (optional)

## MSP 설정
### basic
- peer 와 orderer 에 MSP 인스턴스 설정이 명시
- MSP ID 는 MSP 인스턴스 당 Unique 해야함
- Signing 을 위해 사용되는 signing key, 노드의 x.509 인증서 필요
- MSP identity 들은 expire 되지 않고 CRLs(certificate revocation list) 에 추가함으로써 revoke
곤
### peer 와 orderer 에 MSP Setup
- core.yaml
mspConfigPath 에 설정
> 어디에 쓰는거지? 일단 패스 peer 기본 설정 파일인듯
- docker-compose.yaml
에서 CORE_PEER_LOCALMSPID 로 설정

- orderer.yaml
LocalMSPDir, LocalMSPID 에 설정
- docker-compose.yaml
에서 ORDERER_GENERAL_LOCALMSPID 로 설정

### Channel MSPs
channel policies 는 channel에서 누가 특정한 액션에 참여할 수 있는지 정의
configtx.yaml 에서 설정
> Rule 설정에서 Org1MSP.admin, Org1MSP.peer, Org1MSP.client 가 나오는데 잘 모르겠음

## Appendix Business Domain 적용시 고려사항
Membership life cycle
- Who owns the priviledge to invite organizations to the network?
- What are the minimum security requirements that the organization needs to meet?
- What are the standard contractual agreements that participants should accept?
- What are the IT service-level agreements that the participant will need to adhere to?

## peer? org? member? 에 대한 이해


# Q
어떻게 서명한 내용에 대해서 확인을 하는가?
genesis.block 과 channel.tx 에는 참여하는 조직의 정보가 기록되어 있다.
peer 들은 조직의 구성원이다. MSP 에는 RootCA 라는게 있고 조직의 구성원인 것을 증명한다.

# ref
패브릭 4주차 특강 자료 참곤