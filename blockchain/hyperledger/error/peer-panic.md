# peer panic with invalid blocks [FAB-13470]
## test scenario
트랜잭션을 commit 하고 peer를 내렸다가 올렸을 때, 동기화 과정에서 발생하는 문제?
network: 1 channel, 5 orgs, 2 peers per org, only org5 has persistent data
1. execute transactions
2. bring down network, org5 has blocks
3. then bring up network with org 5 has persistent data from previous test
4. create the same channel and join to channel, the peers of org5 fails as expected due to persistent data
5. ledgers begin to catch-up since the persistent data in org5 peers
6. then the first peer of org1-org4 panic

## 답변
peer 를 rebuild 해야함


## ref
[FAB-13470 peer panic with invalid blocks - Hyperledger JIRA](https://jira.hyperledger.org/browse/FAB-13470)
