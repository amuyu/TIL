

# CodeChain Foundry
Foundry는 블록체인 네트워크를 쉽게 구축할 수 있게 도와주는 프레임워크

블록체인 네트워크 구현에 필요한 기능들을 모듈형태로 제공하고 
개발자는 여러 모둘들을 조합해서 쉽게 application 을 만들 수 있도록 하는 것을 목표로 함


## 특성
- 확장가능한 합의 알고리즘
기본적으로 core 는 텐더민트를 사용 (전체적으로 모든 알고리즘을 지원하고 유지보수 및 개선하는데는 많은 노력이 필요하므로)
- 모듈 시스템
- IBC (ICS : Interchain Standard) 지원
Foundry 를 사용한 블록체인 네트워크들이 여러개 나왔을 때, asset 교환들을 위한 지원
- 모바일 등에서 동작할 수 있는 client 지원 - other subsystem


## 텐더민트 개선 사항
core 는 텐더민트를 사용하고 있기 때문에 텐더민트가 가지고 있는 문제점들을 개선하기 위해 다음과 같은 구현을 진행함

1. 텐더민트에서 리더를 선출하는 로직 - Randomized (Tenderand)
기본적으로는 리던 선출에는 순서가 정해져 있다. 이 경우, 선택적으로 리더만 골라서 DOS 공격을 하는 공격에 취약하다. 이걸 랜덤하게 진행하는 방법
2. Signature aggregation with BLS
블록 합의를 위해서는 2/3 이상은 사인을 해야 한다..... 100개면? 67개의 서명이 필요
서명의 데이터가 늘어나므로 데이터를 찾이하게 된다. 요걸 줄이는 방법에 대한 이야기


## 모듈 시스템

### 요구사항
다음의 요구사항들을 만족하도록 구현

- 여러 모듈들을 조합해서 쉽게 application 을 만들것
- 특정 모듈들을 쉽게 업데이트 할 수 있도록 하는 것
모듈 업데이트를 위해 블록체인을 껏다 켜는 것이 아닌 특정 블록 height 이 후부터 해당 모듈로 업데이트 가능
- 다양한 프로그래밍 언어를 지원, 각 모듈들도 여러 언어로 작성되도 상관없을 것
웹어셈블리로 컴파일되는 모든 언어(C++, Rust, Java, JavaScript, Go 등)
- 다른 블록체인과 통신 (IBC-ICS)
- 보안 문제가 있는 모듈로 인한 문제를 최소화하기 위한 노력 (isolation한 환경 구축)
sandboxing, object capability model


## 서비스
모듈 시스템을 이용하여 여러가지 서비스들을 선택적으로 구현할 수 있음
- query
- transaction handler
- well-known services (rpc endpoints)
- host apis




# Tenderand : Randomized Leader Election in Tendermint
ECVRF(타원곡선을 이용한 VRF)를 구현하고 텐더민트의 “Propose” 단계를 수정하여 리더로서의 자격을 갖춘 노드들이 블록을 제안하도록 구현

## 구현 목표 
기존의 텐더민트는 누가 리더가 될지 어느 정도 예측이 가능하기 때문에 DOS 공격에 취약하다. 
이를 막기 위해서는 예측이 불가능해야 한 구현이 필요했다. 
타원곡선을 이용한 VRF 를 사용해서 리더가 선출되도록 구현했고 테스트넷에서 테스트를 완료함

각 노드들은 VRF 를 통해 random 한 hash 들을 생성하고 생성된 hash 들중에 
가장 높은 우선 순위를 갖는 hash 를 제출한 노드가 리더가 됨

## VRF
VRF 를 통해 랜덤한 값을 생성할 수 있으며 public key로 누구나 우선 순위를 확인할 수 있음
secret key, seed => VRF => random hash, proof
public key, random hash, proof => verify => true, false




# Verisync: CodeChain's Snapshot Sync
모든 블록의 헤더와 블록을 다운로드 받아 검증하는 방식 대신 신뢰할 수 있는 블록 헤더에 대한
상태를 스냅샷으로 다운로드 받아서 동기화 할 수 있도록 구현했다.

## 구현 목표
처음부터 블록을 동기화하거나 데이터를 받는 것이 아니라 필요한 데이터만 받아서 사용하도록 하는 것이 목표

## 구현 방법
스테이트 머클 트리를 청크단위로 나누는 테크닉을 사용
기존 머클 루트 구조를 쪼개서 청크 단위로 나누고 쪼개진 청크들을 재구성할 수 있도록 
terminal branch 를 추가함 (한 chunk 는 4 depth 를 갖도록 설정함)

전체 블록을 다운받을 필요 없이 최종 머클 루트에 이어지는 최상단 청크만 있으면 검증이 가능한 형태가 됨

신뢰할 수 있는 블록 헤더 정보가 있으면 스테이트 동기화 이후부터 최신 블록까지 동작하게 되고 풀노드처럼 동작할 수 있음

## 이슈
몇 가지 issue 들이 있는데, verisync 가 가능하도록 수정함
- 노드들에게 보상을 주기 위해서 validator set 을 한시간정도 유지를 하는데(열심히 일했는지 확인하기 위해서) 보상을 계산할 때, grandparents state 를 확인해야 하는 이슈
보상을 주는 시점 변경, 블록 데이터에 현재 validator set 정보 추가 등
- timelock utxo issue 등