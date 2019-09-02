# node
각 노드는 ledger와 smart contract 를 보유할 수 있고 각 인스턴스를 호스팅한다.
P:peer node, S:smart contract, L:Ledger

peer 에 smart contract 없이 ledger 만 호스팅하는 경우는 거의 없다.
대부분의 peer 는 ledger 를 쿼리하거나 업데이트하는 smart contract 가 하나 이상 설치 되어 있다.

이 외에 peer에는 항상 존재하는 system chain code가 있다.

## application 및 peer
SDK 를 통해 쉽게 peer 에 접근할 수 있으며, transaction 을 생성하고 호출할 수 있습니다.
대략적인 흐름은 다음과 같다. A: Application, O: Orderer

1. connect to peer (A->P)
2. invoke chaincode (A->P)
2-1. peer invokes chaincode with proposal (P->S)
2-2. chaincode generates query or update proposal response (S->L)
3. proposal response (P->A)
4. request that transaction is ordered (A->O)
4-1. Transacions send to peers in blocks (O->P)
4-2. peer updates ledger using transaction blocks (P->L)
5. ledger update event

4-2. 과정에서 P는 L에 적용하기전에 트랜잭션을 확인하고 L이 업데이트되면 완료되었다는 이벤트를 생성한다.

## peer and channel
응용 프로그램 A는 채널 C를 사용하여 피어 P1 및 P2와 직접 통신 할 수 있습니다.????
> 이게 무슨 말일까? channel 에 던지면 channel 에서 알아서 한다는 말같은데....
> sendTansactionProposal 을 보면 명령이 다음과 같다.
> let results = await channel.sendTransactionProposal(request);
> 이때 request 에 peer 이름을 입력할 수 있고, 이런 방법으로 P1과 P2 와 통신할 수 있다.


## peers and organizations
organization 별로 peer 들을 구성하고 organization 에 속한 peer 들을 channel 로 공유하면서
블록체인 네트워크가 확장되고 축소된다.

## peers and identity
peer 가 channel 에 연결하면 digital certificate 는 channel msp 를 통해 org 를 식벽한다.
Peer1 과 peer2 에는 ca1 에서 발급한 identities 가 있다. 
channel 은 org1.msp 를 사용해서 ca1 의 id 가 org1 과 연관되어있는지 결정한다. 
peer3 과 peer4 는 org2.msp 에 의해 org2 의 일부로 식별한다.

> channel 은 각 org의 msp 를 가지고 있어야 한다. configtx.yaml 파일에서 지정해서 channel.tx 를 생성하는 과정에 포함되는 듯 한다.

peer 가 channel 을 사용하여 블록체인 네트워크에 연결할 때마다 peer 의 identity 를 사용해서 권한을 결정한다.
Peer의 identity 를 org 에 매핑하는 것은 msp 에 의해 제공된다.


## peers and orderers
application 과 peer 가 서로 상호 작용하여 모든 peer 의 ledger 가 일관되게 유지되도록 하는 메커니즘은 orderers 라는 특수 노드에 의해 중재된다.

단일 peer가 자체적으로 ledger를 업데이트 할 수 없다. peer 의 ledger 를 업데이트 하려면 네트워크의 다른 Peer 의 승인이 필요한다.
이 프로세스를 합의라고 하며 간단한 쿼리보다 완료하는데 오래 걸린다.

이 섹션에서 orderer 및 peer 가 consensus process 를 관리하는 방법에 대해 자세히 설명한다.

ledger 를 업데이트하려는 application 은 orderer 에 직접 제안하는 프로세스에 참여하므로 블록체인 네트워크의
모든 Peer 가 ledger 를 일관성있게 유지할 수 있다.

> peer 가 아닌 applicatino 단에서 orderer 에 proposal 하기 때문에 orderer 는 하위 peer 에 결과를 알리고 할 것이므로
> 모든 Peer 가 ledger 를 일관성있게 유지한다고 하는 것 같다.

### Phase 1 : Proposal
1 단계는 application 과 peer 집합 간의 interaction 하는 과정이다.
application 에서 transaction 을 Proposal 하면 peer 에서 response 와 endorsed 를 applicatiaon 에 돌려준다.
1. T1, proposal (A -> C)
2. T1, proposal (C -> P)
3. T1, response, endorsed (P -> C)
4. T1, response, endorsed (C -> A) 

처음에 application 에서 ledger 를 업데이트 하기 위한 Peer 집합을 선택하는데,
어떤 peer 를 선택할지는 endorsement policy(defined for a chaincode) 에 달려있다.

> 만약에 peer 마다 instantiate 를 호출할 때, endorsement policiy 를 다르게 호출하면 아니구나 instantiate 는 한번만 호출하고 install 이 여러개 호출?

1단계는 application 이 충분한 Peer 로부터 서명된 응답을 받으면 종료된다.
개별 Peer 는 transaction 의 결과가 최신의 state 를 반영했는지 알 수 없다. (비결정성 이라고 표현함)
application 에서는 비교를 위해 transaction 응답을 수집해야 한다. 다음 transaction section 에서 논의할 예정

1단계에서 불일치한 transaction 응답을 원하는대로 버릴 수 있다. 
나중에 application 이 일치하니 않는 트랜잭션 응답 세트를 사용하여 원장을 업데이트하려고 하면 거부된다.

### Phase 2 : Ordering and packaging transactions into blocks
2 단계는 Packaging 단계이다. orderer 는 많은 application 으로부터 
endorsed transaction proposal response 를 수신하고 block 으로 패키징한다. 
https://hyperledger-fabric.readthedocs.io/en/release-1.4/orderer/ordering_service.html#phase-two-ordering-and-packaging-transactions-into-blocks

### Phase 3: Validation and commit
Transaction work flow 의 마지막 단계는 orderer 에서 peer 로 block 을 배포하고 block 을 ledger 에 적용할 수 있는 검증과정이 포함된다.
peer 에서 block 내의 모든 transaction 은 ledger 에 적용하기 전에 모든 관련 org가 승인했는지 확인하기 위해 유효성을 검사한다.

1. Block distribution (O -> C)
2. Block distribution (C -> P1, P2)
3. Block 처리 (P1 -> L1 , ...)
4. Block 처리 완료 이벤트

3단계는 orderer 가 peer 에 블록을 배포하는 것으로 시작한다. peer는 channel 에서 orderer 에 연결되어 새 블록이 생성되면
orderer 에 연결된 모든 peer 에게 새 블록의 사본을 전송한다. 모든 peer 가 orderer 와 연결될 필요는 없다.
peer 는 gossip 프로토콜을 사용하여 다른 Peer 에게 Block을 cascade 할 수 있다.

block 을 수신하면 peer 는 각 트랜잭션을 블록에 나타나는 순서대로 처리한다.
모든 transaction 에 대해 peer는 endorsement 에 필요한 org가 승인했는지 확인한다.
이 검즌 프로세스는 모든 org가 동일한 결과 또는 결과를 생성했는지 확인한다. 여기서
보증 정책을 위반하는 경우, Peer는 트랜잭션을 거부할 수 있다.

거래가 올바르게 승인된 경우, Peer는 거래를 원장에 적용한다. 이 때, peer 는 ledger consistency 를 체크해야 한다. 
ledger current state 와 ledger 를 업데이트 했을 때의 state 가 일치하는지 확인한다.
트랜잭션이 승인 되어도 원장에 적용하지 못할 수 있다. 예를 들어, 트랜잭션 업데이트가 더 이상 유효하지 않아
더 이상 적용할 수 없도록 다른 트랜잭션이 원장에서 동일한 자산을 업데이트했을 수 있습니다.
원장 사본은 유효성 검사에 대해 동일한 규칙을 따르기 때문에 네트워크에서 일관성을 유지합니다.
3단계에서는 체인 코드를 실행할 필요가 없다.

block 이 peer의 ledger 에 commit 될 때마다 해당 peer 는 적절한 event 를 생성한다.
application 은 이벤트 유형을 동록해서 발생 시, 알림을 받을 수 있다.


# ref
[peers](https://hyperledger-fabric.readthedocs.io/en/release-1.4/peers/peers.html)