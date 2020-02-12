# 소개
medium, youtube

# CodeChain Foundry
SDK 형태로 제공
작년말 부터 이제 시작한 프로젝트
현재 개발 중,, github 에 공유,,

## 개요
mainnet 을 2년동안 개발하면서 겪었던 경험들을 ...
secure token, asset token 등 일반적인 blockchain 기술들 개발 능력들이 있으니.. 활용

블록체인 application 을 쉽게 만들 수 있는 framework
다음의 특성을 갖는 frame work
- 확장가능한 합의 알고리즘 (텐더민트 외에도 pow, pos 등 지원)
- 모듈 시스템
- IBC (ICS : Interchain Standard)
- 모바일 등에서 동작할 수 있는 client - other subsystem

기본적으로 core 는 하나! 
어쨋든 다른 core 들을 구현하고 유지보수 및 개선하고자 하는데는 많은 노력들이 필요하므로...

### 왜 텐더민료?
Safety, Liveness 등 때문

#### 개선 사항
1. 텐더민트에서 리더를 선출하는 로직 - Randomized (Tenderand)
기본적으로는 리던 선출에는 순서가 정해져 있다. 이 경우, 선택적으로 리더만 골라서 DOS 공격을 하는 공격에 취약하다
이걸 랜덤하게 진행하는 방법
2. Signature aggregation with BLS
블록 합의를 위해서는 2/3 이상은 사인을 해야 한다..... 100개면? 67개의 서명이 필요
서명의 데이터가 늘어나므로 데이터를 찾이하게 된다. 요걸 줄이는 방법에 대한 이야기

### 모듈 시스템

#### 요구사항들
여러 모듈들을 조합해서 쉽게 application 을 만들것
특정 모듈들을 쉽게 업데이트 할 수 있도록 하는 것
다양한 프로그래밍 언어를 지원, 각 모듈들도 여러 언어로 작성되도 상관없을 것
보안 문제가 있는 모듈로 인한 문제를 최소화 하기 위한 .. sandboxing, object capability model

#### 구조

##### Module 
- Metadata : interface, export, import
- code : sandbox provider 에서 요구하는 format 으로...
sandbox ? npm? jvm?

##### Coordinator
- module 들을 관리
- 특정 block height 이후 부터 업데이트 된 버전 사용?

##### descripor

#### services
여러 가지 서비스들을 선택적으로 구현할 수 있음
- query
- transaction handler
- well-known services, rpc endpoints
- host apis


### ICS
블록체인이 여러개 나왔을 때, asset 교환들을 어떻게 할 것인가
ICS 세미나나 techtalk 에서 자세한 내용은 보면 될 것 같습니당

relayer? 블록체인 사이에서 패킷을 전송해주는 부분

- IBC
다음 테크톡에서 자세하게 이야기할 예정
ICS 의 모듈은 IBC 에서 모듈역할을 한다...
모듈 간의 연결?


### Cryptography
원래는 비트코인, 이더리움과 동일한 secp256k 를 사용했었으나 제약이 있어
제약사항으로는 위에서 말한 여러 서명들을 하나로 합치고 verify 한다던지 할 때?
두가지의 curves 를 사용하는 방향으로 간다.
속도가 중요!
- Ed25519 : transaction signing
- BLS12-381 : consensus vote signing


===============

# Tenderand : Randomized Leader Election in Tendermint
구현이 완료됨, testnet 에서 테스트 완료
랜덤을 VRF 를 사용해서 리더가 선출되고 확인할 수 있도록 구현

## review
VRF (Verifiable Random Function)
### Sortition Algorithm
algoland?

## Implement

### SVDW Method

### ECVRF

### Apply
unique valid proposal
RLE tendermint : Vanila tendermint
여러 상황들에서 적용할 수 있는 적용방법들

## result
propose timeout was set to 4000ms
블록 평균 생성 시간 4.42s
블록이 생성이 안될 수 있다... proposal 이 없을 때?? 막을 수 가없다? 줄이는 방향으로 갖고 있음\
한 라운드당 티켓 7번, 한 노드당 1.2번 정도의 기회

여러명이 나왔을 때,
해쉬 등에 티켓 값이 나오는데 다시 랜덤 돌려서? 이런 값들을 가지고 가장 높을 값을 갖는다

### frequency of being a leader
6개의 node 를 기준으로 확인했을 때,,,  확률적인 특성이 괜찮다
delegated ccs


## seed value 를 어떤 것으로 사용했는지 확인?
seed 값들이 header 에 포함되어 사용


알고랜드에서는 randomize 한 경우 리더들이 여러명이 나올 수 도 있고 여러번 투표를 해서 줄여나간다
텐더민트에서는 2번의 투표로 이게 가능한가? fork 발생안하나? finalize?


===============

# Snapshot sync proposal

## Goal
처음부터 블록을 동기화하거나 데이터를 받는 것이 아니라 필요한 것들만 받아서 사용하도록 하는 것이 목표
일부분만 받아도 검증이 가능하도록 하는것 (Snapshot 을 잘게 쪼개기)

## process
- header 정보
- bootstrap nodes

## state 구조
merkle patricia tree 를 사용하는데 어떻게 쪼갤까?

## chunk
chunk 단위로 받고 chunk 를 가지고 verify 할 수 있는 방법
branch 의 terminal 을 만들어서 이 부분을 통해 chunk 단위로 검증이 가능함

한 chunk 는 4 depth 를 갖고 있음, codechain 에서 사용하는 depth
> 한 chunk 는 terminal root 를 다 합치면 chunk root hash 와 같다.
chunk 단위도 merkle 형태를 유지하도록 구현되어 있음

이더리움 fast sync 와 비슷한거 같은데...
이더리움의 경우에는 chunk 단위로 검증이 되지 않는다고 한다,.


## 다운받는 process
chunk root 와 terminal 들의 root 를 확인하는 방법으로 verify 하면서 다운로드 한다...

## 구현해야할 것들
- 특정 블록을 받았을 때, 이전의 블록정보만을 가지고 verify 가 가능해야함
- validator set 을 한시간정도 유지를 하는데 보상을 계산할 때, grandparents state 를 확인해야 하는 이슈
- timelock utxo issue 등이 있었음

## precommit 에 해당하는 정보를 어디에 저장할 것인가
자세한건 세미나에.....

## Validator 에게 보상을 어떻게?
validator 에게 보상을 줄 때, 다음 term 이 끝나는 시점에 제공
term 이 끝날 때 주면 되지 않나? block 에 precommit 정보가 있기 때문에
해당 block 이 있어야 제대로 된 보상을 계산할 수 있다.

## precommit 을 확인하기 위해서 이전전 block 이 필요하다
validator set은 이전 블록에 있기 때문에,,,  (다음 validator set 만을 들고 있었음)
해결 방법은 현재 validator set 까지 저장한다

## timelock problem
코드체인도 utxo 구조를 가지고 있음


=============






















