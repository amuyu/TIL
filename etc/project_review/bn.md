# bn
???

# \_parseHex
hex : 0~f
Math.ceil == 올림
왜 6으로 나누지? 6개씩 나눠서 돌려본다, 작은 수부터 돌려본다 24bit == 6글자
words?
// Create possibly bigger array to ensure that it fits the number
// Scan 24-bit chunks and add them to the number
// NOTE: `0x3fffff` is intentional here, 26bits max shift + 24bit hex limb (6글자)
26bits max == 0x7FFFFFF , >> 5 == 0x3fffff
0xFFFFFF = 24bits max?
0x1FFFFFF = 25bits max
0x3FFFFFF = 26bits max
0x3FFFFF = 22bits max

## whatis 26bit limb??
// https://github.com/indutny/bn.js/issues/89
The reason for using 26 bits is that we have only 52bits of precision in javascript.
// 초기 parseHex
```js
BN.prototype._parseHex = function parseHex(number, start) {
  // Scan 3-byte chunks
  this.length = Math.ceil((number.length - start) / 7);
  this.words = new Array(this.length);
  for (var i = number.length - 6, j = 0; i >= start; i -= 6, j++)
    this.words[j] = parseInt(number.slice(i, i + 6), 16);
  if (i + 6 !== start)
    this.words[j++] = parseInt(number.slice(start, i + 6), 16);
  this.length = this.words.length;
};
```
// 0.50 wip 26bit limbs
```js
BN.prototype._parseHex = function parseHex(number, start) {
  // Create possibly bigger array to ensure that it fits the number
  this.length = Math.ceil((number.length - start) / 6);
  this.words = new Array(this.length);
  for (var i = 0; i < this.words.length; i++)
    this.words[i] = 0;

  // Scan 24-bit chunks and add them to the number
  var off = 0;
  for (var i = number.length - 6, j = 0; i >= start; i -= 6) {
    var w = parseInt(number.slice(i, i + 6), 16);
    this.words[j] |= (w << off) & 0x3ffffff;
    this.words[j + 1] |= w >> (26 - off) & 0x3fffff;
    off += 24;
    if (off >= 26) {
      off -= 26;
      j++;
    }
  }
  if (i + 6 !== start) {
    var w = parseInt(number.slice(start, i + 6), 16);
    this.words[j] |= (w << off) & 0x3ffffff;
    this.words[j + 1] |= w >> (26 - off) & 0x3fffff;
  }
  this.strip();
};
```
// var a = new BN('aeadeaa',16)
// [ 48946858, 2 ]
// [ 15392426, 10 ]


## parseHex
hex string 을 bit 로 변환 (4bit)
// 'a' - 'f'
b = c - 49 + 0xa; 16진수의 캐릭터를 변환
assert(!(z & 0xf0), 'Invalid character in ' + str); // 4bit가 넘으면 exception 발생

# 궁금한거?
BN 은 숫자인줄 알았는데 문자를 다룬다?
```js
var a = new BN('dead', 16);
console.log(a.toString());  // 57005
console.log(a.toString(16));  // dead
```

# 참고
[github/bn](https://github.com/indutny/bn.js/)
