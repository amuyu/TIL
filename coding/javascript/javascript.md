
# ECMAScript
표준 ECMA 기술 규격에 정의된 표준화된 스크립트 프로그래밍 언어

# 자바스크립트와의 차이점
자바스크립트와 J스크립트는 모두 ECMA스크립트와의 호환을 목표로 하면서, ECMA 규격에 포함되지 않는 확장 기능을 제공한다.

# Overview
javascript 의 구성요소

## Type
- Number
double-precision 64-bit format IEEE 754 values
parseInt('123', 10); // 123
parseInt('010'); // 8 0 으로 시작하면 octal
+ '42'; // 41 '+ operator 를 사용하면 number 로 변경'
NaN + 5; // NaN 'NaN으로 연산하면 결과는 NaN'
1 / 0; // Infinity 'special values', isFinite() 로 확인 가능
- String
sequences of Unicode characters `UTF-16`
'hello'.length; // 5 'string 의 길이'
'hello'.charAt(0);  // "h"
'hello, world'.replace('hello', 'goodbye'); // "goodbye, world"
'hello'.toUpperCase(); // "HELLO"
- Boolean
- Symbol (new in ES2015)
- Object
  - Function
  - Array
  - Date
  - RegExp
- null
- undefined
javascript 는 null과 undefined 를 구별한다.

## Variables
let, const, var
- let
let 은 block 수준의 변수
- const
변할 수 없는 변수
- var
var 는 일반적인 선언 키워드, 함수 내에서 이용이 가능하다
javascript 는 function 에 scope 가 있다.

## Operators
+, -, * , /, %, += , -=, ++, --
'hello' + ' world'; // "hello world"
'3' + 4 + 5;  // "345"
 3 + 4 + '5'; // "75" 첫 번째 값 따라간다.
비교 연산자 : <, >, <=, >=
The double-equals operator performs type coercion
123 == '123'; // true
1 == true; // true
To avoid type coercion, use the triple-equals operator
123 === '123'; // false
1 === true;    // false

## Control structures
javascript 는 c와 control structures 가 비슷하다.
if and else
while, do-while
for...of
```js
for (let value of array) {
  // do something with value
}
```
for...in
```js
for (let property in object) {
  // do something with object property
}
```
&& and || and a ternary operator
switch

## Objects
javascript 객체는 이름-값 쌍의 간단한 컬렉션으로 생각할 수 있다.
- Dictionaries in Python.
- Hashes in Perl and Ruby.
- Hash tables in C and C++.
- HashMaps in Java.
- Associative arrays in PHP.

create an empty object
```js
var obj = new Object();
var obj = {}; // literal syntax
```

Object literal syntax
```js
var obj = {
  name: 'Carrot',
  for: 'Max', // 'for' is a reserved word, use '_for' instead.
  details: {
    color: 'orange',
    size: 12
  }
};
```

Attribute access
```js
obj.details.color; // orange
obj['details']['size']; // 12
```

create an object prototype
```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// Define an object
var you = new Person('You', 24);
// We are creating a new person named "You" aged 24.
```

objects properties access
```js
//dot notation
obj.name = 'Simon';
var name = obj.name;

// bracket notation
obj['name'] = 'Simon';
var name = obj['name'];
// can use a variable to define a key
var user = prompt('what is your key?')
obj[user] = prompt('what is its value?')
```

bracket 을 사용하면 예약어를 사용할 수 있다.
```js
obj.for = 'Simon'; // Syntax error, because 'for' is a reserved word
obj['for'] = 'Simon'; // works fine
```

## Arrays
javascript 에서 arrays 는 특별한 type 이다.

creating arrays
```js
var a = new Array();
a[0] = 'dog';
a[1] = 'cat';
a[2] = 'hen';
a.length; // 3

var a = ['dog', 'cat', 'hen'];
a.length; // 3
```

`forEach()`
```js
['dog', 'cat', 'hen'].forEach(function(currentValue, index, array) {
  // Do something with currentValue or array[index]
});
```

`push`
```js
a.push(item);
```

Methods
a.toString() : Returns a string with the toString() of each element separated by commas.
a.toLocaleString() :Returns a string with the toLocaleString() of each element separated by commas.
a.concat(item1[, item2[, ...[, itemN]]]): Returns a new array with the items added on to it.
a.join(sep) :	Converts the array to a string — with values delimited by the sep param
a.pop()	: Removes and returns the last item.
a.push(item1, ..., itemN)	: Appends items to the end of the array.
a.reverse()	: Reverses the array.
a.shift()	: Removes and returns the first item.
a.slice(start[, end])	: Returns a sub-array.
a.sort([cmpfn])	: Takes an optional comparison function.
a.splice(start, delcount[, item1[, ...[, itemN]]]) :	Lets you modify an array by deleting a section and replacing it with more items.
a.unshift(item1[, item2[, ...[, itemN]]])	: Prepends items to the start of the array.

## Functions
basic function
```js
function add(x, y) {
  var total = x + y;
  return total;
}

add(); // NaN
add(2, 3, 4); // 5
```
function 은 정의된 파라미터보다 더 많이 가지거나
0 개도 가질 수 있다.



arguments : an array-like object
```js
function add() {
  var sum = 0;
  for (var i = 0, j = arguments.length; i < j; i++) {
    sum += arguments[i];
  }
  return sum;
}

add(2, 3, 4, 5); // 14
/// Rest parameter syntax
function avg(...args) {
  var sum = 0;
  for (let value of args) {
    sum += value;
  }
  return sum / args.length;
}

avg(2, 3, 4, 5); // 3.5
/// array
function avgArray(arr) {
  var sum = 0;
  for (var i = 0, j = arr.length; i < j; i++) {
    sum += arr[i];
  }
  return sum / arr.length;
}

avgArray([2, 3, 4, 5]); // 3.5
/// apply
avg.apply(null, [2, 3, 4, 5]); // 3.5
/// anonymous function
var avg = function() {
  var sum = 0;
  for (var i = 0, j = arguments.length; i < j; i++) {
    sum += arguments[i];
  }
  return sum / arguments.length;
};
```
IIFEs
var 변수 숨기기, 지역변수 사용
data privacy 를 위해 사용한다.
```
// ES5
(function(){
	var a = 5;
})();
// ES6
{
	const a = 5;
	let b = 10;
}
```

## Custom objects
javascript functions 을 classes 처럼 사용한다.
```js
// #1
function makePerson(first, last) {
  return {
    first: first,
    last: last
  };
}
function personFullName(person) {
  return person.first + ' ' + person.last;
}
function personFullNameReversed(person) {
  return person.last + ', ' + person.first;
}

s = makePerson('Simon', 'Willison');
personFullName(s); // "Simon Willison"
personFullNameReversed(s); // "Willison, Simon"

/// #2
function makePerson(first, last) {
  return {
    first: first,
    last: last,
    fullName: function() {
      return this.first + ' ' + this.last;
    },
    fullNameReversed: function() {
      return this.last + ', ' + this.first;
    }
  };
}

s = makePerson('Simon', 'Willison');
s.fullName(); // "Simon Willison"
s.fullNameReversed(); // "Willison, Simon"

/// #3
function Person(first, last) {
  this.first = first;
  this.last = last;
  this.fullName = function() {
    return this.first + ' ' + this.last;
  };
  this.fullNameReversed = function() {
    return this.last + ', ' + this.first;
  };
}
var s = new Person('Simon', 'Willison');  // function 을 new 로 만드네;;
```
Inner function


## Closures
```js
function makeAdder(a) {
  return function(b) {
    return a + b;
  };
}
var x = makeAdder(5);
var y = makeAdder(20);
x(6); // 11
y(7); // 27
```


# prototype
자바스크립트에서 객체는 [[Prototype]] 이라는 은닉(private) 속성을 가진다.
객체가 만들어지기 위해 그 객체의 모태가 되는 객체 (일종의 class)

```js
function A() {}
var AA = new A();
```
AA.prototype 은 없다. prototype 음 function 에만 존재함
AA의 __proto__ 는 object prototype, new 를 사용해서 객체를 생성했으므로
`__proto__` 객체를 만들어내기 위해 사용된 객체 원형에 대한 연결

자바스크립트 기초가 부족한 사람들이 말하는 프로토 타입은 `prototype 프로퍼티`

## 자바스크립트의 프로토타입 프로퍼티란?
모든 함수 객체의 Constructor 는 prototype 이란 프로퍼티를 가지고 있다.
이 prototype 프로퍼티는 객체가 생성될 당시 만들어지는 객체 자신의 원형이 될 prototype 객체를 가리킨다.

자바스크립트의 모든 객체는 생성과 동시에 자기 자신이 생성될 당시의 정보를 취한 `Prototype Object`
라는 새로운 객체를 Cloning 하여 만들어낸다.
프로토타입이 객체를 만들어내기 위한 원형이라면 이 `Prototype Object` 는 자기 자신의 분신이며
자신을 원형으로 만들어질 다른 객체가 참조할 프로토타입이 된다.

객체 자신을 이용할 다른 객체들이 프로토타입으로 사용할 객체가 `Prototype Object` 인 것이다.
위에서 언급한 `__proto__` 라는 prototype 에 대한 link 는 상위에서 물려반은 객체의 프로토타입에 대한 정보이며
prototype 프로퍼티는 자신을 원형으로 만들어질 새로운 객체들 즉 하위로 물려줄 연결에 대한 속성이다.


> __proto__ 는 원형에 대한 연결, prototype 은 하위로 물려줄 연결에 대한 속성

```
function foo(x) {
	this.x = x;
};

var A = new foo('hello');
```
A __proto__ 는 foo
prototype 은 없음

console.log(A.x);
의 결과 는 hello

console.log(A.prototype.x)
의 결과는 syntax error
x 는 아직 정의 되지 않음... ?

`prototype 프로퍼티`는 Constructor 가 가지는 프로퍼티
함수객체만 이 프로퍼티를 가지고 있다.

A 객체는 함수 객체가 아니다. foo라는 원형을 이용하여 함수객체를 통해 만들어진 Object 객체에 확장된
단일 객체, 즉 A는 prototype 프로퍼티를 소유하고 있지 않기에 에러임
즉 프로토타입을 이해하려면 foo.prototype 은 OK, A.prototype.x 는 error 다

```js
//#예제 1.
var A = function () {
    this.x = function () {
         //do something
    };
};

//#예제 2.
var A = function () { };
A.prototype.x = function () {
    //do something
};
```

예제 1의 this.x 는 A의 프로퍼티 x이고 예제 2는 A의 prototype object 에 연결된 x이다.
왜? prototype 을 사용하여야 하는가?
하위 객체들에게 미치는 영향

## 프로토타입 체인을 이용한 상속
객체의 어떤 속성에 접근하려 할 때, 객체 자체 속성 뿐만 아니라 객체의 프로토타입,
프로토타입의 프로토타입 등 프로토타입 체인의 종단에 이를 때까지 속성을 탐색한다.

프로토타입 체인
객체의 생성 과정에서 모태가 되는 프로토타입과의 연결고리가 이어져 상속관계를 통하여 상위 프로토타입으로 연속해서 이어지는 관계를 프로토타입 체인이라고 한다. 이 연결은 __proto__ 를 따라 올라간다.

프로토타입 체인은 하위 객체에서 상위객체의 프로퍼티와 메소드를 상속받는다.
그리고 동일한 이름의 프로퍼티와 메소드를 재정의 하지 않는 이상 상위에서 정의한 내용을 그대로 물려받는다.


## 프로토타입기반 프로그래밍 이해하기
객체의 원형인 프로토타입을 이용하여 새로운 객체를 만들어내는 프로그래밍 기법

프로토타입을 이용한 복사와 객체 특성을 확장해 나가는 방식을 통해 새로운 객체를 생성해낸다.

Prototype Object, Prototype Link


## promise
[ES6 Promises(1) - the API](http://webframeworks.kr/tutorials/translate/es6-promise-api-1/)
Promises 는 비동기 프로그래밍의 한 부분을 도와주는 패턴입니다.
### producing a promise
```js
var promise = new Promise(
        function (resolve, reject) { // (A)
            ...
            if (...) {
                resolve(value); // success
            } else {
                reject(reason); // failure
            }
        });
```
### consuming a promise
```js
promise.then(
        function (value) { /* fulfillment */ },
        function (reason) { /* rejection */ }
    );
```

### promise 를 사용하는 2가지 방법
http://han41858.tistory.com/11
`new Promise(function(resolve, reject))`를 사용하는 패턴과 `Promise.resolve(function())` 로 시작하는 패턴
Promise.resolve 는 reject 가 없고 에러가 발생할 경우, throw 를 호출한다.

### singleton
http://blog.javarouka.me/2012/02/javascripts-pattern-1-singeton-patterrn.html

## 참고
[A re-introduction to JavaScript (JS tutorial)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript)
[상속과 프로토타입](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain)
[Object prototype 이해하기](http://insanehong.kr/post/javascript-prototype/)
[javascript 개발 가이드](https://github.com/nhnent/fe.javascript/wiki)
