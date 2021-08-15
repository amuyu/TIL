
# overvieww
- 리액트 컴포넌트 타입스크립트로 작성하기


# 리액트 컴포넌트 타입스크립트로 작성하기
## 프로젝트 생성
새로 프로젝트를 만들 때,
```shell
npx create-react-app ts-react-tutorial --typescript
npx create-react-app ts-react-tutorial --template typescript
```
typescript 를 추가할 때,
```shell
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
# or
yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

타입스크립트를 사용하는 리액트 컴포넌트는 `*.tsx` 확장자를 사용

## component 만들기
간단한 component 만들기
```js
import React from 'react';

type GreetingsProps = {
  name: string;
};

const Greetings: React.FC<GreetingsProps> = ({ name }) => (
  <div>Hello, {name}</div>
);

export default Greetings;
```

React.FC 를 사용할 때, props 타입을 Generics 로 넣어서 사용한다.

React.FC 를 사용해서 얻을 수 있는 이점은 두 가지가 있다.
첫번째는, props 에 기본적으로 children 이 들어가있다.
두번째는, 컴포넌트의 defaultProps, propTypes, contextTypes 를 설정할 때, 자동완성이 될 수 있다.

단점도 있다.
children 이 옵셔널 형태로 들어가 있다보니 컴포넌트의 props 의 타입이 명백하지 않다.
defaultProps 에 값이 들어오지 않으면 에러가 발생한다. 

> 이러한 이슈때문에 React.FC 를 쓰지 말라는 팁도 존재합니다. 이를 쓰고 안쓰고는 여러분의 자유이지만, 저는 사용하지 않는 것을 권장합니다.