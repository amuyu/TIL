# event 사용
1.사용자 인터페이스의 스마트 계약 반환 값
2.데이터가있는 비동기식 트리거
3.더 싼 형태의 창고

## 사용자 인터페이스의 스마트 계약 반환 값
sendTransaction 호출 후, 함수에 대한 리턴 값을 이벤트로 받는다.
When the transaction invoking foo is mined, the callback inside the watch will be triggered.

## 더 싼 형태의 창고
프론트 엔드에 의해 렌더링 될 수있는 히스토리 데이터를 저장하는 것입니다.

# index 를 사용할 수 잇다.
```
event Transfer(address indexed _from, address indexed _to, uint256 _value)
...
tokenContract.Transfer ({_ from : senderAddress})
...
tokenContract.Transfer ({_ to : receiverAddress})
```

# topic
The topics argument is order-dependent. For non-anonymous events, the first item in the topic list is always the keccack hash of the event signature.
Subsequent topic items are the hex encoded values for `indexed event arguments`. 
When transaction emits an event, it is stored in the transaction’s log

```js
example2.doWork.call(5); // returns 6, but
example2.sum.call(); // returns 0
```
However, call invocations do not modify blockchain and therefore will not modify sum variable nor log events

To persist changes to storage and emitted events user must send transaction signed with his private key and supply gas for the execution.

# tip
transaction 저장할 필요가 없다?
event 로부터 데이터를 긁어온다.

# 참고
[Introduction to Event Logs on Ethereum](https://jakubstefanski.com/post/2017/10/ethereum-event-logs/)
[Technical Introduction to Events and Logs in Ethereum](https://media.consensys.net/technical-introduction-to-events-and-logs-in-ethereum-a074d65dd61e)
[keccak_256 tool](https://emn178.github.io/online-tools/keccak_256.html)
