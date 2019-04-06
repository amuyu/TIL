# Redux 예제를 통해 사용해보기
store : 모든 동적 데이터들을 담아두는 곳
action : 어떤 변화가 일어나야 할 지 나타내는 객체, 모든 action 에는 type 이 있다.
reducer : action 타입에 따라 행동, action 객체를 받았을 때, 데이터를 어떻게 바꿀지 처리할지 정의하는 객체

# Redux: React 앱의 효율적인 데이터 교류
Redux 는 data-state와 UI-state를 관리해주는 도구이다.
데이터 교류 및 state 관리를 쉽고 효율적으로 해준다.

React 에서 데이터흐름은 단일 방향으로만 흐른다. state 및 props 를 사용해 parent-child 관계를 통하여 데이터를 교류하는데 컴포넌트 개수가 많아지면 데이터 흐름이 복잡해진다.

이에 대해서 React에서는 Flux 사용을 조언한다.

FLUX 패턴
Action -> Dispatcher -> Store -> View
View 에서 Dispatcher 로 Action 을 보낼 수 있음
View -> Action -> Dispatcher

store 에 모든 데이터를 담고 있고, 컴포넌트끼리는 store 통해 교류한다. dispatch 가 발생하면 store에 있는 데이터를 업데이트 하고 데이터가 변경되면 subscribe 해서 데이의 변동을 반영한다.

Dispatcher 는 작업이 중첩 되지 않도록 한다. 어떤 Action 이 Dispatcher 를 통해 Store에 있는 데이터를 처리하고 그 작업이 끝날 때 까지 다른 Action 들을 대기시킨다.

Redux 는 Flux 아키텍쳐를 좀 더 편하게 사용할 수 있게 해주는 라이브러리이다. (dispatch > subscirbe)

## Redux 3가지 원칙
- Single Source of Truth : 어플리케이션의 state를 위해 단 한 개의 store를 사용한다. store 의 데이터 구조는 개발자 하기 나름이다. 보통 nested 된 구조로 이루어져 있다.
- State is read-only : state 를 변경하기 위해서는 action 이 dispatch 되어야 한다.
- Changes are made with Pure Functions : action 객체를 처리하는 함수를 Reducer 라고 하는데 Reducer 는 순수 함수로 작성되어야 한다. action 을 dispatch 하면 reducer 가 받아서 state를 변경한다.

순수 함수란
외부 네트워크 혹은 데이터베이스에 접근하지 않아야 한다.
return 값은 오직 parameter 값에 의존해야 한다.
인수는 변경되지 않아야 한다.
같은 인수로 실행된 함수는 언제나 같은 결과를 반환해야 한다.
순수하지 않은 API 호출을 하지 말아야한다. (Date 및 Math 함수 등)


## store 명령
store.getState() : 현재 스토어에 있는 데이터를 반환한다.
store.dispatch(ACTION) 을 호출해서 action 을 실행한다.
store.subscribe(LISTENER) : dispatch 메소드가 실행되면 리스너가 실행된다. 다시 리렌더링하도록 설정한다.

## react-redux 를 사용하지 않고 만들어보기
store 생성 시, reducer 가 인수로 사용된다.
```js
/*
 * Store
 */
const store = createStore(counterReducer);
```
생성 후, action 을 호출하기위 dispatch 를 실행한다.
```js
store.dispatch(increase(1));
```
increase라는 정의된 ACTION 을 실행하고 action에서는 type 과 수행 결과를 리턴한다.
```js
/*
 * Action
 */
const INCREMENT = "INCREMENT";

function increase(diff) {
    return {
        type: INCREMENT,
        addBy: diff
    };
}
```
reducer 에서는 초기 상태를 정의하고 action type 에 따라 state 에 데이터를 변경한다.
```js
/*
 * Reducer
 */
const initialState = {
    value: 0
};

const counterReducer = (state = initialState, action) => {
    switch(action.type) {
        case INCREMENT:
            return Object.assign({}, state, {
                value: state.value + action.addBy
            });
        default:
            return state;
    }
}
```

## 모듈
redux, react-redux 설치

## react-redux 를 사용하기
action 과 reducer는 별도로 작성하는게 관리하기 편리한듯

connect를 사용해 React Component를 Redux Store 에 연결해준다.
```js
connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
```
mapStateToProps : store의 state 컴포넌트를 props 에 매핍시켜준다.
mapDispatchToProps : 함수형 props 를 실행했을 때, 지정한 action 을 dispatch 하도록 설정한다.


### combineReducers
`combineReducers` 여러 개의 reducer를 한 개로 합칠 때 사용하는 redux 메소드
```js
const counterApp = combineReducers({
    counter,
    extra
});
```
이 reducer를 사용하셔 store를 만들게 되면, store의 구조는 다음과 같다.
```js
{
    counter: { value: 0, diff: 1 }
    extra: { value: 'this_is_extra_reducer' }
}
```
각 reducer 에 다른 key를 주고 싶다면
```js
const counterApp = combineReducers({
    a: counter,
    b: extra
});
```

### store 생성
store를 만들 때는 createStore() 메소드를 사용하며 reducer 가 인수로 사용된다.
```js
/*
 * Store
 */
const store = createStore(counterReducer);
```

렌더링 할 때, Redux 컴포넌트인 <Provider> 에 store를 설정해주면 하위 컴포넌트들에 따로 parent-child 구조로
전달해주지 않아도 connect 될 때 store에 접근할 수 있다.



[참고](https://velopert.com/1015)

## 참고
[redux 정복하기](https://velopert.com/3346)
[redux 를 통한 React 어플리케이션 상태 관리](https://velopert.com/3365)
[Redux: 예제를 통해 사용해보기](https://velopert.com/1266)
