# vuepress

## init
```bash
yarn create vuepress-site {directoryName}
```

## package 설치
```bash
yarn
```

## configuration
- docs/.vuepress/config.js : entry file of configuration

## page routing
default page routing 은 다음과 같다.
|Relative Path|Page Routing|
|---|---|
|/README.md|/|
|/guide/README.md|/guide/|
|/config.md|/config.html|

폴더 트리구조를 따라 간다.

## Multiple Sidebars
sidebar 의 theme 설정을 변경
보여주려는 sidebar 구조를 디렉토리 형태로 만든다.
*디렉토리 구조*
```
.
├─ README.md
├─ contact.md
├─ about.md
├─ foo/
│  ├─ README.md
│  ├─ one.md
│  └─ two.md
└─ bar/
   ├─ README.md
   ├─ three.md
   └─ four.md
```
`.vuepress/config.js` 설정을 수정한다.
```js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: {
      '/foo/': [
        '',     /* /foo/ */
        'one',  /* /foo/one.html */
        'two'   /* /foo/two.html */
      ],

      '/bar/': [
        '',      /* /bar/ */
        'three', /* /bar/three.html */
        'four'   /* /bar/four.html */
      ],

      '/baz/': 'auto', /* automatically generate single-page sidebars */

      // fallback
      '/': [
        '',        /* / */
        'contact', /* /contact.html */
        'about'    /* /about.html */
      ]
    }
  }
}
```
여기서 fallback 은 가장 나중에 설정해야 한다.


# ref
https://vuepress.vuejs.org/guide/getting-started.html#prerequisites