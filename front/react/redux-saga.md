

# 살펴보기
'Task' 라는 개념을 Redux로 가져오기 위한 지원 라이브러리이다.
비동기처리를 Task 로써 기술하기 위해 `Effect`와 비동기처리를 동기적으로 표현하는 방법을 제공한다.

*Effect*
- select : state 로부터 필요한 데이터를 꺼낸다.
- put : action 을 dispatch 한다.
- take : action 을 기다린다. 이벤트의 발생을 기다린다.
- call  promise 의 완료를 기다린다.
- fork : 다른 task 를 시작한다.
- join : 다른 task 의 종료를 기다린다.



Sagas 는 generator 함수로 구성되어 있다.
```js
function* mySaga() {

}
```

# login api
`LOGIN_REQUEST` 가 store 에 디스패처 되기 전까지 기다리다가 dispatch 되면 실행
```js
import * as api from "api";
import { LoginRequestAction, loginSuccess, loginFailure } from "./loginActions";

function* loginSaga() {
  const action: LoginRequestAction = yield take("LOGIN_REQUEST");
  const { name, password } = aciton.payload;
  try {
    yield call(api.login, name, password);
  } catch (err) {
    yield put(loginFailure(err));
    return;
  }
  yield put(loginSuccess());
}

```


# usage

# store 에 middleware 셋팅
```js
import { createStore, applyMiddleware } from 'redux';
import createSagaMiddleware from 'redux-saga';
import logger from 'redux-logger';
import reducer from './reducers';
import rootSaga from './sagas';

export default function configureStore(initialState) {
  const sagaMiddleware = createSagaMiddleware();
  const store = createStore(
    reducer,
    initialState,
    applyMiddleware(
      sagaMiddleware, logger()
    )
  );
  sagaMiddleware.run(rootSaga);
  return store;
};
```

# takeEvery
Effect
```js
import { call, put, fork, takeEvery } from 'redux-saga/effects';

function* runRequestSuggest(action) {
  const { data, error } = yield call(API.suggest, action.payload);
  if (data && !error) {
    yield put(successSuggest({ data }));
  } else {
    yield put(failureSuggest({ error }));
  }
}

function* handleRequestSuggest() {
  yield takeEvery(REQUEST_SUGGEST, runRequestSuggest);
}

export default function* rootSaga() {
  yield fork(handleRequestSuggest);
}
```

# delay & takeEvery
```js
import { delay } from 'redux-saga';
import { call, put, fork, take } from 'redux-saga/effects';

function* runRequestSuggest(text) {
  const { data, error } = yield call(API.suggest, text);
  if (data && !error) {
    yield put(successSuggest({ data }));
  } else {
    yield put(failureSuggest({ error }));
  }
}

function forkLater(task, ...args) {
  return fork(function* () {
    yield call(delay, 1000);
    yield fork(task, ...args);
  });
}

function* handleRequestSuggest() {
  let task;
  while (true) {
    const { payload } = yield take(REQUEST_SUGGEST);
    if (task && task.isRunning()) {
      task.cancel();
    }
    task = yield forkLater(runRequestSuggest, payload);
  }
}

export default function* rootSaga() {
  yield fork(handleRequestSuggest);
}
```




# ref
[redux-saga로 비동기처리와 분투하다.](https://github.com/reactkr/learn-react-in-korean/blob/master/translated/deal-with-async-process-by-redux-saga.md)