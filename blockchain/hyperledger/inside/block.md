# Block add

1. service/gossip_service.go - InitializeChannel
state provider 생성
coordinator 객체 생성

2. state/state.go - NewGossipStateProvider
원장에 커밋하기 전에 개인 rwset(read write set) 및 블록의 도착을 조정합니다.? 라는 말이...뭐지? 상태 제공자?
gossipchannel 의 상태 정보를 업데이트 한다. (BlockHeight)
channel 내부에 message 를 던진다.

3. state/state.go - deliverPayloads
ledger 에 block payload 정보를 전달한다.
GossipStateProviderImpl 구현체의 commitBlock 을 호출한다.

4. state/state.go - commitBlock
ledgerResources.StoreBlock 을 호출한다.
`NewGossipStateProvider` 을 호출하는 부분을 보면 ledger 로 coordinator 를 넣고 있다. (gossip_service.go)

5. privdata/coordinator.go - StoreBlock
ledger 에 block 을 저장한다.
committer_impl.go - DoesPvtDataInfoExistInLedger
ledgerCommiter 가 pvtdata 를 갖고 있는지 확인한다. 있으면 block 을 commit 없으면 안한다. (peerledger 와 vledger?)
commit 할 data 가 있으면 CommitWihtPvtData 를 호출한다. (vledger)

5-1. ledgerstorage/store.go - DoesPvtDataInfoExistInLedger ??
주어진 block number 에 대해서 ledger 가 pvtdata info 를 들고 있는지 확인 (아마도 peerledger 를 확인하는듯)
마지막 commit된 block height 과 block height 을 비교해서 확인
근데 (blockNum+1 <= pvtStoreHt) ???  pvtStoreHt = lastCommittedBlock + 1???
commit 하려는 blocknumber 가 들고 있는 blcknumber 보나 작아야 한다.??
// DoesPvtDataInfoExist returns true when
// (1) the ledger has pvtdata associated with the given block number (or)
// (2) a few or all pvtdata associated with the given block number is missing but the
//     missing info is recorded in the ledger (or)
// (3) the block is committed does not contain any pvtData.

6. committer/committer_impl.go - CommitWithPvtData

7. kvledger/kvledger.go - CommitWithPvtData

8. ledgerstorage/store.go - CommitWithPvtData


9. blkstorage/fsblkstorage.go - AddBLock
block 을 추가하고 blockcommit 에 걸린 시간과 추가한 blockNumber 를 update 함

10. fsblkstorage/blockfile_mgr.go - addBlock
BlockchainInfo 정보(peer 에서 들고있는 blockchain 정보)를 호출해서 
추가하려는 block previousHash 와 bcInfo 의 currentBlockHash 를 비교해서 다른 경우, 에러가 발생함
이상이 없으면 block 을 serializeBlock 하고 기록한 후
updateCheckpoint, updateBlockchainInfo 를 호출함



> 10번 과정에서 에러가 발생한 이후의 처리는?
store.CommitWithPvtData 에서 AddBlock 호출 후, 에러가 발생하면 Rollback 호출함
coordinator.StoreBlock 에서 commit failed 메시지 출력
state.deliveryPayloads 에서 error log 남기고 pass

> AddBlock 을 호출하는 곳은?
state.go -> deliverPayloads
state.go -> commitBlock
coordinator.go -> StoreBlock
committer_impl.go

> store_imple 에서 lastCommittedBlock 은?
Peerledger 동기화하면서 update 된다.
store.syncPvtdataStoreWithBlockStore 에서 갱신됨
// syncPvtdataStoreWithBlockStore checks whether the block storage and pvt data store are in sync
// this is called when the store instance is constructed and handed over for the use.
// this check whether there is a pending batch (possibly from a previous system crash)
// of pvt data that was not committed. If a pending batch exists, the check is made
// whether the associated block was successfully committed in the block storage (before the crash)
// or not. If the block was committed, the private data batch is committed
// otherwise, the pvt data batch is rolledback


> pvtData 가 뭔지?
https://hyperledger-fabric.readthedocs.io/en/release-1.4/private-data/private-data.html
core.yaml 에 pvtData 항목이 있넴?
privatedata 일거 같은데 .. 맞는 듯... 조직내에서 비공개로 데이터를 유지하기 위한 데이터를 말함
예를 들어, StoreBlock 할 때,  DoesPvtDataInfoExistInLedger 를 체크하는데
함수 결과가 false 이면, block 을 저장하지 않음... 뭔가 애매함

> peerledger 는 언제 commit?