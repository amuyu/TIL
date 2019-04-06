# Trie
```go
type Trie struct {
	db           *Database
	root         node
	originalRoot common.Hash

	// Cache generation values.
	// cachegen increases by one with each commit operation.
	// new nodes are tagged with the current generation and unloaded
	// when their generation is older than than cachegen-cachelimit.
	cachegen, cachelimit uint16
}
```

# cachedTrie


# trie 와 leveldb
this.putRaw(hash, serialized, cb)
## TrieNode
type, key, value
## TryeUpdate
```go
k = keybtyesToHex(key)
// insert(n node, prefix, key []byte, value node)
t.insert(root, nil, k, valueNode(value))
```

# transaction 처리 부터 마이닝
handler(msg) ,worker 에서 처리,
miner tx 처리, consensus 합의, chain db 저장
miner/worker.commintNewWork()
commitTransaction()
commitUncles() : 엉클 블록을 포함시킨다
Finalize() : 신규 블록을 만든다. (합의엔진)
consensus.Finalize -> stated.IntermediateRoot

# 블록체인 생성
worker.wait()
책에는 chain.WriteBlockAndState (WriteBlockWithState)
여기에서 state.Commit을 함 (work에 state 가 저장되어있음)

# trie.tryGet
resolveHash(rootHash) > trie.db.Node(hash) > decode
* 여기서 decode 는 node struct로 decode 됨
* 최종적으로 valueNode 에는 data 만들어가 있음
node 종류에 따라 처리하고 다음 node 정보를 가져옴
ethereum trie 는 memory 들고 있음 동기화 시점에서 db에 접근?

db 동기화 이후 trie setting



# node 가져오기
fullNode, shortNode, hashNode, valueNode

# understanding the ethereum trie
trie 는 block 검증 외에는 필요하지 않는다? (그럴 수도 있지만 좀..)
transaction list 는 트리로 저장된고 직렬화된다.(다른 클라이언트에 보내기 위해)
클라이언트는 트랜잭션 목록 트리를 재구성한다.
RLP (recursive length prefix encoding) : 트리의 모든 entry를 인코딩하는데 사용

radix를 merkle 로 개선
각 노드는 hash로 되어있고 leveldb 에서 찾는다.

hex-prefix(HP) encoding
leaf 와 extension, odd, even 구분
terminate flag를 사용해 키가 참조하는 유형을 나타낸다.
flag가 켜져있으면 leaf 아니면 노드 조회
4 bit binary number, (nibble)

key, node



# trie func
## put
```js
/**
 * Stores a given `value` at the given `key`
 * @method put
 * @param {Buffer|String} key
 * @param {Buffer|String} Value
 * @param {Function} cb A callback `Function` which is given the argument `err` - for errors that may have occured
 */
Trie.put(key, value, cb)
```

## findPath
```go
// findPath
Trie.prototype.findPath = function (targetKey, cb) {
  var self = this
  var root = self.root
  var stack = []
  targetKey = TrieNode.stringToNibbles(targetKey)

  this._walkTrie(root, processNode, cb)

  function processNode (nodeRef, node, keyProgress, walkController) {
    var nodeKey = node.key || []
    var keyRemainder = targetKey.slice(matchingNibbleLength(keyProgress, targetKey))
    var matchingLen = matchingNibbleLength(keyRemainder, nodeKey)

    stack.push(node)

    if (node.type === 'branch') {
      if (keyRemainder.length === 0) {
        walkController.return(null, node, [], stack)
      // we exhausted the key without finding a node
      } else {
        var branchIndex = keyRemainder[0]
        var branchNode = node.getValue(branchIndex)
        if (!branchNode) {
          // there are no more nodes to find and we didn't find the key
          walkController.return(null, null, keyRemainder, stack)
        } else {
          // node found, continuing search
          walkController.only(branchIndex)
        }
      }
    } else if (node.type === 'leaf') {
      if (doKeysMatch(keyRemainder, nodeKey)) {
        // keys match, return node with empty key
        walkController.return(null, node, [], stack)
      } else {
        // reached leaf but keys dont match
        walkController.return(null, null, keyRemainder, stack)
      }
    } else if (node.type === 'extention') {
      if (matchingLen !== nodeKey.length) {
        // keys dont match, fail
        walkController.return(null, null, keyRemainder, stack)
      } else {
        // keys match, continue search
        walkController.next()
      }
    }
  }
}
// walkTrie
Trie.prototype._walkTrie = function (root, onNode, onDone) {
  var self = this
  root = root || self.root
  onDone = onDone || function () {}
  var aborted = false
  var returnValues = []

  if (root.toString('hex') === ethUtil.SHA3_RLP.toString('hex')) {
    return onDone()
  }

  self._lookupNode(root, function (node) {
    processNode(root, node, null, function (err) {
      if (err) {
        return onDone(err)
      }
      onDone.apply(null, returnValues)
    })
  })

  // the maximum pool size should be high enough to utilise the parallelizability of reading nodes from disk and
  // low enough to utilize the prioritisation of node lookup.
  var maxPoolSize = 500
  var taskExecutor = new PrioritizedTaskExecutor(maxPoolSize)

  function processNode (nodeRef, node, key, cb) {
    if (!node) return cb()
    if (aborted) return cb()
    var stopped = false
    key = key || []

    var walkController = {
      stop: function () {
        stopped = true
        cb()
      },
      // end all traversal and return values to the onDone cb
      return: function () {
        aborted = true
        returnValues = arguments
        cb()
      },
      next: function () {
        if (aborted) {
          return cb()
        }
        if (stopped) {
          return cb()
        }
        var children = node.getChildren()
        async.forEachOf(children, function (childData, index, cb) {
          var keyExtension = childData[0]
          var childRef = childData[1]
          var childKey = key.concat(keyExtension)
          var priority = childKey.length
          taskExecutor.execute(priority, function (taskCallback) {
            self._lookupNode(childRef, function (childNode) {
              taskCallback()
              processNode(childRef, childNode, childKey, cb)
            })
          })
        }, cb)
      },
      only: function (childIndex) {
        var childRef = node.getValue(childIndex)
        var childKey = key.slice()
        childKey.push(childIndex)
        var priority = childKey.length
        taskExecutor.execute(priority, function (taskCallback) {
          self._lookupNode(childRef, function (childNode) {
            taskCallback()
            processNode(childRef, childNode, childKey, cb)
          })
        })
      }
    }
    onNode(nodeRef, node, key, walkController)
  }
}
```


# ethereum_trie
trie, statedb, database
//
```go
// package state, database.go
type Trie interface {
	TryGet(key []byte) ([]byte, error)
	TryUpdate(key, value []byte) error
	TryDelete(key []byte) error
	Commit(onleaf trie.LeafCallback) (common.Hash, error)
	Hash() common.Hash
	NodeIterator(startKey []byte) trie.NodeIterator
	GetKey([]byte) []byte // TODO(fjl): remove this when SecureTrie is removed
	Prove(key []byte, fromLevel uint, proofDb ethdb.Putter) error
}
```
## Commit 은 언제
block이 confirm 되면 commit 을 해서 db.write 해야할듯

stateObject.CommitTrie > updateTrie > Commit
CommitTrie 호출이 Statedb.commit에서 호출
sync할 때, commit을 호출하는듯??
db, write 하고 commit 호출

stateDB.AddBalance > stateObject.AddBalance > stateObject.SetBalance
self.db.journal.append
journal.go (dirty 관리)

updateStateObject?

## Node
```go
type (
	fullNode struct {
		Children [17]node // Actual trie node data to encode/decode (needs custom encoder)
		flags    nodeFlag
	}
	shortNode struct {
		Key   []byte
		Val   node
		flags nodeFlag
	}
	hashNode  []byte
	valueNode []byte
)
```
