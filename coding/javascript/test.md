# javascript test framework
Jasmine, Mocha, Jest, NodeUnit 등등

# Tape
Unit test 를 위한 최소한의 기능 제공을 목표로 한다
ape provides both a basic test runner and a core set of TDD assertions


# install
```sh
npm install tape -g
npm install tape —save-dev
```

# basic test format
기본 구조는 다음과 같다.
```js
var test = require( 'tape' ) ;
test( '<TEST NAME>', function( assert ) {
  assert.<FUNCTION>( <PARAMETERS>, '<STATEMENT THAT WILL BE TRUE IF TEST PASSES>' ) ;
  assert.end() ;
} ) ;
```
예를 들어 작성하면 다음과 같다.
```js
var test = require( 'tape' ) ;
test( 'My first test', function( assert ) {
  assert.equal( 1, 2, 'Numbers 1 and 2 are the same' ) ;
  assert.end() ;
} ) ;
```
작성 후, node 명령으로 실행
```js
node test.js
```

# Test Plans
얼마나 테스트할지 정하거나 종료 시점에 대해 호출할 수 있다.
asser.plan method:
```js
test( 'My second test', function( assert ) {
  assert.plan( 2 ) ; // Specifies that we will be executing exactly two tests
  assert.equal( 1 + 1, 2, '1 + 1 = 2' ) ;
  assert.equal( 2 + 2, 4, '2 + 2 = 4' ) ;
} ) ;
```
assert.end method:
```js
test( 'My third test', function( assert ) {
  assert.equal( 1 + 1, 2, '1 + 1 = 2' ) ;
  assert.equal( 2 + 2, 4, '2 + 2 = 4' ) ;
  assert.end() ;
} ) ;
```

# Assertions
```js
assert.true( value, msg ) / assert.false( value, msg )

assert.equal( actual, expected, msg ) / assert.notEqual( actual, expected, msg )

assert.deepEqual( actual, expected, msg ) / assert.notDeepEqual( actual, expected, msg )

assert.throws( function, [expected], msg ) / assert.doesNotThrow( function, expected, msg )
```

# package.json
```js
...
"scripts": {
"test": "node_modules/.bin/tape test/*.js"
},
...
```



# ref
[테스트를 잘 작성하는 방법](http://shiren.github.io/2015-09-24-유닛테스트가-해야할-5가지-답변-테스트를-잘-작성하는-방법/)
[javascript mocha 로 테스트 시작하기](http://programmingsummaries.tistory.com/383)
[Unit Testing JavaScript Applications With Tape](https://medium.com/@andy.neale/unit-testing-javascript-applications-with-tape-1d4f5d5fc825)
