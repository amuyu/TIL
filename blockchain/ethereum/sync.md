# 동기화
- full sync : 전체 블록 동기화
- fast sync : 블록 헤더만 동기화, 트랜잭션 유효성 등을 검증할 수 없다.
- light sync : 현재 상태 정보 동기화, 세부 항목들의 검증이 필요한 경우 세부항목 값을 포함하고 있는 전체 정보를 다운로드 한다.


# 동기화
handler.handleMsg => pm.synchronise(p)
* pm : protocolmanager
downloader.synchronise
syncState() ,statesync.go
NewStateSync, sync.go
trie.NewTrieSync, sync.go
AddSubTrie, sync.go
Process, sync.go
BroadcastBlock, sync.go

Commit, sync.go
  ethd.Putter.put
commit, statesync.go : loop 돌면서 commit

what is schedule? (request)
fetcher.go
bc.insertChain 에서 bc.WriteBlockAndState 호출, 블록체인 생성
