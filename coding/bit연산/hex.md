
# hextoBit
// bn.js
// parseInt 를 사용하다가 변경함
```js
function parseHex (str, start, end) {
  console.log('========> start')
  var r = 0;
  var len = Math.min(str.length, end);
  var z = 0;
  for (var i = start; i < len; i++) {
    var c = str.charCodeAt(i) - 48;
    console.log('c:', c)
    r <<= 4;

    var b;

    // 'a' - 'f'
    if (c >= 49 && c <= 54) {
      b = c - 49 + 0xa;

    // 'A' - 'F'
    } else if (c >= 17 && c <= 22) {
      b = c - 17 + 0xa;

    // '0' - '9'
    } else {
      b = c;
    }

    r |= b;
    z |= b;
    console.log('r:', r)
    console.log('r:', r.toString(2))
    console.log('r:', r.toString(16))
  }

  console.log('z:', z.toString(2))
  console.log('z:', (z & 0xf0))
  assert(!(z & 0xf0), 'Invalid character in ' + str);
  console.log('========> end')
  return r;
}
```
// parseInt 사용
```js
parseInt(n.toString(), 16).toString(2);
```
