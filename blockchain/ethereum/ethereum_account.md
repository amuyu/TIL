# account
There are two types of accounts: externally owned accounts (EOAs) and contract accounts.
Normal or externally controlled accounts and
contracts, i.e., snippets of code, think a class.

EOA 주소는 공개키에 의해 결정되고 Contract 계정에 의한 주소는 contract 가 만들어질 때 결정된다.
모든 계정은 256비트 문자와 256비트 문자를 매핑하는 storage를 가지고 있고 이더리움 잔액을 가지고 있다.

account는 머클 패트리시아 트리에 저장된다.

## source
```
// package: accounts, file: accounts.go
type Account struct {
  Address common.Address  `json:"address"`
  URL     URL             `json:"url"`
}
// package: Common, file: Types.go
const AddressLength = 20
type Address [AddressLength]byte
// package: core/state, file: state_object.go
type Account struct {
  Nonce     uint64        // 트랜잭션 수
  balance   *big.int      // 이더 잔고
  Root      common.hash   // 머클 패트리시아 트리의 루트 노드
  CodeHash  []byte        // 스마트 컨트랙트 바이트 코드
}
```


# address
이더리움은 공용키 암호화를 사용  ECDSA (Elliptic Curve Digital Signature Algorithm)
```js
var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider('https://ropsten.infura.io/'));
var util = require('ethereumjs-util');
var tx = require('ethereumjs-tx');

var privateKey = '0xc0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0de';
var publicKey = util.bufferToHex (util.privateToPublic (privateKey));
```
개인키와 관련해서 이더리움 주소는 공개 키 의 SHA3-256 (Keccak) 해시의 마지막 160 비트입니다 .
pirvateKey ----> (ECDSA) ----> publicKey ----> (sha3-256) ----> address

# state
account 들이 모인 것을 ethereum 에서는 state 라고 하고 stateObject 구조체로 표현한다.
어카운트에 접근하여 상태를 변경하려면 stateObject를 통해 접근한 후 상태를 변경할 수 있다.
```
type stateObject struct {
	address  common.Address
	addrHash common.Hash // hash of ethereum address of the account
	data     Account     // account
	db       *StateDB    // 상태를 저장할 DBMS에 대한 포인터

  trie Trie // trie 저장소
	code Code // contract bytecode
}
```

# property
nonce : transactoin count
balance
storageRoot
codeHash : 값이 있으면 contract account
근데 normal account 에 code hash 가 비어있지는 않음 ethUtil.sha3() 호출한 값이 들어가 있음

# getBalance
```go
// blockNumber 가 없으면 'latest'
// state 에서 balance 를 조회
// pakcage : ethapi
func (s *PublicBlockChainAPI) GetBalance(ctx context.Context, address common.Address, blockNr rpc.BlockNumber) (*big.Int, error) {
	state, _, err := s.b.StateAndHeaderByNumber(ctx, blockNr)
	if state == nil || err != nil {
		return nil, err
	}
	b := state.GetBalance(address)
	return b, state.Error()
}

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

// package : state
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

// package : state
func (self *StateDB) GetBalance(addr common.Address) *big.Int {
	stateObject := self.getStateObject(addr)
	if stateObject != nil {
		return stateObject.Balance()
	}
	return common.Big0
}

// package : state
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

# 참고
```go
type Header struct {
	ParentHash  common.Hash    
	UncleHash   common.Hash    
	Coinbase    common.Address
	Root        common.Hash    
	TxHash      common.Hash    
	ReceiptHash common.Hash    
	Bloom       Bloom          
	Difficulty  *big.Int       
	Number      *big.Int       
	GasLimit    uint64         
	GasUsed     uint64         
	Time        *big.Int       
	Extra       []byte         
	MixDigest   common.Hash    
	Nonce       BlockNonce     
}
```
[blockChain](https://i.stack.imgur.com/QpcFh.png)



## 참고
[Inside ethereum transaction](https://medium.com/@codetractio/inside-an-ethereum-transaction-fa94ffca912f)
[account management](http://ethdocs.org/en/latest/account-management.html)
[account-unique](https://ethereum.stackexchange.com/questions/4299/account-uniqueness-guaranteed)
