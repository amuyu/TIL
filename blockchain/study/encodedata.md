# signature
```js
var fullName = functionName + '(' + types.join() + ')'  
var signature = CryptoJS.SHA3(fullName,{outputLength:256}).toString(CryptoJS.enc.Hex).slice(0, 8)  
```

# encode
// web3/lib/solidity/formatters.js
```js

```
// web3/lib/solidity/param.js
```js
/**
 * This method should be called to encode param
 *
 * @method encode
 * @returns {String}
 */
SolidityParam.prototype.encode = function () {
    return this.staticPart() + this.dynamicPart();
};
```

# rawEncode
```js

```

# 참고
[ethereumjs-abi](https://github.com/ethereumjs/ethereumjs-abi)
[encode설명](https://ethereum.stackexchange.com/a/32330)
[abi변환사이트](https://abi.hashex.org)
