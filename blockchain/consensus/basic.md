
BFT(Byzantine Fault Tolerance)
배신자가 전체 중에서 1/3보다 작아야 하고, 전체 중 2/3보다 많으면 올바른 결정을 하게 되는 것입니다. 

# LFT
Broadcast Block -> Verify Block -> Broadcast vote -> Count Vote -> commit(과반이상) & 새로운 block 제안 

# LFT2(Loop Fault Tolerance 2)
PBFT를 경량화하여 성능을 향상시킨 합의 알고리즘으로, 기존 PBFT보다 높은 확장성과 네트워크 대역폭을 보장하는 동시에 완전한 안전성(Safety) 및 생존성(Liveness) 또한 확보했다.
https://github.com/icon-project/LFT2/blob/master/Whitepaper%20-%20LFT2%20(ENG).pdf
Round1 에서 블록1을 제안할 때, 블록0 을 commit
Round2 에서 블록2를 제안하면서, 블록1을 commit

# PBFT(Practical Byzantine Fault Tolerance)
https://steemit.com/consensus/@kblock/48-pbft-consensus-algorithm
Tendermint는 PBFT에 DPoS 합의 알고리즘을 결합
간단히 요약 하면,
비동기 네트워크에서 배신자 노드가 f개 있을 때, 총 노드 개수가 3f+1개 이상이면 해당 네트워크에서 이루어지는 합의는 신뢰할 수 있다는 것을 수학적으로 증명한 알고리즘

MIGUEL CASTRO, BARBARA LISKOV 의 논문 "Practical Byzantine Fault Tolerance and Proactive Recovery" 참고
0. request(client->primary)
1. pre-prepare(primary->backup) 
primary 노드가 pre-prepare 메시지를 backup 노드들에게 전달
2. prepare(backup->all)
pre-prepare 메시지를 받은 노드들은 검증 결과가 참이면 prepare 메시지를 모든 노드들에게 전달
3. commit(->all)
pre-prepare, prepare 메시지를 수집해서 
Pre-prepare 메시지 개수가 2f+1개이고 Prepare 메시지가 2f개 이상인 경우  "prepared certificate"가 되고,
모든 노드들에게 commit 메시지 전달
4. reply

한번의 합의 절차만을 전제로 하면, 한 명의 배신자 노드로 인해서 safety 가 깨지게 됨 
노드 1은 블록1을 노드2, 3, 4는 블록2를 가지고 있는 상황이 존재함

# tendermint
tendermint 합의 알고리즘
propose(블록제안) -> prevote(propose 블록 검증) -> precommit(prevote 2/3 이상) -> commit(precommit 2/3 이상상