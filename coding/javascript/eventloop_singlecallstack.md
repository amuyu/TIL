# 이벤트 루프
자바사크립트 엔진은 단일 호출 스택을 사용하고 요청이 들어올 때마다 해당 요청을
순차적으로 호출 스택에 담아 처리한다. 비동기 요청과 동시성에 대한 처리는 자바스크립트 엔진을
구동하는 환경, 즉 브라우저나 Node.js 가 담당한다.

Node.js 는 비동기 IO 를 지원하기 위해 libuv 라이브러리를 사용하며, 이 libuv 가 이벤트루프를 제공한다.

자바스크립트의 함수 실행 방식을 `Run to Completion` 이라고 하는데
하나의 함수가 실행되면 이 함수의 실행이 끝날 때까지는 다른 어떤 작업도 중간에 끼어들지 못한다.

그렇다면 비동기 요청에 대한 처리를 어떻게 할 수 있을까?
이 역할을 하는 것이 태스크 큐와 이벤트 루프이다.

# 태스크 큐와 이벤트 루프
- 모든 비동기 API 들은 작업이 완료되면 콜백 함수를 태스크 큐에 추가한다.
- 이벤트 루프는 '현재 실행중인 태스크가 없을 때' (주로 호출 스택이 비워졌을 때) 태스크 큐의 첫 번째 태스크를 꺼내와 실행한다.


실행중인 태스크 < 태스크 큐

아래의 코드는 콘솔에 B->A 순서로 출력한다.

```js
setTimeout(function() {
    console.log('A');
}, 0);
console.log('B');
```

프론트 환경에서 렌더링 엔진과 관련해서 이런 코드가 요긴하게 쓰인다.

```js
$('.btn').click(function() {
    showWaitingMessage();
    longTakingProcess();
    hideWaitingMessage();
    showResult();
});
```

`longTakingProcess`가 너무 오래 걸리는 작업이기 때문에 그 전에 `showWaitingMessage`를 호출해서 로딩 메시지('로딩중…'과 같은)를 보여주려고 한다. 하지만 실제로 이 코드를 실행해 보면 화면에 로딩 메시지가 표시되는 일은 없을 것이다. 이유는 `showWaitingMessage` 함수의 실행이 끝나고 렌더링 엔진이 렌더링 요청을 보내도 해당 요청은 태스크 큐에서 이미 실행중인 태스크가 끝나기를 기다리고 있기 때문이다. 실행중인 태스크가 끝나는 시점은 호출 스택이 비워지는 시점인데, 그 때는 이미 `showResult` 까지 실행이 끝나 있을 것이고, 결국 렌더링이 진행되는 시점에는 `hideWaitingMessgae`로 인해 로딩 메시지가 숨겨진 상태일 것이다. 이를 해결하기 위해서 다음처럼 `setTimeout`를 사용할 수 있다.

```js
$('.btn').click(function() {
    showWaitingMessage();
    setTimeout(function() {
        longTakingProcess();
        hideWaitingMessage();
        showResult();
    }, 0);
});
```

`longTakingProcess` 를 태스크 큐에 추가하기 때문에 이벤트 루프는 렌더링 요청을 먼저 처리하고 로딩 메시지를 화면에 보여지게 된다.


# 마이크로 태스크

```js
setTimeout(function() { // (A)
    console.log('A');
}, 0);
Promise.resolve().then(function() { // (B)
    console.log('B');
}).then(function() { // (C)
    console.log('C');
});
```

이 코드의 콘솔 순서는 B->C->A 인데, 이유는 바로 프라미스가 마이크로 태스크를 사용하기 때문이다.

마이크로 태스크?

마이크로 태스크는 일반 태스크보다 더 높은 우선순위를 갖는 태스크라고 할 수 있다.







```js
while(queue.waitForMessage()){
  queue.processNextMessage();
}
```


## 참고
https://meetup.toast.com/posts/89
