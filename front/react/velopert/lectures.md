
[강좌 리스트](https://velopert.com/reactjs-tutorials)

# React 강좌 01: 소개 및 맛보기
## React?
React 는 페이스 북에서 개발한 라이브러리로 개발자로 하여금 재사용 가능한 UI를 생성할 수 있게 해준다.
이 라이브러리는 Virtual DOM 이라는 개념을 사용해 상태의 변화에 따라 선택적으로 렌더링한다. 따라서, 최소한의 DOM 처리로 컴포넌트들을 업데이트 할 수 있다.

## Virtual DOM
DOM은 Document Object Model 의 약자로 객체를 통해 구조화된 문서를 표현하는 방법이다. XML 혹은 HTML 로 작성된다.
웹 브라우저는 이 DOM 을 활용하여 객체에 JavaScript 와 CSS를 적용한다. DOM 은 트리 형태로 되어있어서,
특정 node를 찾을 수도 있고, 수정할 수도 있고, 제거하거나 원하는 곳에 삽입할 수도 있다.

### DOM 의 문제점
DOM 은 동적 UI 에 최적화 되어 있지 않다.
HTML은 정적이다. (자바스크립트나 jQuery 를 사용해 손을 볼 수 있지만..)

트위터, 페이스북, 인스타 등 규모가 큰 앱 (데이터들이 많은 앱)은 스크롤을 좀 내리다 보면
수많은 데이터가 로딩된다. 그리고 각 데이터들을 표현하기 위해서는 수많은 요소(element) 들이 필요하다.
(예: 페이스북에서 포스트 한 개를 표현할 때 사용되는 요소의 개수는 약 100개이다.)

이렇게 규모가 큰 앱에서 DOM 에 직접 접근하여 변화를 주면 성능에 이슈가 발생한다.
그 이유는 DOM 에서 변화가 발생하면 브라우저는 CSS를 다시 연산하고 레이아웃을 구성하고 웹페이지를 리페인트 하게
되는데 이 때 시간이 허비된다. (reflow : 레이아웃을 새로 구성하면서 계산, repaint : 색상변경)

따라서 성능 개선을 하기 위해서는 reflow 횟수를 줄이기 위해 코드를 최적화 해야한다.

### 해결법
최소한의 DOM 조작을 통해서 작업을 처리하는 방식으로 개선을 할 수 있다.
이를 위해 DOM 작업을 가상화하는 방법을 사용하게 됨

Virtual DOM 을 사용하면, 실제 DOM 이 아닌 이를 추상화 시킨 자바스크립트 객체를 구성하여 사용한다.

React 에서 데이터가 변하여 브라우저 상에 업데이트 할 때는 다음과 같은 절차를 밟는다.
1. 데이터가 업데이트되면, 전체 UI 를 Virtual DOM 에 리렌더링 합니다.
2. 이전 Virtual DOM 에 있던 내용과 현재의 내용을 비교합니다.
3. 바뀐 부분만 실제 DOM 에 적용이 됩니다.

결국 컴포넌트가 업데이트 될 때, 레이아웃 계산이 한번만 이뤄진다.

React 는
> 우리는 다음 문제를 해결하기 위해 React를 만들었습니다:
 지속해서 데이터가 변화하는 대규모 애플리케이션을 구축하기.

와 같은 환경에서 사용하는 것이 좋다.

# 작업환경 설정하기
## Global Package 설치
작업 환경 설치를 위해서는 몇 가지 글로벌 패키지가 설치 되어야 한다.
- babel : ECMAScript6 Syntax 를 사용할 수 있게한다.
- webpack : 모듈 번들러로서 브라우저 위에서 import(require)를 할 수 있게 하고 자바스크립트 파일을 하나로 합쳐준다.
- webpack-dev-server : 개발서버로 별도의 서버를 구축하지 않고 웹 서버를 열 수 있다. (hot-loader)
```sh
npm install -g babel webpack webpack-dev-server
```

## 프로젝트 생성
```sh
npm init  // package.json
```

## Dependency 및 Plugin 설치
```sh
npm install --save react react-dom
npm install --save-dev babel-core babel-loader babel-preset-react babel-preset-es2015 webpack webpack-dev-server
```

## 디렉토리 구조 이해 및 파일 생성
react-tutorial
├── package.json         
├── public            # 서버 public path
│   └── index.html    # 메인 페이지
├── src               # React.js 프로젝트 루트
│   ├── components    # 컴포넌트 폴더
│   │   └── App.js    # App 컴포넌트
│   └── index.js      # Webpack Entry point
└── webpack.config.js # Webpack 설정파일

## 컴파일러, 서버 및 로더 설정
webpackconfig.json
```js
module.exports = {
    entry: './src/index.js',

    output: {
        path: __dirname + '/public/',
        filename: 'bundle.js'
    },

    devServer: {
        inline: true,
        port: 7777,
        contentBase: __dirname + '/public/'
    },

    module: {
            loaders: [
                {
                    test: /\.js$/,
                    loader: 'babel-loader',
                    exclude: /node_modules/,
                    query: {
                        cacheDirectory: true,
                        presets: ['es2015', 'react']
                    }
                }
            ]
        }
};
```
package.json
```js
"scripts": {
    "start": "webpack-dev-server --hot --host 0.0.0.0"
  },
```


# JSX
XML-like Syntax 를 native Javascript 로 변환해준다.
자바스크립트에서 html 태그를 사용할 수 있다.
## Nested Elements
컴포넌트에서 여러 Element 를 렌더링 해야 할 때, element 들을 필수적으로
container element 안에 포함시켜줘야 한다.
## JavaScript Expression
`{}` 로 wrapping 하면 된다.
```js
sayHey(){
       alert("hey");
    }

render(){
    let text = "Dev-Server"
    return  (
        <div>
            <h1> Hello Velopert </h1>
            <h2> Welcome to {text}</h2>
            <button onClick={this.sayHey}>Click Me</button>
        </div>
    );
}
```
## If-Else 문 사용 불가
JSX 안에서는 If-Else 문이 사용불가하다 ternary 표현을 사용해야 한다.
( condition ? true : false )
## Inline Style
string 형식이 사용되지 않고 key 가 camelCase 인 object 를 사용한다.
pStyle 을 p element 에 적용할 때,
```js
 render(){
        let text = "Dev-Server";

        let pStyle = {
            color: 'aqua',
            backgroundColor: 'black'
        };

        return  (
            <div>
                <h1> Hello Velopert </h1>
                <h2> Welcome to {text}</h2>
                <button onClick= {this.sayHey}>Click Me</button>
                <p style = {pStyle}>{1 == 1 ? 'True' : 'False'}</p>
            </div>
        );
    }
```

# style 과 className
style 을 객체 형식으로 작성한 후,  render 함수에서 호출할 수 있다
```js
render() {
    const style = {
      backgroundColor: 'black',
      padding: '16px',
      color: 'white',
      fontSize: '12px'
    };

    return (
      <div style={style}>
        hi there
      </div>
    );
  }
```

클래스를 설정할 때는 class 대신에 className 을 사용한다.
```css
.App {
  background: black;
  color: aqua;
  font-size: 36px;
  padding: 1rem;
  font-weight: 600;
}
```
component 에서 호출
```js
render() {
    return (
      <div className="App">
        리액트
      </div>
    );
  }
```


# Component 생성 및 모듈화
컴포넌트는 React.Component 클래스를 상속하여 만든다.

한 파일엔 여러개의 컴포넌트가 존재할 수 있다.

Component들을 모듈화하여 여러 파일로 분리해서 사용한다.
Component 를 구현할 때 의 일반적인 흐름
- Form.js,
- Form.css 작성
- Component 호출
- 렌더링
## 클래스형 컴포넌트
Component 를 상속받아 class 구현
## 함수형 컴포넌트
props 만 받아서 보여주는 경우엔 함수 형태로도 사용이 가능하다.
props 받아서 `<div>... </div>` return


# State 와 Props 사용하기
## props
컴포넌트에서 사용할 데이터 중 변동되지 않는 데이터를 다룰 때 사용
child 컴포넌트에서 parent 컴포넌트로부터 데이터를 받을 때 props 를 사용한다.
### 사용 방법
child 컴포넌트에서 사용할 때,
`{ this.props.propsName }`
형식으로 사용하고, parent 에서 child 컴포넌트에 넘길때는
`< > 괄호 안에 propsName="value"`
형식으로 사용한다.
### defaultProps
defaultProps 를 설정할 수 있다.
```js
static defaultProps = {
    name: '기본이름'
}

classNmae.defaultProps = {
  name: '기본이름'
};
```
### 기본 값 설정하기
props 를 지정해주지 않았을 때, 사용하는 기본값 설정
className.defaultProps = { propName: value }
### Type Validate 하기
컴포넌트에서 원하는 props 의 Type과 전달 된 props 의 Type 이 일치하지 않을 때 콘솔에서 오류 메시지가 나타나게 하고 싶으면, 컴포넌트 클래스의 propTypes 객체를 설정하면 된다.
className.propTypes = { propName: type }
[type validate](https://reactjs.org/docs/components-and-props.html)

## State
컴포넌트에서 유동적인 데이터를 다룰 때, state를 사용한다.
React.js 어플리케이션을 만들 땐, state를 사용하는 컴포넌트의 갯수를 최소화 하는 것을 노력해야 한다. 예를 들어, 10개의 컴포넌트에서 유동적인 데이터를 사용할게 될 땐, 각 데이터에 state를 사용할 게 아니라, props 를 사용하고 10개의 컴포넌트를 포함시키는 container 컴포넌트를 사용하는 것이 효율적이다.

state 의 초기값을 설정할 때는 생성자에 this.state={ } 를 사용한다.
렌더링 할 때는 { this.state.stateName } 을 사용한다.

### SetState
state 를 업데이트 할 때는 this.setState() 메소드를 사용한다.
setState 는, 객체로 전달되는 값만 업데이트를 해줍니다.
이런 state가 있다고 할 때
```js
state = {
  number: 0,
  foo: 'bar'
}
```
이렇게 호출하면
```js
this.setState({ number: 1 });
```
foo 는 그대로 남고 number 만 업데이트 된다.
하지만 깊숙한 곳까지는 확인하지 못한다.
```js
state = {
    number: 0,
    foo: {
      bar: 0,
      foobar: 1
    }
  }
```
foobar 를 업데이트 해주려면 자바스크립트의 전개 연산자를 사용한다.
```js
this.setState({
  number: 0,
  foo: {
    ...this.state.foo,
    foobar: 2
  }
});
```
기존의 state 값을 가져오려면 다음과 같이 호출한다.
```js
this.setState(
  (state) => ({
    number: state.number
  })
);
```
state 를 변경할 때 기존의 객체는 건들이지 않고 하려면 state 구조가 복잡할 경우 작업이 번거롭다.
이러한 작업을 쉽게 해줄 수 있는 것이 바로 Immutable.js 입니다!




### 메소드 작성
ES6 에선 setState 메소드를 사용하게 될 메소드를 bind 해주어야한다
this.methodname = this.methodname.bind(this);

state 를 변경하면 자동으로 render 한다.

# 리액트에서 배열 다루기
state 내부의 값을 직접 수정하면 안된다.
push, splice, unshift, pop 내장함수는 배열 자체를 직접 수정하므로 안된다
기존의 배열에 기반하여 새 배열을 만들어내는 함수인 concat, slice, map, filter 같은 함수를 사용해야합니다
concat - 배열 이어 붙이기

## 컴포넌트 Iteration (반복) - Map
map() 메소드는 파라미터로 전달 된 함수를 통하여 배열 내의 각 요소를 프로세싱 하여 그 결과로 새로운 배열을 생성한다.

arr.map(callback, [thisArg])
파라미터 설명
callback 새로운 배열의 요소를 생성하는 함수
  - currentValue 현재 처리되고 있는 요소
  - index 요소의 index 값
  - 메소드가 불려진 배열
thisarg callback 함수 내부에서 사용할 this 값 설정

```js
var numbers = [1, 2, 3, 4, 5];
var processed = numbers.map(function(num){
    return num*num;
});
// ES6
let numbers = [1, 2, 3, 4, 5];
let result = numbers.map((num) => {return num*num});
```

## 컴포넌트 mapping
데이터 배열을 mapping 하여 컴포넌트 배열로 변환하는 과정을 살펴보도록 하겠습니다.

row 에 해당하는 Component 를 만들고
```js
class ContactInfo extends React.Component {
    render() {
        return(
            <li>{this.props.name} {this.props.phone}</li>
            );
    }
}
```
데이터 배열을 가져와 map() 함수를 호출해 이를 반복한다.
```js
 render(){
        return(
            <div>
                <h1>Contacts</h1>
                <ul>
                    {this.state.contactData.map((contact, i) => {
                        return (<ContactInfo name={contact.name}
                                            phone={contact.phone}
                                              key={i}/>);
                    })}
                </ul>
            </div>
        );
    }
```

# State 내부 Array 에 원소 삽입/제거/수정
강좌 제거로 pass, 다은 강좌 살펴보기
추가는 concat을 사용해서,
수정은 배열 복사 후, 변경해서 변경,
제거는 filter를 사용한다


# Input 상태 관리하기
onChange 이벤트가 발생하면, e.target.value 값을 통하여 이벤트 객체에 담겨있는 현재의 텍스트 값을 읽어올 수 있습니다
이를 state 에 set하면 된다.
```js
...
onChange={this.handleChange}
...
handleChange = (e) => {
    console.log(e.target.value)
    this.setState({
      [e.target.name]: e.target.value
    })
  }
```

# 부모 컴포넌트에 정보 전달하기
부모 컴포넌트에서 메소드를 생성하고 props 로 전달한다.
