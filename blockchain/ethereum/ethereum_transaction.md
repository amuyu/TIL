# Recursive Length Prefix (RLP)
이더리움 데이터를 직렬화하고 재구성할 수 있도록 도와주는 인코딩/디코딩 알고리즘
RLP 의 목적은 구조를 인코딩하는 것, 특정 데이터 유형(string, float)에서 higher-order protocols 인코딩한다.
임의로 중첩된 2진 데이터 배열을 인코딩함
[길이 + item(string, array)]
## 인코딩 타입
인코딩하는 item 정의
- A string (ie. byte array) is an item
- A list of items is an item
number 의 경우, hex 로 변환해서 string type 으로 처리한다
## 방법
- [0x00, 0x7f] 범위 내에 있는 단일 바이트는 고유 RLP 인코딩
- 0~55 bytes(string type), (0x80 + string.length)한 single byte와 string bytes. first byte range [0x80, 0xb7]
- 55 > bytes(string type), (0xb7 + byte(string.length).length)한 single byte와 string.length 와 string bytes. first byte range [0xb8, 0xbf]
- 0~55 bytes(array type), (0xc0 + list.length)한 single byte와 연결된 items(RLP encoding 된 items). first byte range [0xc0, 0xf7]
- 55 > bytes(array type), (0xf7 + byte(array.length).length)한 single byte 와 연결된 items. first byte range [0xf8, 0xff]
## example
The string "dog" = [ 0x83, 'd', 'o', 'g' ]
The list [ "cat", "dog" ] = [ 0xc8, 0x83, 'c', 'a', 't', 0x83, 'd', 'o', 'g' ]
The empty string ('null') = [ 0x80 ]
The empty list = [ 0xc0 ]
The integer 0 = [ 0x80 ]
The encoded integer 0 ('\x00') = [ 0x00 ]
The encoded integer 15 ('\x0f') = [ 0x0f ]
The encoded integer 1024 ('\x04\x00') = [ 0x82, 0x04, 0x00 ]
The set theoretical representation of three, [ [], [[]], [ [], [[]] ] ] = [ 0xc7, 0xc0, 0xc1, 0xc0, 0xc3, 0xc0, 0xc1, 0xc0 ]
The string "Lorem ipsum dolor sit amet, consectetur adipisicing elit" = [ 0xb8, 0x38, 'L', 'o', 'r', 'e', 'm', ' ', ... , 'e', 'l', 'i', 't' ]

# transaction
## 파라미터
```js
/
*
 * @property {Buffer} raw The raw rlp encoded transaction
 * @param {Buffer} data.nonce nonce number
 * @param {Buffer} data.gasLimit transaction gas limit
 * @param {Buffer} data.gasPrice transaction gas price
 * @param {Buffer} data.to to the to address
 * @param {Buffer} data.value the amount of ether sent
 * @param {Buffer} data.data this will contain the data of the message or the init of a contract
 * @param {Buffer} data.v EC recovery ID
 * @param {Buffer} data.r EC signature parameter
 * @param {Buffer} data.s EC signature parameter
 * @param {Number} data.chainId EIP 155 chainId - mainnet: 1, ropsten: 3
 * */
```
## tx 만들어지는 과정
rawTx ----> (rlp+hash) ----> 0x8b69a0ca303305a92d8d028704d65e4942b7ccc9a99917c8c9e940c9d57a9662 ----> (privateKey sign) ----> data
```js
var rawTx = {
    nonce: web3.toHex(transactionCount),
    gasPrice: web3.toHex(gasPrice),
    gasLimit: web3.toHex(gasLimit),
    // gasLimit: web3.toHex(30000000),
    to: toAddress,
    from: ownerAddress,
    data: data
};

var p = new Buffer('c0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0dec0de', 'hex');
var transaction = new tx(rawTx);
transaction.sign(p);
```

# contract method call
transaction 의 input 파라미터를 보면 처음 32bit 는  함수의 해시의 첫 번째 32 비트 이다.
```
web3.eth.getTransaction('0xaf4a217f6cc6f8c79530203372f3fbec160da83d1abe048625a390ba1705dd57').input
//
0xa9059cbb000000000000000000000000adeade867ea91533879d083dd47ea81f0eee3a37e000000000000000000000000000000000000000000000000d02ab486cedbffff
console.log (web3.sha3 ( 'transfer (address, uint256)'));
// 0xa9059cbb2ab09eb219583f4a59a5d0623ade346d962bcd4e46b11da047c9049b
// a9059cbb first 32bit
```

# encodeParam
## string encode
```js
abi.rawEncode(["string"],["Hello world"])
// raw Encode inner method...
headLength = 32
output.push(encodeSingle('uint256', headLength))
var cur = encodeSingle(type, value)
output.push(cur)

// encodeSingle inner method..
// type : string,
encodeSingle('bytes', new Buffer(arg, 'utf8'))
// bytes
arg = new Buffer(arg)
ret = Buffer.concat([ encodeSingle('uint256', arg.length), arg ])

// result
32bit : headLength, 0000000000000000000000000000000000000000000000000000000000000020
32bit : string length, 000000000000000000000000000000000000000000000000000000000000000b
32bit : string, 48656c6c6f20776f726c64000000000000000000000000000000000000000000
```

# help
be : big-endian
le : little-endian

# 참고
[Inside ethereum transaction](https://medium.com/@codetractio/inside-an-ethereum-transaction-fa94ffca912f)
[Data structure in Ethereum. Episode 1: Recursive Length Prefix (RLP) Encoding/Decoding](https://medium.com/@phansnt/data-structure-in-ethereum-episode-1-recursive-length-prefix-rlp-encoding-decoding-d1016832f919)
  - RLP 설명과 함께 몇 가지 의문점을 던진다
[difference tx and rawtx](https://ethereum.stackexchange.com/questions/6905/difference-between-transactions-and-raw-transactions-in-web3-js?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
[whatisatx](http://ethdocs.org/en/latest/contracts-and-transactions/account-types-gas-and-transactions.html#what-is-a-transaction)
[sending_eather](https://ethereum.gitbooks.io/frontier-guide/content/sending_ether.html)
[ethereumjs-abi](https://github.com/ethereumjs/ethereumjs-abi)
