data > rlpencode > trie
blockChain
node - block - transaction
contract byte code > evm에서 op코드로 변환
account, transaction, message, receipt, block, blockchain

# trie
디지털 tree를 나타내는 용어
(root, path, node, leaf)

## Radix trie
Rasix tire is used to optimize for searching
the key in dataset will be the path to reach the value.
trie 의 path는 ascii 문자이고 값을 찾는데 사용한다.

```js
[i0, i1 ... in, value]  // i0 ... in is the symbols of the alphabet
```

## Merkle trie
머클트리는 이진 트리이다.
Merkle trie is used to authenticate data
데이터를 검증하기 위해서 결정적 암호화 해쉬를 만들어서 사용한다
전체 데이터는 leaf 에 저장되고 parent node 는 각가의 hash 로 구성된다

데이터 변조를 막을 수 있고, 특정 자식 노드 중 하나의 해시값을 알면 그 노드의
모든 자식 노드의 데이터를 검증할 수 있다.

leaf 의 value 를 변경하면 그 위의 path 들의 node들은 다 변경되어야 한다
root의 변경없이 데이터 조작은 불가능하다

이더리움은 머클 트리를 사용하여 전체 어카운트 정보를 담고 있는
상태(Root)와 트랜잭션(TxHash) 그리고 트랜잭션의 처리 결과 등을 알 수 있는 리시트(ReceiptHash)
의 값을 저장하고 머클 루트를 keccak256으로 암호 해싱한 후 이를 해당 블록 헤더에 포함한다.

이렇게 해서 블록체인의 전체 데이터를 다운로드하지 않고 블록 헤더만 다운로드해도
다양한 블록체인의 상태를 조회할 수 있다.

### 좀 더 설명
두 개씩 거래를 묶어서 올라가면 거래량이 기하급수적으로 늘어나도
특정 거래를 찾는 경로는 단순하다. 거래의 건수인 N이 증가할 때마다
특정 거래의 경로를 찾는 경우의 수는 log2(N)으로 늘어난다.

8개의 거래를 머클트리로 만든다고 하면 거래를 찾기 위해서는 세번의
경로만 찾아가면 된다. 1024개의 거래라고 해도 열번의 경로를 찾아가는 연산으로
데이터를 찾을 수 있다.

### 검증
데이터에 대한 검증을 할 때, 머클의 루트 트리와 데이터를 갖고 있다면
path 를 통해서 검증을 할 수 있다. path를 대로 해쉬를 하여 마지막 해쉬값을 구해
머클 루트 해쉬와 값이 같은지 확인한다.

### Q
머클 트리를 사용해서 경량 동기화할 수가 있다
트랜잭션이 어떤 블록에 포함되어 있는지? 어카운트의 이더 잔액이 얼마인지?
어카운트가 존재하는지 등 다양한 조회가 가능하다.. 어떻게?
검증이냐 조회냐


## ethereum 의 Merkle 증명
transaction, receipts, state tree 가 있다.

## 머클 패트리시아 트리
블록체인의 모든 데이터를 참여자들이 공유하기 때문에 블록체인의 크기가 증가한다면 공유를 위해
많은 데이터를 동기화해야 한다. 트리 내 모든 정보는 레벨 DB에 저장한다.

어카운트 정보를 포함하고 있는 상태(Root) 는 key, value, map 구조이기 때문에 자주 변경된다.
이렇게 상태가 자주 변경되는 특징 때문에 머클 트리에 패트리시아 트리의 특징을 결합한
머클 패트리시아 트리를 사용한다.

You can see that, we used path to find the value (an attribute of Radix trie) and if a value in trie is changed,
it will cause the rootHash of trie changed (an attribute of Merkle trie).

The main advantage of the Patricia trie is its compact storage.

### 머클 패트리시아 트리의 특징
- 트리 내 모든 항목은 RLP 인코딩
- 트리 내 모든 노드에 대한 경로는 RLP 인코딩 후 Keccak256 암호 해시하여 레벨 DB에 저장된다.
  - 레벨 DB에 저장되는 키는 다음 노드에 대한 path가 된다.
  - 트리 내에 다음 노드의 경로를 알 수 있는 키로 조회하면, 다음 노드에 대한 접근 경로를 알 수 있다.
  - 이 과정을 반복해서 마지막 노드에 저장된 경로값을 찾을 수 있다.
- 트리는 blank, leaf, extension, branch 라는 네 가지 노드 타입을 갖는다.
  - blank : 비어있는 노드
  - leaf : [path, value] (value 는 ether 같은 실제 값)
  - extension : [path, key] (key는 연결된 노드의 해시값, level DB 호출 시 키값)
  - branch : [0, ..., f, value] 로 17개 항목으로 구성된 리스트 구조,
            0~f는 다음 노드를 가리키는 역할을 하기 때문에 16 개의 자식 노드를 가질 수 있다.

#### hp encoding
- Distinguish leaf and extension from each other without terminator.
- Convert the path to even length.

#### HP encoding spec
노드의 구분
```
node type    path length    |    prefix    hexchar
--------------------------------------------------
extension    even           |    0000      0x0
extension    odd            |    0001      0x1
leaf         even           |    0010      0x2
leaf         odd            |    0011      0x3
```

### 상태 머클 패트리시아
이더리움에서 월드 상태 트리 구조에서 경로를 나타내는 키값을 따라가면
머클 패트리시아 트리의 경로를 따라 리프 노드에 저장되어 있는 이더값을 조회할 수 있다.

### key-value
leafnode 의 value element 는 partialPath, data로 이루어진다.
partialPath 는 hp encoded (hex-prefix)
node의 elements 는 rlp encode
value rap encode

ethereum 에서 patricia 트리는 네가지 트리에서 사용한다
- stateRoot
- storeageRoot
- transactionRoot
- receiptRoot



# Patricia trie find path
// DataSet
```js
{
  '39': 'chicken',
  '395': 'duck'
}
```
이더리움에서 branch node (patricia trie)는 key-value를 저장한다.
- key : node의 hash
- value : 17 elements array (~16:index, 17:value)

위의 DataSet을 보면 key 가 path에 해당한다
아래와 같은 trie가 있다고 할 때, 395의 value를 찾는 방법은
다음과 같다. key ’395’를 3,9,5 로 나누고 순서대로 내려간다.
trie의 node를 찾고 index 대로 따라간다.

key - value
rootHash - [3]hashA, [5]hashE, [C]hashB, data
hashA - [9]hashC, null
hashC - [5]hashD, chicken
hashD - duck


# trie 와 leveldb
간단하게 구현했을 때..
this.putRaw(hash, serialized, cb)
## TrieNode
type, key, value
## TryeUpdate
```go
k = keybtyesToHex(key)
// insert(n node, prefix, key []byte, value node)
t.insert(root, nil, k, valueNode(value))
```


### 머클 상태 전이 증명
블록 헤더에는 상태 정보 트리, 트랜잭션 트리, 리시트 트리의 암호화된 해시 루트를
포함함으로써 다양한 정보를 조회할 수 있다.
- 해당 트랜잭션이 특정 블록에 포함되어 있는가?
- 특정 어카운트의 현재 잔액은?
- 해당 어카운트가 현재 존재하는가?

상태 트리 루트와 트랜잭션과 리시트 트리 루트에서 관련 정보를 조회해서
결과를 재현해봄으로써 상태 전이가 맞는지 증명할 수 있다.


# db
Ethereum’s Rust client Parity uses rocksdb. Whereas Ethereum’s Go, C++ and Python clients all use leveldb.

## level db
구글이 만든 key-value 기반 storage library

## Data item prefixes
```go
// package core , file : database_util.go
headerPrefix        = []byte("h") // headerPrefix + num (uint64 big endian) + hash -> header
tdSuffix            = []byte("t") // headerPrefix + num (uint64 big endian) + hash + tdSuffix -> td
numSuffix           = []byte("n") // headerPrefix + num (uint64 big endian) + numSuffix -> hash
blockHashPrefix     = []byte("H") // blockHashPrefix + hash -> num (uint64 big endian)
bodyPrefix          = []byte("b") // bodyPrefix + num (uint64 big endian) + hash -> block body
blockReceiptsPrefix = []byte("r") // blockReceiptsPrefix + num (uint64 big endian) + hash -> block receipts
lookupPrefix        = []byte("l") // lookupPrefix + hash -> transaction/receipt lookup metadata
bloomBitsPrefix     = []byte("B") // bloomBitsPrefix + bit (uint16 big endian) + section (uint64 big endian) + hash -> bloom bits
```

## history
web3 호출 getBalance

GetBalanceAt - ethclient eth_getBalance

backend#StateAndHeaderByNumber

hashroot 는 headerByNumber 를 사용해서 가져옴
HeaderChain 에서 currentHeader 를 가져옴
```go
// package : eth
func (b *EthApiBackend) StateAndHeaderByNumber(ctx context.Context, blockNr rpc.BlockNumber) (*state.StateDB, *types.Header, error) {
	// Pending state is only known by the miner
	if blockNr == rpc.PendingBlockNumber {
		block, state := b.eth.miner.Pending()
		return state, block.Header(), nil
	}
	// Otherwise resolve the block number and return its state
	header, err := b.HeaderByNumber(ctx, blockNr)
	if header == nil || err != nil {
		return nil, nil, err
	}
	stateDb, err := b.eth.BlockChain().StateAt(header.Root)
	return stateDb, header, err
}

// header
// blockChain
func (b *LesApiBackend) HeaderByNumber(ctx context.Context, blockNr rpc.BlockNumber) (*types.Header, error) {
	if blockNr == rpc.LatestBlockNumber || blockNr == rpc.PendingBlockNumber {
		return b.eth.blockchain.CurrentHeader(), nil
	}
	return b.eth.blockchain.GetHeaderByNumberOdr(ctx, uint64(blockNr))
}

// block chain
// StateAt returns a new mutable state based on a particular point in time.
func (bc *BlockChain) StateAt(root common.Hash) (*state.StateDB, error) {
	return state.New(root, bc.stateCache)
}

// StateDB
func New(root common.Hash, db Database) (*StateDB, error) {
	tr, err := db.OpenTrie(root)
	if err != nil {
		return nil, err
	}
	return &StateDB{
		db:                db,
		trie:              tr,
		stateObjects:      make(map[common.Address]*stateObject),
		stateObjectsDirty: make(map[common.Address]struct{}),
		logs:              make(map[common.Hash][]*types.Log),
		preimages:         make(map[common.Hash][]byte),
		journal:           newJournal(),
	}, nil
}

// StateDB
// Retrieve the balance from the given address or 0 if object not found
func (self *StateDB) GetBalance(addr common.Address) *big.Int {
	stateObject := self.getStateObject(addr)
	if stateObject != nil {
		return stateObject.Balance()
	}
	return common.Big0
}

stateObjects
trie 형태로 저장되어 있음, addr 이 key로 해서 value를 가져오면됨
가져온 데이터는 decode 해서 stateObject 객체로 변환해야함
// getStateObject
func (self *StateDB) getStateObject(addr common.Address) (stateObject *stateObject) {
	// Prefer 'live' objects.
	if obj := self.stateObjects[addr]; obj != nil {
		if obj.deleted {
			return nil
		}
		return obj
	}

	// Load the object from the database.
	enc, err := self.trie.TryGet(addr[:])
	if len(enc) == 0 {
		self.setError(err)
		return nil
	}
	var data Account
	if err := rlp.DecodeBytes(enc, &data); err != nil {
		log.Error("Failed to decode state object", "addr", addr, "err", err)
		return nil
	}
	// Insert into the live set.
	obj := newObject(self, addr, data)
	self.setStateObject(obj)
	return obj
}
```

trie를 가져오는게...
block > headerRoot >




## 참고
[ethereum wiki](https://github.com/ethereum/wiki/wiki/Patricia-Tree)
[github/maerkle-patricia-tree](https://github.com/ethereumjs/merkle-patricia-tree)
[Merkling in Ethereum](https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/)
[merkle tree 란 뭔가요](https://steemit.com/kr/@jsralph/merkle-trees)
[Exploring Ethereum's state trie with Node.js](https://wanderer.github.io/ethereum/nodejs/code/2014/05/21/using-ethereums-tries-with-node/)
[머클트리를 알아보자 02](https://steemit.com/kr/@twinbraid/5uzvbu-02)
[머클트리를 통해 자료를 검증하기](http://coinnews.tistory.com/76)
[해시함수와 패트리샤 트리 강의](https://www.youtube.com/watch?v=WllX5j5USJs)
[Understanding the ethereum trie](https://easythereentropy.wordpress.com/2014/06/04/understanding-the-ethereum-trie/)
[Merkle Tree& Patricia Merkle Tree](https://docs.google.com/presentation/d/1J8vbpvo7E8B0-h9uHsm7Sp94g3CA2VjjvTIqVNucwyg/edit#slide=id.g34da99002e_0_47)
[Secure Tree - state trie의 키가 256 bit인 이유](https://blog.seulgi.kim/2018/05/ethereum-secure-tree.html)

### Data structure 시리즈
[Data structure in Ethereum. Episode 4: Diving into examples.](https://medium.com/coinmonks/data-structure-in-ethereum-episode-4-diving-by-examples-f6a4cbd8c329)
[Radix trie and Merkle trie](https://medium.com/@phansnt/data-structure-in-ethereum-episode-2-radix-trie-and-merkle-trie-d941d0bfd69a)
[Data structure in Ethereum. Episode 1: Recursive Length Prefix (RLP) Encoding/Decoding.](https://medium.com/coinmonks/data-structure-in-ethereum-episode-1-recursive-length-prefix-rlp-encoding-decoding-d1016832f919)
[Data structure in Ethereum. Episode 1+: Compact (Hex-prefix) encoding.](https://medium.com/coinmonks/data-structure-in-ethereum-episode-1-compact-hex-prefix-encoding-12558ae02791)
[Data structure in Ethereum. Episode 3: Patricia trie.](https://medium.com/coinmonks/data-structure-in-ethereum-episode-3-patricia-trie-b7b0ccddd32f)
