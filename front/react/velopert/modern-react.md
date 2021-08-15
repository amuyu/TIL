# 개요

- useEffect
- useMemo
- useCallback
- React.memo
- useReducer
- 커스텀 Hooks
- Context API
- immer
- componentDidCatch

# defaultProps

defaultProps 설정

```js
Hello.defaultProps = {
  name: "이름없음",
};
```

# useEffect 를 사용하여 마운트/언마운트

마운트 시에 하는 작업들

- `props` 로 받은 값을 컴포넌트의 로컬 상태로 설정
- 외부 API 요청 (Rest api)
- 라이브러리 사용 (D3, Video.js 등...)
- setInterval 을 통한 반복작업 혹은 setTimeout 을 통한 작업 예약

언마운트 시에 하는 작업들

- setInterval, setTimeout 사용하여 등록한 작업들 clear 하기
- 라이브러리 인스턴스 제거

## deps 에 특정 값 넣기

deps 에 특정 값을 넣게 된다면, 컴포넌트가 처음 마운트 될 때에도 호출이 되고,
지정한 값이 바뀔 때에도 호출이 됩니다.
useEffect 안에서 사용하는 상태나, props 가 있다면 useEffect 의 deps 에 넣어주어야 합니다.
deps 파라미터를 생략하면, 컴포넌트가 리렌더링 될 때마다 호출됩니다.

참고로 리액트 컴포넌트는 기본적으로 부모컴포넌트가 리렌더링되면 자식 컴포넌트 또한 리렌더링 됩니다.
물론, 실제 DOM 에 변화가 반영되는 것은 바뀐 내용이 있는 컴포넌트에만 해당합니다.
하지만, Virtual DOM 에는 모든걸 다 렌더링하고 있다는 겁니다.

나중에는 컴포넌트를 최적화하는 과정에서 기존의 내용을 그대로 사용하면서 Virtual DOM 에 렌더링 하는
리소스를 아낄 수도 있습니다.

```js
useEffect(() => {
  console.log("set user");
  console.log(user);
  return () => {
    console.log("before user");
    console.log(user);
  };
}, [user]);
```

====

# useMemo 를 사용하여 연산한 값 재사용하기

useMemo 에 등록한 값이 변할 때만 리렌더링 한다.
memo 는 memorized 를 뜻한다.

```js
const count = useMemo(() => countActiveUsers(users), [users]);
```

====

# useCallback 을 사용하여 함수 재사용하기

useCallback 은 특정 함수를 새로 만들지 않고 재사용하고 싶을 때 사용합니다.

useMemo, useCallback 은 props 가 바뀌지 않으면 Virtual DOM 에
새로 렌더링하지 않고 컴포넌트의 결과물을 재사용 하는 최적화 작업에 사용한다.

====

# React.memo 를 사용한 컴포넌트 리렌더링 방지

React.memo 는 컴포넌트의 props 가 바뀌지 않았다면, 리렌더링을 방지하여
컴포넌트의 리렌더링 성능 최적화를 해줄 수 있다.

```js
export default React.memo(CreateUser);
```

React.memo 에서 두번째 파라미터에 `propsAreEqual` 이라는 함수를 사용하여
특정 값들만 비교를 하는 것도 가능합니다.

```js
export default React.memo(
  UserList,
  (prevProps, nextProps) => prevProps.users === nextProps.users
);
```

잘못 사용할 경우, 버그들이 발생할 수 있다. 함수형 업데이트로 전환을 안했는데
users 만 비교하게 되면 onToggle 과 onRemove 에서 최신 users 배열을 참조하지 않아 심각한 오류가 발생할 수 있다.

====

# useReducer 를 사용하여 상태 업데이트 로직 분리하기

상태를 관리하는 함수
컴포넌트의 상태 업데이트 로직을 컴포넌트에서 분리시킬 수 있다.
컴포넌트 바깥에 작성할 수도 있고, 다른 파일에 작성 후 불러와서 사용 할 수도 있다.
현재 상태와 액션 객체를 파라미터로 받아와서 새로운 상태를 반환해준다.

```js
function reducer(state, action) {
  return nextState;
}
```

`action` 은 업데이트를 위한 정보를 가지고 있다.

```js
// 카운터에 1을 더하는 액션
{
  type: 'INCREMENT'
}
// 카운터에 1을 빼는 액션
{
  type: 'DECREMENT'
}
// input 값을 바꾸는 액션
{
  type: 'CHANGE_INPUT',
  key: 'email',
  value: 'tester@react.com'
}
// 새 할 일을 등록하는 액션
{
  type: 'ADD_TODO',
  todo: {
    id: 1,
    text: 'useReducer 배우기',
    done: false,
  }
}
```

`useReducer` 는 다음과 같이 사용한다.

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

`state` 는 컴포넌트에서 사용할 수 있는 상태를 가르키게 되고, `dispatch` 는 액션을 발생시키는 함수이다.

컴포넌트에서 관리하는 값이 하나고, 그 값이 단순한 숫자, 문자열 또는 boolean 값이라면 useState 로 관리하는게 편하다
컴포넌트에서 관리하는 값이 여러개가 되어서 상태 구조가 복잡해진다면 useReducer 로 관리하는 것이 편해질 수 있다.
상태 구조가 복잡해질 경우, React.memo() 에서 특정 값들만 비교하도록 한다.

===

# 커스텀 Hooks 만들기

useState, useEffect, useReducer, useCallback 등 Hooks 를 사용하여 원하는 기능을 구현하고
컴포넌트에서 사용하고 싶은 값들을 반환해준다.

코드 재사용, 한 파일이 길어지는거 방지

```js
import { useState, useCallback } from "react";

function useInputs(initialForm) {
  const [form, setForm] = useState(initialForm);
  // change
  const onChange = useCallback((e) => {
    const { name, value } = e.target;
    setForm((form) => ({ ...form, [name]: value }));
  }, []);
  const reset = useCallback(() => setForm(initialForm), [initialForm]);
  return [form, onChange, reset];
}

export default useInputs;
```

====

# Context API 를 사용한 전역 값 관리

Context API 를 사용하면 프로젝트 안에서 전역적으로 사용 할 수 있는 값을 관리 할 수 있다.

`React.createContext()` 함수를 사용해서 새로운 Context 를 만듭니다.

```js
const UserDispatch = React.createContext(null);
```

Context 를 만들면, Context 안에 Provider 라는 컴포넌트가 들어있는데 이 컴포넌트를 통하여
Context 의 값을 정할 수 있습니다.

```js
<UserDispatch.Provider value={dispatch}>...</UserDispatch.Provider>
```

이렇게 설정해주고 나면 Provider 에 의하여 감싸진 컴포넌트 중 어디서든지 우리가 Context 의 값을
다른 곳에 바로 조회해서 사용할 수 있다.

====

# Immer 사용법

불변성을 지켜주면서 배열이나 객체를 업데이트 하는 모듈
구조가 복잡한 경우에 사용하기 좋다.
성능적으로는 Immer 를 사용하지 않는 코드가 조금 더 빠르다.

```js
import produce from "immer";
const state = {
  number: 1,
  dontChangeMe: 2,
};

const nextState = produce(state, (draft) => {
  draft.number += 1;
});

console.log(nextState);
```

====

# componentDidCatch 로 에러 잡아내기

`componentDidCatch` 라는 생명주기 메서드를 사용하여 리액트 앱에서 발생하는 에러를 처리하는 방법을 알아보자

===

# prettier typescript not working

https://dev.to/timo_ernst/prettier-autoformat-for-typescript-not-working-13d8
