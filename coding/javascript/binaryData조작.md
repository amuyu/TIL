# String.charCodeAt()
주어진 인덱스에 대한 UTF-16(유니코드)를 나타내는 0~65535 사이의 정수를 반환 (Ascii code)


# buffer 인코딩
Buffer 클래스에서 기본이 utf-8 인코딩
type 에 따라 변환하는 방법이 다르므로 주의한다.
## string
```js
var buf = new Buffer('Hello World!')  // utf8
var buf = new Buffer('8b76fde713ce', 'base64')  // base64 인코딩

const buf = Buffer.from('Hello World!')
```
## int
int 를 hex 로 변환하고 buffer 로 변환
```js
var hex = i.toString(16)
var buf = Buffer.from(hex, 'hex')
```

# buffer 디코딩
```js
var str = buf.toString();
```

# padToEven
even length 로 변환
```js
function padToEven (a) {
  if (a.length % 2) a = '0' + a
  return a
}
```

# toBuffer
// rlp.toBuffer
```js
function toBuffer (v) {
  if (!Buffer.isBuffer(v)) {
    if (typeof v === 'string') {
      if (isHexPrefixed(v)) {
        v = Buffer.from(padToEven(stripHexPrefix(v)), 'hex')
      } else {
        v = Buffer.from(v)
      }
    } else if (typeof v === 'number') {
      if (!v) {
        v = Buffer.from([])
      } else {
        v = intToBuffer(v)
      }
    } else if (v === null || v === undefined) {
      v = Buffer.from([])
    } else if (v.toArray) {
      // converts a BN to a Buffer
      v = Buffer.from(v.toArray())
    } else {
      throw new Error('invalid type')
    }
  }
  return v
}
```


## 참고
[바이너리 데이터의 조작, 인코딩, 디코딩을 위한 버퍼 활용](https://gist.github.com/bynaki/559196064241f5dd776f)
