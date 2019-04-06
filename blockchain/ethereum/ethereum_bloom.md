# bloom filter
특정 원소가 집합에 속하는지 검사하는데 사용할 수 있는 확률형 자료 구조입니다.
굉장히 빠르고 메모리 효율적
다행인 점은 이 친구가 집합에 속한 원소를 속하지 않았다고 말하는 일(False Negative)은
절대 없다는 것입니다. 반대로 속하지 않은 원소를 가끔 속해있다(False Positive)고 얘기하기는 하죠.
false positive가 발생하는 경우 데이터베이스를 조회하여 데이터 존재 여부(원소 포함 여부)를 판단할 수 있음

# false positive가 일어날 확률

# 단점
한 번 삽입한 원소는 삭제할 수 없다.

# 예
Google Chrome은 위험한 사이트 검사에 Bloom Filter를 사용

## 참고
[알아두면 좋은 자료구조 bloom filter](https://steemit.com/kr-dev/@heejin/bloom-filter)
[확률적 자료구조를 이용한 추정 - 원소 포함 여부 판단(Membership Query)과 Bloom Filter](http://d2.naver.com/helloworld/749531)
[Bloom Filter 개요](http://www.mimul.com/pebble/default/2012/03/30/1333089490367.html)
