# install
npm install -g create-react-app
create-react-app my-app

cd my-app
rm -f src/*

Add a file named index.css in the src/ [index.css](https://codepen.io/gaearon/pen/oWWQNa?editors=0100)
Add a file named index.js in the src/ [index.js](https://codepen.io/gaearon/pen/oWWQNa?editors=0010)

Add these three lines to the top of index.js
```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
```

index.js 에서 `Game` render 호출
```js
ReactDOM.render(
    <Game />,
    document.getElementById('root')
  );
```

# Overview

## React.component
`props` 라고 하는 parameter 를 받는다.
render metod 를 return 한다.
render method 는 무엇을 그릴지를 리턴한다.
React element 를 리턴한다.

component 는 `<ShoppingList />` 처럼 쉽게 사용할 수 있다.

## Getting Started
tutorial 에는 3가지 components 가 있다.
Square, Board, Game

Square Component 는 single button 을 그린다. board 에는 9개의 square가 있다.
Game component 는 board 를 render 한다.

renderSquare method 에서 square 에 value 를 전달한다.
Square 에서 props 로 value 를 받는다.

React component 는 state 를 갖는다. state 에 square 의 현재 value 를 저장하자
state 를 사용하면 this.props.value 를 this.state.value 로 바꿀 수 있다.
그럴려면 state.value 값을 set해야 하는데..

## Lifting State Up
현재는 Square 의 상태를 Board 에서 알아야 하는 구조기 때문에
Squre 에서 갖고 있던 State 를 Board 에서 갖도록 변경한다.

Board 에서 Square 에 state, onClick 두 가지 props 를 전달한다.

## Why Immutability Is Important
이전 code 에서 `.slice()` operator 를 사용해서 state.squares 를 copy 했다.

data를 바꾸는 두 가지 방법이 있다.
- data 를 직접 바꾸는 것 (mutate)
- 새로 copy 된 객체로 바꾸는 것

Data change with mutation
```js
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}
```

Data change without mutation
```js
var player = {score: 1, name: 'Jeff'};

var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanged, but newPlayer is {score: 2, name: 'Jeff'}

// Or if you are using object spread syntax proposal, you can write:
// var newPlayer = {...player, score: 2};
```

결과는 같지만 data를 직접 바꾸지 않는 것이 구성요소 및 성능을 향상시키는데 추가 이점이 있습니다.

### Easier Undo/Redo and Time Travel
titme travel 을 구현이 더 쉬워진다.
mutation 을 피하면 오래 전 데이터 참조를 유지할 수 있다.

### Tracking Chnages
객체에 대한 변화를 확인하기 쉽다.
immutable object 는 객체에 대한 참조가 변했는지만 확인하면 된다.

### Determining When to Re-render in React
React 에서 Immutability 은 단순한 순수 컴포넌트를 빌드할 때가 가장 큰 이점이 있다.

변화에 대한 확인이 쉬워지기 때문에 re-rendered 할 때를 결정하기도 쉽다.


## Functional Components
Square 처럼 `render` method 만 경우, functional components 를 사용한다.
React.Component 를 상속받는 class를 정의하는 것보다 간단하다.

```js
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

## Declaring a Winner
game 승자를 계산하는 함수를 추가해서 Board의 render 와 handleClick 에서
호출해 게임 승패가 결정되었는지 확인한다.


## Storing a History
Game component 에서 이동한 리스트를 보여주도록 한다.

Square 에서 Board 로 state 를 댕겨온것 처럼 Board 에서 Game 으로
State 를 댕겨온다.

## keys
리스트를 render 할 때 react 는 항상 item 에 대한 정보를 저장한다.
react에서는 자동으로 key 를 저장하고 호출해서 쓴다. element 들을 표시할 때
목록의 unique 한 값으로 key 를 사용하면 좋다.


## Implementing Time Travel
move list 에 key 로 move 를 입력하고
jumpTo function 을 추가한다.
jumpTo 에서는 변경하려는 move 로 stepNumber 를 변경하도 play 순서를 변경한다.


[tutorial](https://reactjs.org/tutorial/tutorial.html)
[react/quickstart](https://reactjs.org/docs/hello-world.html)
