# 이더리움 단일 상태 모델
트랜잭션은 어카운트의 상태 변화를 가져온다.

## 이더리움에서의 거래, 상태 전이
APPLY(S, TX)->S'
S:현재상태, APPLY:상태변이함수, S':변이된 상태

이더리움에서는 state 를 이해하는 것이 중요하다.
이더리움에서 모든 기본 단위는 account 다. 모든 account는 중복되지 않는 식별자로
특정 주소를 부여받고 잔액이나 트랜잭션, 스마트 컨트랙트의 중간 코드처럼 필요한
데이터를 저장하기 위한 일련의 저장 공간들을 갖는다.

이더리움의 전체 상태는 전체 어카운트의 상태를 말한다.
이더리움에 새로운 블록이 추가되면 전체 어카운트의 상태도 변한다.
모든 어카운트의 상태 정보는 블록과 블록 내에 연결된 머클 패트리시아 트리로
저장되고 관리된다.


## state trie pruning
모든 block state root를 가지고 있다.
state root : system의 전체 state 를 가지고 있는 merkle tree 의 root hash,

목적은 동기화, 히스토리 트랜잭션을 처리하지 않고 블록체인과 신속하게 동기화할 수 있다.
노드로부터 나머지 트리를 받아서 (hashlookup, wire protocol message)
모든 해쉬들이 일치하는지 확인한다.

1. 많은 block header 를 다운로드한다.
2. 가장 긴 chain 끝에 있는 헤더를 계산한다. 헤더에서 100block으로 돌아가라 안전하게
3. 100개의 state root에서 state tree 다운로드한다.
4. 계속 진행한다.

light client 에서는 장점이다. any account 의 balance와 status를 즉시 계산할 수 있다.

그러나 state tree는 단점이 있다. 모든 data를 저장하기 위해서 디스크 사용량이 어마어마하게 증가한다.

block 에서 data는 로그적으로 커다란 수의 노드에서 두번 저장되어야 한다.?
block chain의 실제 용량이 1.3giga 여도 디스크에 저장되는 사이즈는 10~40giga 이다.

header를 간단하게 구현한다. 동기화는 하드 디스크 사용량을 0으로 하고? 1~2 개월마다 다시 동기화한다.
다른 방법은 state tree pruning 이다. 참조 카운팅을 해서 트리의 노드를 추적한다. 트리에서 빠져나가면
노드를 ‘death row’ x blocks 동안 노드가 다시 사용되지 않는다면, 데이터베이스에서 제거합니다?
5000 블락 이상된 히스토리는 저장하지 않는다.



1. N 블록을 처리할 때, 모든 노드들을 추적한다. 참조가 0인,
노드들의 hash를 “death row” 에 넣는다. N+X 블록에서 삭제할 수 있다.
2. death row에 있다 상태가 변경되면 c참조 count를 증가 시킨다.
이 노드는 M블락에서 다시 “death row” 로 이동하게 될 것이다.
3. N+X 블락을 처리하게 되면 block N에 대해서 여전히 deletion mark되어 있으면
삭제한다.
4. block 을 revert 하기 위해서 journal 에 참조수의 변화를 저장한다.
5. 블록을 처리할 때 N-X 블락에서 journal을 삭제한다.


# intro
Ethereum is a transaction-based “state” machine

## state trie -  the one and only
There is one global state trie, and it updates over time.
In it, a path is always: sha3(ethereumAddress) and
a value is always: rlp(ethereumAccount).
More specifically an ethereum account is a 4 item array of [nonce,balance,storageRoot,codeHash].
At this point it's worth noting that this storageRoot is the root of another patricia trie:


## State trie
The state trie contains a key and value pair for every account which exists on the Ethereum network.

## Storage trie  -  where the contract data lives
A storage trie is where all of the contract data lives.
Each Ethereum account has its own storage trie.

## Transaction trie - one per block
path : rlp(transactionIndex)
value : transaction object (nonce, gasPrice, gasLimit, value)

# 참고
[state-tree-pruning](https://blog.ethereum.org/2015/06/26/state-tree-pruning/)
[Merkling in Ethereum](https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/)
[Diving into Ethereum’s world state](https://medium.com/cybermiles/diving-into-ethereums-world-state-c893102030ed)
  - trie 의 구조에 대해서 잘 설명함, 어떤 데이터들이 있는지
[patricia tree](https://github.com/ethereum/wiki/wiki/Patricia-Tree)
