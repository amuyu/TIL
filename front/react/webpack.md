# 모듈 시스템 - webpack
javascript 모듈화 명세를 만드는 대표적인 작업 그룹에 `CommonJS` 와 `AMD(Asynchronous Module Definition)` 이 있다.
`webpack` 은 두 그룹의 명세를 모두 지원하는 Javascript 모듈화 도구다.

# 모듈 정의와 모듈 사용
모듈을 작성하고 다음과 같이 module.exports 속성에 외부에 배포할 모듈을 전달해 모듈을 정의한다.
```js
module.exports = {message: 'webpack'};
```
모듈을 사용할 때는 모듈을 로딩하는 파일의 require() 함수에 로딩할 모듈의 경로를 전달한다.
```js
require('./examplemodule');
```
여러 모듈을 합치거나 중첩해서 모듈을 로딩할 수도 있다.
```js
// hello.js
module.exports = 'Hello';  
// world.js
module.exports = 'World';  
// greeting.js
var greeting = require('./hello') + require('./world');
module.exports = greeting;  
```
모듈을 정의하고 모듈을 사용하도록 로딩하는 방법은 어렵지 않다. 다만 모듈로 만든 파일은 바로 웹페이지에 넣어 브라우저에서 실행할 수 없다.
webpack 으로 컴파일해 브라우저에서 실행할 수 있는 형태로 바꿔야 한다.

====

# webpack 사용 방법
webpack 에 설치되면 다음과 같이 명령어를 실행해 모듈을 컴파일한다.
```js
webpack ./entry.js bundle.js
```

엔트리 파일은 의존 관계에 있는 다양한 모듈을 사용하는 시작점이 되는 파일이다.
번들 파일은 브라우저에서 실행할 수 있게 모듈을 컴파일한 파일이다.

webpack 에서 컴파일은 엔트리 파일을 시작으로 의존 관계에 있는 모듈을 엮어서 하나의 번들 파일을 만드는 작업이다.
Javasript 를 사용하는 HTML 코드에서는 컴파일 결과로 만들어진 번들 파일만 포함하면 된다.

====

# 모듈의 스코프
컴파일 과정에서 각 모듈은 함수로 감싸진다. 모듈을 작성할 때 모듈의 변수가 전역 변수가 되는 것을 피하려 함수로 변수를 감쌀 필요가 없다.

====

# 설정 파일 사용
엔트리 파일이 많거나 옵션이 많으면 입력하기 불편하기 때문에 설정 파일을 만들어 컴파일하면 이런 불편을 줄일 수 있다.
설정 파일의 기본 형태는 다음과 같다.
```js
// webpack.config.js
module.exports = {  
    context: __dirname + '/app', // 모듈 파일 폴더
    entry: { // 엔트리 파일 목록
        app: './app.js' 
    },
    output: {
        path: __dirname + '/dist', // 번들 파일 폴더
        filename: '[name].bundle.js' // 번들 파일 이름 규칙
    }
}
```

====

# 로더
webpack 의 로더는 다양한 리소스를 Javascript 에서 바로 사용할 수 있는 형태로 로딩하는 기능이다.

예를 들어서, 어떤 템플릿 html 파일을 로드한다면, 다음과 같이 구현할 수 있다.
템플릿 로더로 `handlebars-loader` 를 사용한다.

webpack.config.gs 설정
```js
module.exports = {  
    ...
    output : {
        ...
    },
    module : {
        loaders : [
        // 적용할 파일의 패턴(정규표현식)과 적용할 로더 설정
        {
            test : /\-tpl.html$/,
            loader : 'handlebars'
        }]
    }
}
```
템플릿 파일은 다음과 같다. example-tpl.html
```html
<div>{{greeting}}</div> 
```
모듈에서는 다음과 같이 작성한다.
```js
var listTpl = require('./example-tpl.html');  
listTpl( { greeting: 'Hello World' } );  
```

handlebars-loader 를 활용하면 템플릿 파일을 별도의 HTML 파일로 관리할 수 있어 유지와 보수가 쉬워진다.




# ref
[Javascript 모듈화 도구, webpack](https://d2.naver.com/helloworld/0239818)
[Javascript 표준을 위한 움직임](https://d2.naver.com/helloworld/12864)
[webpack](https://webpack.js.org)