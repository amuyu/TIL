# react todo-list

main? section?  JSX 사용
jsx.global

flex

색상 참조 할 때 사용
[open-color](https://yeun.github.io/open-color/)

flex-study game
[flex game](http://flexboxfroggy.com/#ko)


[flex 생활 코딩](https://opentutorials.org/course/2418/13526)

# 다양한 방식의 리액트 컴포넌트 스타일링 방식
https://velog.io/@velopert/react-component-styling
- 컴포넌트 하나마다 Sass 파일 하나씩 만들어서 
- styled-components 사용

## 그냥 css 사용
css 작성 시, 가장 중요한 점은 중복되는 클래스명 생성하지 않기
그 방법에는 네이밍, css selector 사용이 있음
### naming
- 컴포넌트-클래스 
- BEM Naming : 하나의 파일에 몰아서 작성할 때 편리
- CSS Selector : 최상위 요소 > 하위 요소

## Sass 사용
sass 는 css pre-processor 로서 복잡한 작업을 쉽게 할 수 있게 하고, 코드의 재활용성, 코드의 가독성을 높여준다.
`.scss`, `.sass` 가 있음.... scss 를 많이 쓴다고 함
`node-sass` 라는 라이브러리를 설치 해야 

scss import 시, path 가 길어지는 부분을 해결하기 위해서는 
webpack 에서 `sass-loader` 설정을 커스터마이징하여 해결할 수 있음, 
하지만 커스터마이징 하려면 `yarn eject` 를 통해서 세부설정을 꺼내주어야 한다.

자주 사용하는 Sass 라이브러리 중에서 반응형 디자인을 쉽게 해주는 `include-media`와
색상 팔레트인 `open-color` 가 있음
yarn add open-color include-media
```js
@import '~include-media/dist/include-media';
@import '~open-color/open-color';
```

## CSS Module
파일이름_클래스이름__해쉬값 형태로 클래스네임을 자동으로 고유한 값으로 만들어 사용
[파일이름].module.css 로 파일을 작성해야함


## classNames 
CSS 클래스를 조건부로 설정, CSS Module 을 사용할 때 함께 사용하면 좋다
`classnames/bind` 를 사용해서 사용할 수 있다
```sh
yarn add classnames
```

Sass 를 사용할 때도 파일이름 뒤에 `.module.scss` 를 입력하면 CSS Module 로 사용가능하다

## styled-components
현존하는 리액트 css-in-js 관련 라이브러리 중에서 가장 잘나가는 라이브러리
자바스크립트 파일안에서 스타일까지 작성



