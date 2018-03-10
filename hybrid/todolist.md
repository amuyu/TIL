[흔하디 흔한 투두리스트](https://velopert.com/3480)


# 작업환경 설정
- Node LTS 버전 설치
- yarn
- 에디터

# create-react-app 설치 및 사용
리액트 프로젝트를 생성하기 위해 페이스북에서 만든 리액트 프로젝트 생성 도구인 `create-react-app` 을 사용한다.
설치하고 프로젝트를 생성한다.

# 프로젝트 초기화
App.js 내용을 제거한다.

# 컴포넌트 구성하기
## TodoTemplate
스타일하기 위한 템플릿 역할
함수형 컴포넌트로 작성한다. ({form, children}) => {...} 형태로 작성한다.
```js
const TodoListTemplate = ({form, children}) => {
  return (
    <main className="todo-list-template">
      <div className="title">
        오늘 할 일
      </div>
      <section className="form-wrapper">
        {form}
      </section>
      <section className="todos-wrapper">
        { children }
      </section>
    </main>
  );
};

export default TodoListTemplate;
```
TodoListTemplate 사용할 때 form과 children 입력은 다음과 같이 한다.
```js
<TodoListTemplate form={<div>이렇게 말이죠.</div>}>
    <div>여기엔 children 자리구요.</div>
</TodoListTemplate>
```
## 두 번째 컴포넌트 Form 만들기


## 세 번째 컴포넌트 TodoItemList 만들기
리스트를 렌더링하게 될 때는, 함수형이 아닌 클래스형 컴포넌트로 작성해야 나중에 성능 최적화를 할 수 있다.


## 네 번째 컴포넌트 TodoItem 컴포넌트 만들기
`e.stopPropagation()` 이벤트의 확산을 멈춰준다.

todo-text 쪽을 보시면, checked 값에 따라 className 에 checked 라는 문자열을 넣어주웠습니다. CSS 클래스를 유동적으로 설정하고 싶다면 이렇게 템플릿 리터럴 을 사용하시면 됩니다.
`classnames` 를 사용하면 훨씬 쉽게 할 수 있다.


## 상태관리 하기
우리 프로젝트에서 상태가 필요한 컴포넌트는 Form 과 TodoItemList 입니다. 리액트에 익숙하지 않다면, 리액트의 state를 각 컴포넌트에 넣어주어야 하는것이 아닌가? 라고 생각이 들 수도 있습니다.

다른 컴포넌트끼리 직접 데이터를 전달하는 것은 비효율적인 방법이다.
컴포넌트들은 부모를 통하여 대화를 해야 한다. 내부 컴포넌트 끼리는 대화하지 않는다.

App 이 Form 과 TodoItemList 의 부모 컴포넌트이니, 해당 컴포넌트에 input, todos 를 넣어주고 값들을 props 로 전달해서 기능을 구현한다.

리액트에서 자주 사용하는 구조 중에서, 컴포넌트를 만들 때 프리젠테이셔널 컴포넌트와 컨테이너 컴포넌트로 구분하는 방식이 있다. 미리 소개 하자면 뷰만을 담당하는 컴포넌트와 상태 관리를 담당하는 컴포넌트로 분리하는 것을 말한다.


### 초기 state 정의하기
state 추가
```js
id = 3 // 이미 0,1,2 가 존재하므로 3으로 설정

state = {
  input: '',
  todos: [
    { id: 0, text: ' 리액트 소개', checked: false },
    { id: 1, text: ' 리액트 소개', checked: true },
    { id: 2, text: ' 리액트 소개', checked: false }
  ]
}
```


react state 에서 배열을 다룰 때는 절대로 push를 사용하면 안된다.
(아마도 immutable 때문인듯), concat 을 사용한다.

비구조화 할당
```js
const {
      handleChange,
      handleCreate,
      handleKeyPress
    } = this;
```

## TodoItemList 에서 배열을 TodoItem 컴포넌트 배열로 변환하기
todos 안에 있는 객체들을 화면에 보여주기 위해선, todos 배열을 컴포넌트 배열로 변환해주어야 한다. 배열을 변환 할 때는 자바스크립트 배열의 내장함수 map을 사용

TodoItemList 에 todos를 전달

todos 를 map() 함수를 호출해서 렌더링
배열을 렌더링 할 때에는 key 값이 꼭 있어야 한다.

## 체크 하기/체크 풀기
체크를 하거나 푸는 함수를 추가한다.

배열을 업데이트 할 때는 배열의 값을 직접 수정하면 안된다.
전개 연산자를 사용해 업데이트 해야할 배열 혹은 객체의 내용을 복사해 주어야 한다.

## 아이템 제거하기
filter 명령을 사용해서 제거

## 컴포넌트 최적화
현재 소스에서는 input 에 값을 입력할 때마다 TodoItem  render 함수가 실행되고 있음

shouldComponentUpdate 는 컴포넌트가 리렌더링 할지 말지 정해준다. todos 값이 바뀔 때, 리 렌더링 하도록 값을 비교하면 된다.

아이템 체크를 껏다 켜거나 새 todo를 입력하면 또 모든 컴포넌트가 렌더링 된다.
