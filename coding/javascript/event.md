# EventEmitter
EventEmitter 는 일종의 observer 패턴 구현이다.
이벤트를 정의하고 > 이벤트에 객체를 전달 > 리스너로 불리는 함수 객체 실행

이벤트를 내보내는 객체는 `EventEmitter` 의 인스턴스 이다.
`eventEmiter.on()` 을 사용해 이벤트 정의한다.
`eventEmiter.emit()` 을 사용해 이벤트를 호출한다.

이벤트를 호출할 때, 이벤트에 붙어 있는 함수는 동기적으로 호출한다.
setImmediate()이나 process.nextTick()메소드를 사용해 리스너 함수를 비동기도 동작하도록 전환할 수 잇다.


# usage
```js
// events 모듈 사용
var events = require('events');

// EventEmitter 객체 생성
var eventEmitter = new events.EventEmitter();


// event와 EventHandler 를 연동(bind)
// eventName 은 임의로 설정 가능
eventEmitter.on('eventName', eventHandler);

eventEmitter.emit('eventName');
```



[eventemitter](https://www.haruair.com/blog/3396)
[Evnet Loop](https://velopert.com/267)
