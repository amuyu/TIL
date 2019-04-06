# Rx
Rx proves to be most valuable when composing sequences of events.
- UI events like mouse move, button click
- Domain events like property changed, collection updated, “Order Filled”, “Registration accepted” etc
- Infrastructrure events like from file watcher, system and WMI events
- Integration events like a broadcast from a message bus or a push event from websockets api or other low latency middleware
- Integration with a CEP engine like StreamInsight or StreamBase

Rx is also very well suited for introducing and managing concurrency for the purpose of offloading.
That is, performing a given set of work concurrently to free up the current thread.
A very popular use of this is maintaining a responsive UI.

# Won't use Rx
+ IObservable<T> 는 IEnumerable<T> 를 대체할 수 없다. 
- IEnumerable<T> 가 자연스러운 곳에는 Rx 를 사용해서는 안된다. 


# Rx 이해
array 에 대해서 요소별로 처리하는 방식을 보면,
다음과 같은 방식은 poll 방식이라고 할 수 있다.
```js
const foo = ["a","b","c","d","e"];

for ( var f in foo ) {
    console.log(f);
}
```
observable 을 사용하면 push 방식이라고 할 수 있다.
why? array 가 stream 위에서 sequential 하게 데이터를 push 해 subscribe callback
으로 받게 되므로
```js
const foo = new Rx.Observable.of("a","b","c","d","e");

foo.subscribe(f => {
    console.log(f);
});
```


# 참고
[반응형 프로그래밍이란 무엇인가?](https://brunch.co.kr/@yudong/33)
