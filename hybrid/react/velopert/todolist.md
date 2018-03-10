# 흔하디 흔한 할 일 목록 만들기
[흔하디 흔한 할 일 목록 만들기](https://velopert.com/3480)
create-react-app 을 사용한 할일 목록 만들기

## 상태 관리 하기
Form 과 TodoItemList 에서 상태가 필요하다.
리액트에 익숙하지 않다면, 리액트의 state를 각 컴포넌트에 넣어주어야 하는 것이 아닌가? 라고 생각이 들 수 있다.

다른 컴포넌트 끼리 직접 데이터를 전달하는 것은 ref를 사용하여 할 수야 있겠지만 비효율적이다. 꼬이고 유지보수가 힘들어지므로...

컴포넌트들은 부모를 통하여 대화를 해야 한다.
여기서는 App 이 Form 과 TodoItemList 의 컴포넌트이니, App에 input, todos 상태를 넣어주고 해당 값들과 값들을 업데이트 하는 함수들을 각각 컴포넌트에 props 로 전달해주어서 기능을 구현한다.

리액트에서 자주 사용하는 구조 중에선, 컴포넌트를 만들 때 프리젠테이셔널 컴포넌트와 컨테이너 컴포넌트로 구분하는 방식이 있다.

App에서 다음과 같이 state를 정의 한다.
```js
state = {
  input: '',
  todos: [
    { id: 0, text: ' 리액트 소개', checked: false },
    { id: 1, text: ' 리액트 소개', checked: true }
    { id: 2, text: ' 리액트 소개', checked: false }
  ]
}
```

### todos
todos 를 추가하는 부분이나 초기 데이터를 정의한 부분을 보면
object literal syntax 를 사용해서 object 를 초기화하는 것을 볼 수 있다.
```
todos: [
  { id: 0, text: ' 리액트 소개', checked: false },
  { id: 1, text: ' 리액트 소개', checked: true }
  { id: 2, text: ' 리액트 소개', checked: false }
]
...
todos: todos.concat({
  id: this.id++,
  text: input,
  checked: false
})
...
handleRemove = (id) => {
    const { todos } = this.state;
    this.setState({
      todos: todos.filter(todo => todo.id !== id)
    });
  }
...
```
