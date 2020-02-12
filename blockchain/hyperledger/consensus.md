# consensus
블록체인은 분산시스템 
분산되어있는 node 간 block 유효성에 대한 합의가 필요하다. (누가 진짜냐)
트랜잭션과 블록의 유효성을 보장하기 위한 알고리즘 (트랜잭션 실행 순서, 결과 보장)
모든 노드가 동일한 하나의 체인을 가질 수 있도록 특정 메커니즘에 의하여 블록이 생성 및 연결되게 하는 것
종류 : pow, pos, pbft 등
order, execute, validate, commit

---

# kafka ordering service
## solo
single ordering node 구성에 사용
하나가 모든 클라이언트들에 대해 서비스 한다. 
확장성이나 고가용성에 대해 고려되지 않음, 따라서 이것은 그냥 테스트성

---

## kafka
Linkedin에서 개발한 분산 메시징 시스템

![kafka](https://t1.daumcdn.net/cfile/tistory/253BF244550914E21A)
kafka는 pub-sub 구조 내에서 빠르게 메시지를 pull 방식으로 전달하여 수신 피어 측의 부담을 최소화하고 속도를 향상시킬 수 있다

Kafka의 broker는 topic(channel)을 기준으로 메시지를 관리한다. Producer(peer)는 특정 topic의 메시지를 생성한 뒤 해당 메시지를 broker(peer)에 전달한다. 
Broker가 전달받은 메시지를 topic별로 분류하여 쌓아놓으면, 해당 topic을 구독하는 consumer들이 메시지를 가져가서 처리하게 된다.

---

Kafka는 확장성(scale-out)과 고가용성(high availability)을 위하여 broker들이 클러스터로 구성되어 동작하도록 설계되어있다. 
심지어 broker가 1개 밖에 없을 때에도 클러스터로써 동작한다. 
클러스터 내의 broker에 대한 분산 처리는 아래의 그림과 같이 Apache ZooKeeper가 담당한다
![kafka-zookeeper](https://t1.daumcdn.net/cfile/tistory/270D49435509151E2A)

---

Kafka가 하는 역할에 대해  CFT (Crash Fault Tolerance) 같은 용어를 쓰는데, BFT처럼 제대로된(?) 합의 알고리즘이 아니라,
주키퍼-카프카 구성으로 장애에 대한 대처가 가능한 상태에서 오더링을 하기 때문이다. (클러스터 구성)

---

kafka 가 ordering service 에서 하는 역할 : 순서대로 쌓이도록 하고, 설정대로 포장
#
- BatchTimeout
- MaxMessageCount
- AbsoluteMaxBytes
- PreferredMaxBytes
#

![transaction flow](https://getoutsidedoor.com/wp-content/uploads/2018/09/transaction-flow-5.png)

---


## raft
뗏목


# ref
[Hyperledger Fabric Architecture: 6 합의 알고리즘](https://medium.com/@yjw113080/hyperledger-fabric-architecture-6-%ED%95%A9%EC%9D%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-d59857005b4d)
[kafka 소개](https://epicdevs.com/17)
[[하이퍼레저 패브릭] 합의 알고리즘 (consensus algorithm)](https://hamait.tistory.com/1006)
[블록체인 합의 알고리즘 알아보기 2편(PBFT, Sieve, Tendermint, Raft, Paxos, PoA)](https://medium.com/@kimjunyong/6-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%ED%95%A9%EC%9D%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-2%ED%8E%B8-pbft-sieve-tendermint-raft-paxos-poa-a8af8d6eaccd)