# transaction Flow
1. transaction proposal
2. endorsed transaction
3. check endorsed transaction
4. block pacakaging
5. distribute and check transaction
6. commit


escc, vscc

endosing peer
committing peer
Leader Peer 

# Committer peer 
모든 Peer 는 committer peer 임

# Commit 이란?
Ledger 에 block 을 반영하는 것을 말함

# vscc
보증 정책 조건 충족 여부 확인
거래들간의 충돌이 없는지 확인 MVCC?

# validated ledger (VLedger)
valid and committed Transaction 들만 유지하기 위해 peer 들은 VLedger 유지할 수 있습니다.
VLedger 는 유효하지 않은 transaction 들을 걸러낸 hash chain 입니다.

PeerLedger blocks 은 invalid transactions 들을 가지고 있습니다. (endorsement 때문이거나 Version 이 안맞거나)
이러한 transactions 들은 vBlock 에 추가되기 전에 필터링한다.
vBlock 은 크기가 동적이고 비어있을 수 있다.

vBlock 은 모든 peer 에 의해 hash chain 으로 연결된다. 모든 block 은 다음 정보를 포함한다.
- 이전 vBlock의 hash
- vBlock Number
- 정렬된 transaction list
- peerledger 의 hash

# PeerLedger checkpointing
peerledger 는 invalid transaction 을 포함하며, 영원히 기록되지 않는다.
그러나 peer는 peerledger 를 vBLock을 설정하면 정리할 수 없다. 
새 peer 가 네트워크에 참여하는 경우 다른 peer 는 폐기된 block 을 참여 peer 에게 전송할 수 없다.

checkpointing mechanism 은 vBlock 의 유효성을 설정하고 checkpointed vBlock 이 peerledger 를 대신할 수 있습니다.



# 이건 또 뭔 소리?
https://medium.com/landingblock-korea/26-하이퍼레저-패브릭-hyperledger-fabric-이해-a64e8fccd357
endorser 는 항상 committer 가 됨

# 알고 싶은 것
block commit 과정에서 block sync? 등의 이유로 commit 을 못하는 경우, 어떻게 동작하는지 

# ref
[peers](https://hyperledger-fabric.readthedocs.io/en/release-1.4/peers/peers.html)
[transaction flow](https://hyperledger-fabric.readthedocs.io/en/release-1.4/txflow.html)