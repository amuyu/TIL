
php...
# Vue SSR vs PreRenderer
검색엔진에 노출시키는 과정 설명

# 서비스
Vue + Vue Router + Vuex
SPA 으로 리팩토링하였습니다.

## 히스토리 느낌 구성
### 1
SPA 로 해서 검색엔진에 노출이 안된다.
그래서 SSR 이 필요하다!!!!
SSR (Server Side Rendering)
ssr.vuejs.org/ko
ssr을 하려면 서버에서 자바스크립트를 동작시켜야 한다.

### 2
Php 를 사용하고 있어서 어떻게 해야하지...?? Node.js 로 다 변경해야 한다.
멘붕..

### 3
특정 몇 페이지가 검색엔진 노출이 안되는 상황
`서버 사이드 렌더링 vs 사전 렌더링`
몇 가지 마케팅 페이지 개선을 위해서 사용한다면 사전 렌더링을 써라..
Prerendering


### 4
prerender 에서 발생할 수 있는 문제가 있다.
로그인에 따라 값이 바뀌는 경우 문제가 된다. (페이지 호출 후에 상태가 변경되서 view가 깜빡인다.)
화면이 깜빡이는 것보다 차라리 늦게 뜨는게 낫다....
`webpack.js.org/plugins/define-plugin` 을 찾음


## SSR (Server Side Rendering)
### Node.js
vue 에서 제공하는 문서보고 한땀한땀 구현
Nuxt 라는 라이브러리 추천              
### 비 Node.js
v8 엔진 모듈 사용
헤드리스 브라우저 사용 ,,,, 서버에 브라우저를 띄워놓고 호출한 뒤 클라이언트로 전송



## Prerendering
1. 소스를 미리 빌드하고 헤드리스 브라우저에서 동작시킨다.
2. Phantom.js 에서 동작시키고 그 결과를 html 로 저장한다.
3. html 을 서버사이드 쪽 템플릿 파일로 사용한다.
4. 서버 운영
1~3 을 webpack으로 구현해야 한다.



## 참고
[샘플 코드](https://github.com/wan2land/gdg171021-vue-ssr-vs-prerender)
