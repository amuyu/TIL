# Subject
```js
var subject = new Rx.Subject();
var subscription = subject.subscribe(
  x => console.log('next: ' + x),
  e => console.log('error: ' + e.message),
  () => console.log('completed'));

subject.next(1); // => next: 1
subject.next(2); // => next: 2
subject.completed(); // => completed
subscription.dispose();

//출처: http://visualize.tistory.com/471 [시각화를 배우고 정리합니다]
```

# map
// https://www.learnrxjs.io/operators/transformation/map.html
```js
import { from } from 'rxjs/observable/from';
import { map } from 'rxjs/operators';

//emit (1,2,3,4,5)
const source = from([1, 2, 3, 4, 5]);
//add 10 to each value
const example = source.pipe(map(val => val + 10));
//output: 11,12,13,14,15
const subscribe = example.subscribe(val => console.log(val));
```

## 참고
[RxJS - Subject 사용하기](http://visualize.tistory.com/471)
//http://arnoldyoo.tistory.com/18 [Arnold's life]
