# gossip protocol
peer 는 gossip 을 활용하여 확장 가능한 방식으로 ledger 및 channel data 를 브로드 캐스트한다.
gossip 은 지속적이며 channel 의 각 peer 는 지속적으로 최신 ledger data 를 수신한다.

1. 사용 가능한 member peer 를 지속적으로 식별하고 오프라인 상태인 Peer를 감지하여 peer 검색 및 channel 멤버쉽을 관리한다.
2. channel 모든 Peer 에 ledger 를 전파한다. 나머지 채널과 동기화되지 않은 데이터를 가진 Peer 는 누락된 블록을 식별하고 올바를 데이터를 복사해서 자체적으로 동기화한다.
3. ledger data 의 peer to peer 상태 전송 업데이트를 허용해서 새로 연결된 Peer 의 속도를 높일 수 있다.

## Anchor peer 
gossip 에서 다른 조직의 peer 가 서로에 대해 알 수 있도록 하는데 사용한다.

# ref
[gossip protocol](https://hyperledger-fabric.readthedocs.io/en/release-1.4/gossip.html)