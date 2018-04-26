# trie
디지털 tree를 나타내는 용어
(root, path, node, leaf)

## Radix trie
Rasix tire is used to optimize for searching
trie 의 path는 ascii 문자이고 값을 찾는데 사용한다.

```js
[i0, i1 ... in, value]  // i0 ... in is the symbols of the alphabet
```

## Merkle trie
Merkle trie is used to authenticate data
데이터를 인증하기 위해서 결정적 암호화 해쉬를 만들어서 사용한다
전체 데이터는 leaf 에 저장되고 parent node 는 각가의 hash 로 구성된다

leaf 의 value 를 변경하면 그 위의 path 들의 node들은 다 변경되어야 한다
root의 변경없이 데이터 조작은 불가능하다

## ethereum 의 Merkle 증명
transaction, receipts, state tree 가 있다.

## 참고
[Radix trie and Merkle trie](https://medium.com/@phansnt/data-structure-in-ethereum-episode-2-radix-trie-and-merkle-trie-d941d0bfd69a)
[ethereum wiki](https://github.com/ethereum/wiki/wiki/Patricia-Tree)
[github/maerkle-patricia-tree](https://github.com/ethereumjs/merkle-patricia-tree)
[Merkling in Ethereum](https://blog.ethereum.org/2015/11/15/merkling-in-ethereum/)
[merkle tree 란 뭔가요](https://steemit.com/kr/@jsralph/merkle-trees)
[Exploring Ethereum's state trie with Node.js](https://wanderer.github.io/ethereum/nodejs/code/2014/05/21/using-ethereums-tries-with-node/)
