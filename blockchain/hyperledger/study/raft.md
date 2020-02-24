

# Raft?

Byzantine Fault
악의적인 노드에 의한 합의 문제를 해결 중점
- pow, pos, pbft, rbft

Non-Byzantine Fault
system crash, network down, network delay, packet loss 해결 중점
- Kafka, Paxos, Zookeeper.

----

# Why Raft?

BFT 계열은 일반적으로 공용네트워크

Fabric 은 private network 
신뢰할 수 있는 노드 확보
MSP(인증서) 사용
=> 악의적인 노드 제한할 수 있다.

악의적인 노드들을 배제하고 조금더 효율적으로 운영한다. (성능)

----

# Raft?
RAFT is distributed crash Fault tolerance consensus algorithm.

In technical term Raft is a consensus algorithm for managing a replicated log Replicated log is a part of Replicated state machine(RSM).

----

## Replicated State Machine(RSM)


![rsm](https://miro.medium.com/max/1072/0*Spg9YniaVRqzSmW8.png)
1 : 클라이언트는 명령으로 구성된 요청을 Leader 노드에 보낸다.
2 : Leader 노드가 새 요청을 로그에 추가하고이 요청을 모든 Follower 노드에 보낸다. follwer 노드는 요청을 로그에 추가하고 확인 메시지를 보낸다.
3 : 대부분의 노드가 확인 메시지를 보내면 Leader는 로그를 상태 머신에 커밋하여 일부 출력을 생성합니다. Leader가 로그를 커밋하면 follwer들도 SM에 로그를 커밋합니다.
4 : Leader가 클라이언트에게 응답을 보냅니다.


----


## Basic
### Node State
![basic feature of raft](https://miro.medium.com/proxy/1*_B3mkKkJiCXJDQJNdd17KA.png)

Follower(F): Initially all node will be in follower state. If a follower receives no communication, it becomes a Candidate. Follower’s log can be overridden by the Leader.
Candidate(C): Initiate election and if it receives votes from the majority, it becomes a leader.
Leader (L): handle client request and make sure that all follower has the same replica of logs. A leader can’t override its log.

----

### Term
Term is the arbitrary length, numbered with consecutive integer
![term](https://miro.medium.com/max/800/0*CSr-CwEm3apz782M.jpg)

----

### Leader Election 
leader 는 권한을 유지하기 위해 모든 follower 에게 주기적으로 heartbeats 를 날린다.
follwer 는 election timeout 동안 아무런 통신도 받지 못하면 새로운 지도자를 선출하기 위해 선거를 시작한다.
![election timeout](https://miro.medium.com/max/1260/1*oBCzKBamZNiWqHuV-WNOFw.png)

----

### Leader Election (시작)
Follwer 시점
Follower 는 current term 을 늘리고 candiate state 로 전환
Vote Itself, Request Vote 전송
![Request Vote](https://miro.medium.com/max/926/1*YS-hODwCUG4N2kZPhVTqsQ.png)

----
### Leader Election (결과1)
리더가 된 경우
![vote 1](https://miro.medium.com/max/890/1*NMqbNKCZFZAi1oO8LgPrwA.png)

----

### Leader Election (결과2)

다른 노드가 리더가 된 경우
![vote 2](https://miro.medium.com/max/1950/1*tYSWw-YOBla2muY-ThZ4_g.png)

----

### Leader Election (결과3)
분할 투표
re-initiate election,
make sure that each server has random election time out
![vote 3](https://miro.medium.com/max/1002/1*cUGmx_pPoss6uTvbWut-Sg.png)

----

## Replicated Process
리더는 클라이언트 요청 처리, AppendEntries RPC 날려서 Log 를 replicated
![commit entries](https://miro.medium.com/max/1290/0*ZwXUApD3CBa_VA6G.png)

----

## Aergo (블로코)
### 데이터
Raft Consensus를 위해 저장하는 데이터는 크게 Raft Log와 블록 관련 Data로 나누어 진다.
저장된 블록은 Commit 과정을 거친 후 블록체인에 추가될 때에 관련 Account 정보와 Chain 연결 정보 등이 저장된다.
- Raft Log
- 블록 Data : Raft Log 의 Data 형태로 저장
![저장소](https://www.blocko.io/wp-content/uploads/2019/09/1.png)

----

### Raft Log
- Term : Leader 선출 시에 할당되는 1씩 증가하는 Sequence 번호이다. 따라서 각 Leader에 붙은 ID 역할을 한다
- Index : 새로운 Log가 저장될 때마다 1씩 증가하는 Sequence 번호이다. 
- Data : Raft 를 통해 공유되는 정보 (블록)
![term](https://www.blocko.io/wp-content/uploads/2019/09/2.png)

----

## Raft Message
- AppendEntry : Leader 가 Log 를 Follower 에 전파 (Log 에 블록이 포함, 블록은 Log에만 저장, 블록체인에는 연결되지 않는다)
- Heartbeat : Leader 와 Follwer 사이에 liveness 를 체크, commit 정보를 전달한다. commit 정보를 받고, commit 된 log 에 저장된 블록 실행하여 블록체인에 추가

----

## 동기화
1. Leader 는 새로 발행한 Log 를 AppendEntry 메시지로 보낸다. 새로운 Log 정보와 Previous Entry 의 meta 정보(term/index)
2. Follower 는 previous entry 의 meta 정보가 log 저장소에 저장된 마지막 로그와 일치하는지 검사
3. 일치하면 Log entry 를 저장하고 AppendEntryResponse 메시지로 성공 응답
![appendentry](https://www.blocko.io/wp-content/uploads/2019/09/3.png)

----


# Hyperledger fabric 에서 raft 구현
Raft 는 Kafka 를 대체하는 ordering service 로 사용
각 노드들은 Raft State Machine(RSM)
![raft osn](https://miro.medium.com/max/1230/0*Cn_uq-E9leCA5zjo)

----

## 프로세스
1 : 트랜잭션을 leader 에게 전달
2 : 리더는이 트랜잭션의 유효성이 검증 된 구성 순서 번호가 현재 구성 순서 번호와 같은지 확인합니다. (그렇지 않은 경우 트랜잭션을 다시 확인하고이 유효성 검사에 실패하면 거부합니다.)
리더는 들어오는 트랜잭션을 블록 커터의 Ordered 메소드로 전달합니다. 블록이 아직 형성되지 않으면 아무 것도 수행하지 않습니다. 그렇지 않으면 후보 블록을 만듭니다.
3 : 블록이 형성되면 리더가 로컬 Raft Finite State Machine에 제안합니다.
4 : FSM은 블록을 충분한 OSN에 복제하여 커밋 될 수 있도록 시도합니다.
5 : 그러면 수신 복제본의 로컬 원장에 블록을 쓸 수 있습니다.


----


# ref
[Raft protocol](https://raft.github.io/raft.pdf)
[raft-fabric-sample](https://github.com/IBM/raft-fabric-sample)
[Detail Analysis of Raft](https://medium.com/@spsingh559/detail-analysis-of-raft-its-implementation-in-hyperledger-fabric-d269367a79c0)
[Raft consensus를 이용한 동기화 이해하기](https://www.blocko.io/blockchain/raft-consensus를-이용한-동기화-이해하기/)