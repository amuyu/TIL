
# velopert - router
## project structure
- components : react 컴포넌트
- pages : 각 라우트들이 위치하는 디렉토리
- client : 브라우저에서 사용할 최상위 컴포넌트
- server : 서버 측에서 사용할 코드 (서버사이드 렌더링)
- shared : 서버와 클라이언트에서 공용으로 사용되는 컴포넌트 App.js
- lib : 웹 연동을 구현할 때 API 와 코드 split 할 대 필요한 코드

## NODE_ENV 설정
코드를 불러올 때 '../components/Something' 이런 식으로 불러와야 하는 코드를 'components/Something' 으로 불러올 수 있도록 
프로젝트 루트 경로를 설정함
```json
"scripts": {
    "start": "cross-env NODE_PATH=src react-scripts start",
    "build": "cross-env NODE_PATH=src react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
}
```

## Root 컴포넌트
Router 컴포넌트를 사용하기 위해 `BrowserRouter` 를 적용하기 위한 컴포넌트, `index.js` 에서 Root 를 호출
```js
const Root = () => (
    <BrowserRouter>
        <App/>
    </BrowserRouter>
);
```

====

## 기본 라우트
라우트 컴포넌트들을 `pages` 에 구현한다.
컴포넌트들을 한 파일로 내보낼 수 있도록 `index` 를 구현한다.
라우트에 맞춰서 컴포넌트를 보여주도록, App 컴포넌트를 수정한다. - Route 컴포넌트 사용
```js
import React, { Component } from 'react';
import { Route } from 'react-router-dom';
import { Home, About } from 'pages';


class App extends Component {
    render() {
        return (
            <div>
                <Route exact path="/" component={Home}/>
                <Route path="/about" component={About}/>
            </div>
        );
    }
}

export default App;
```

====

## 라우트 파라미터 읽기
라우트의 경로에 특정 값을 넣는 방법을 알아보겠습니다.
방법은 두 가지가 있는데, `params` 를 사용하는 것과 `query` 를 사용하는 것입니다.

라우터로 설정하는 컴포넌트는 3가지의 props 를 전달받는다.
- history : push, replace 를 통해 다른 경로로 이동하거나 앞 뒤 페이지로 전환할 수 있다.
- location : 현재 경로에 대한 정보를 지니고 있다.
- match : 어떤 라우트에 매칭 되어 있는지 정보가 있고 params 정보를 가지고 있다.

### params
Route component path 를 다음과 같이 수정해서 사용한다. `/about/:name`
```js
<Route path="/about/:name" component={About}/>
```
`Switch` 컴포넌트를 사용해서 첫번째 라우트만 보여주고 나머지는 보여주지 않도록 할 수 있다.

### query
라우터 v3 에서는 URL 쿼리를 해석해서 객체로 만들어주는 기능이 자체적으로 있었는데, 개발자들이
여러가지 방식을 사용할 수 있도록 더이상 내장하지 않는다.
`query-string` 라이브러리를 사용해서 해석
`?detail=true` 라는 쿼리를 받는다고 하면 다음과 같이 확인할 수 있다.
```js
const About = ({ location, match }) => {
    const query = queryString.parse(location.search);
    const detail = query.detail === 'true';
    ...
};
```

====


## 라우트 이동하기
앱 내에서 다른 라우트로 이동할 때에는 `Link` 컴포넌트를 사용해야 합니다.
menu 라는 컴포넌트를 만들면 다음과 같다.
```js
import React from 'react';
import { Link } from 'react-router-dom';

const Menu = () => {
    return (
        <div>
            <ul>
                <li><Link to="/">Home</Link></li>
                <li><Link to="/about">About</Link></li>
                <li><Link to="/about/foo">About Foo</Link></li>
            </ul>
            <hr/>
        </div>
    );
};

export default Menu;
```
`Root` 컴포넌트에 `Menu` 컴포넌트를 추가한다.
`NavLink` 컴포넌트를 사용해서도 이동할 수 있다. URL 이 활성화가 되면 특정 스타일 혹은 클래스를 지정할 수 있다.

====

## 라우트 속의 라우트
이전 버전에서는 모든 라우트는 최상위에서 정해야 했지만 v4 에서는 컴포넌트 내부에 또 Route 를 사용할 수 있게 했다.
다음과 같은 방법으로 사용 가능하다
```js
const Posts = ({match}) => {
    return (
        <div>
            <h2>Post List</h2>
            <ul>
                <li><Link to={`${match.url}/1`}>Post #1</Link></li>
                <li><Link to={`${match.url}/2`}>Post #2</Link></li>
                <li><Link to={`${match.url}/3`}>Post #3</Link></li>
                <li><Link to={`${match.url}/4`}>Post #4</Link></li>
            </ul>
            <Route exact path={match.url} render={()=>(<h3>Please select any post</h3>)}/>
            <Route path={`${match.url}/:id`} component={Post}/>
        </div>
    );
};
```

====




# ref
[리액트 라우터 사용해보기](https://velopert.com/3417)
- 리액트 라우터 사용해보기
- 코드 스플리팅 사용하기
- 서버사이드 렌더링 해보기
