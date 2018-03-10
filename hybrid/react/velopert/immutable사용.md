[React Immutable.js – 리액트의 불변함, 그리고 컴포넌트에서 Immutable.js 사용하기](https://velopert.com/3486)

# Overview
리액트는 state를 변경할 때 무조건, setState로 업데이트
업데이트 과정에서 기존의 객체의 값을 직접적으로 수정하면 안 됨

부모 컴포넌트가 리렌더링 되면, 자식 컴포넌트들 또한 리렌더링 된다.
이런 최적화는 shouldComponentUpdate 를 구현해서,,,

# 불변함을 유지하다보면 코드가 복잡해진다.
코드가 복잡해지는 작업을 쉽게 해주는게 바로 `Immutable.js` 이다.

immutable 을 사용할 때 패키지 설치를 해야한다.
Codesadbox? online editor

## 사용 규칙
1. 객체는 map
2. 배열은 list
3. 설정할 땐 set
4. 읽을 땐 get
5. 읽은 다음에 설정할 때 Update
6. 내부에 있는걸 ~ 할 땐 뒤에 In 을 붙인다.
7. 일반 자바스크립트 객체로 변환할 땐 toJS
8. List 엔 배열 내장함수와 비슷한 함수들이 있다.
    - push, slice, filter, sort, concat
9. 특정 key 를 지울 때 (혹은 List에서 원소를 지울 때) delete 사용


## 리액트 컴포넌트에서 Immutable 사용하기
state 자체를 Immutable 데이터로 사용하는 것은 지원되지 않는다.

state 내부에 Immutable 객체를 만들고, 상태관리를 모두 이 객체를 통해서 진행하면 된다.

## 계속 .get, .getIn 하는거 싫다 그렇다면 Record
Record 를 사용하면 Immutable 의 set, update, delete 등을 계속 사용할 수 있으면서 값을 조회할 때 get, getIn 을 사용할 필요 없이, data.input 이런식으로 조회를 할 수 있다.
