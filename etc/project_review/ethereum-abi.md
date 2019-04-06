# parseTypeArray
array 파싱 array type에는 '[]' 들어가므로 문자열로 파싱
```js
function parseTypeArray (type) {
  var tmp = type.match(/(.*)\[(.*?)\]$/)
  if (tmp) {
    return tmp[2] === '' ? 'dynamic' : parseInt(tmp[2], 10)
  }
  return null
}
```

# isArray
array 인지 확인, ']' 가 마지막에 위치하는지 확인

# isDynamic
string, bytes, dynamic
```js
// Is a type dynamic?
function isDynamic (type) {
  // FIXME: handle all types? I don't think anything is missing now
  return (type === 'string') || (type === 'bytes') || (parseTypeArray(type) === 'dynamic')
}
```

# elementaryName
type 이름 변환
- int[x] > int256[x]
- int > int256
- uint[x] > uint256[x]
- uint > uint256
- fixed[x] > fixed128x128[x]
- ufixed[x] > ufixed128x128[x]
- fixed > ufixed128x128
- etc > etc
* fixed : Fixed Point Numbers ?? decimal 을 사용하는 변수인가?

# encodeSingle
address > uint160
bool > uint8 (1:0)
string > bytes
static array > (type, arg)
dynaic array > (uint256, array.size) + (type, arg)
bytes > (uint256, bytes.length) + bytes + zero(32bit 채우기용)
uint > BN > BN.toArrayLike(32)
int > BN > BN.toTwos(256) > BN.toArrayLike(32)


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
z
// result
64bit : headLength, 0000000000000000000000000000000000000000000000000000000000000020
64bit : string length, 000000000000000000000000000000000000000000000000000000000000000b
64bit : string, 48656c6c6f20776f726c64000000000000000000000000000000000000000000
```


## 참고
[ethereum-abi](https://github.com/ethereumjs/ethereumjs-abi)
