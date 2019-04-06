
# 블록
블록 헤더, 엉클 블록, 트랜잭션으로 구성된다

```go
type Header struct {
	ParentHash  common.Hash    // 부모 블록 헤더의 해시
	UncleHash   common.Hash    // 엉클 블록들의 해시값
	Coinbase    common.Address // 블록의 마이닝 작업 후 이더를 지급받을 어카운트 주소
	Root        common.Hash    // 어카운트 상태 정보가 모여 있는 머클 패트리시아 트리의 루트 노드에 대한 해시값
	TxHash      common.Hash    // 블록 내 모든 트랜잭션의 리시트들의 머클트리의 루트 노드
	ReceiptHash common.Hash    // 블록 내 모든 트랜잭션의 머클트리의 루트 노드
	Bloom       Bloom          // 로그 정보를 검색하는데 사용하는 32바이트 블룸 필터
	Difficulty  *big.Int       // 블록의 난이도
	Number      *big.Int       // 블록의 번호
	GasLimit    uint64         // 최대 가스 총합
	GasUsed     uint64         // 트랜잭션에 의해 사용된 가스의 총합
	Time        *big.Int       // 블록의 최초 생성 시간
	Extra       []byte         // 추가 정보
	MixDigest   common.Hash    // 마이닝에 사용하는 정보
	Nonce       BlockNonce     // 마이닝에 사용하는 정보
}
```

# 엉클 블록
블록 생성에 성공하였고 검증에 오류가 없어 다른 노드에 브로드캐스팅 되었으나 다른 마이너의 블록에 비해
난이도가 낮아 등록되지 못한 블록
## 엉클 블록이 발생시키는 문제
- 트랜잭션 처리 지연
- 컴퓨터 파워의 낭비
- 보안 문제, 엉클 블록 생성으로 블록 생성시간이 길어지고 난이도가 줄어든다 난이도가 줄어들면
블록 타임이 줄어들고 컴퓨팅 파워가 큰 마이너의 영향력이 커진다. 영향력이 커진 마이너에 의해 악의적으로
변경될 수 있다.

# 고스트 프로토콜
엉클블록의 문제를 고스트 알고리즘을 사용하여 해결한다.
- 하나의 블록은 하나의 부모 블록을 지정하고, 0 또는 그 이상의 엉클 블록을 지정한다.
- 블록 A에 포함된 엉클 블록은 다음과 같은 속성을 갖는다.
  - 블록 A의 k번째 조상의 자손이어야 한다. 2 <= k <=7, 한 블록이 생성된 후, 6개의 블록이 연결될 때까지 기다린다.
  - 블록 A의 조상이어서는 안된다.
  - 엉클 블록은 반드시 유효한 블록 헤더를 가져야 하지만 미리 검증되거나 유효한 블록일 필요는 없다.


# BlockChain
```go
type BlockChain struct {
	chainConfig *params.ChainConfig // Chain & network configuration
	cacheConfig *CacheConfig        // Cache configuration for pruning

	db     ethdb.Database // Low level persistent database to store final content in
	triegc *prque.Prque   // Priority queue mapping block numbers to tries to gc
	gcproc time.Duration  // Accumulates canonical block processing for trie dumping

	hc            *HeaderChain
	rmLogsFeed    event.Feed
	chainFeed     event.Feed
	chainSideFeed event.Feed
	chainHeadFeed event.Feed
	logsFeed      event.Feed
	scope         event.SubscriptionScope
	genesisBlock  *types.Block

	mu      sync.RWMutex // global mutex for locking chain operations
	chainmu sync.RWMutex // blockchain insertion lock
	procmu  sync.RWMutex // block processor lock

	checkpoint       int          // checkpoint counts towards the new checkpoint
	currentBlock     atomic.Value // Current head of the block chain
	currentFastBlock atomic.Value // Current head of the fast-sync chain (may be above the block chain!)

	stateCache   state.Database // State database to reuse between imports (contains state cache)
	bodyCache    *lru.Cache     // Cache for the most recent block bodies
	bodyRLPCache *lru.Cache     // Cache for the most recent block bodies in RLP encoded format
	blockCache   *lru.Cache     // Cache for the most recent entire blocks
	futureBlocks *lru.Cache     // future blocks are blocks added for later processing

	quit    chan struct{} // blockchain quit channel
	running int32         // running must be called atomically
	// procInterrupt must be atomically called
	procInterrupt int32          // interrupt signaler for block processing
	wg            sync.WaitGroup // chain processing wait group for shutting down

	engine    consensus.Engine
	processor Processor // block processor interface
	validator Validator // block and state validator interface
	vmConfig  vm.Config

	badBlocks *lru.Cache // Bad block cache
}
```


# etc
etherscan.io 는 현재 작동 중인 이더리움의 모든 블록체인 정보를 탐색하고 조회할 수 있는 서비스 제공
블록체인의 정보를 동기화해서 해당 정보를 MongoDB로 옮긴 후 분석하고 있다.
