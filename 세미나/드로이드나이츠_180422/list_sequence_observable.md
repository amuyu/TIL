# List, Sequence, Observable
method chaining
sequence, iterable

List 의 chaining 은 매 chain 마다 결과값을 새로운 리스트에 담아 넘겨주는 방식
- 리스트에 데이터가 많은 경우 시간이 오래 걸릴 수 있다.
- 최종 결과 중 특정 값을 찾으려 할 때 모든 element에 대한 chain이 끝나야만 확인할 수 있다.

Sequence chaining 은 각 chain 마다 적절한 sequence 를 새로 만들고
chain의 첫 sequence iterator부터 순차적으로 chain된 sequence 의
iterator의 hasNext, next 가 수행되어 최종 결과물 반환
- terminate method 가 호출될 때 해당 method에 따른 결과값이 호출
- 작은 사이즈를 가지고 있으면 list보다 연산이 느려질 수 있음
- 많은 양의 사이즈 혹은 특정값을 반환받고 싶을 때 더 효과적일 수 있다.

[발표자료](https://speakerdeck.com/omjoonkim/what-is-the-difference-between-list-sequence-and-observable)
