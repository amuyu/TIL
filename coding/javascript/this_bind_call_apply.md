
# this
Method 내에서 this 는 object
global function 내에서 this는 window, global or undefined

# call
Function.prototype.call 을 이용해서도 문제를 해결할 수 있다. call 은 파라미터 중 첫 번째 인자를, 함수 내부에서 사용할 this 로 만들어 준다

# apply
apply 도 마찬가지다. 그러나 call 과는 달리 apply 는 파라미터를 배열(Array) 형태로 받는다.

# bind
Function.prototype.bind 는 원하는 Function 에 인자로 넘긴 this 가 바인딩 된 새로운 함수를 리턴한다.
```
function A() {
    const self = this
    self.text = 'hello world'
}

A.prototype.print = function() {
    var a = gFn.bind(this)
    a()
}

function gFn() {
    console.log(this.text)
}
```

# 참소
[Javascript this, call, apply 그리고 bind](https://blog.weirdx.io/post/3214)
[bind 고급편](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Bound_functions_used_as_constructors)
