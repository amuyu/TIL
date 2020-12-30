# Vuepress Quick start
Vuepress 는 마크다운 문서를 html 로 자동으로 변환해 기술 문서 사이트를 빠르게 구축할 수 있는 서비스입니다. 이 문서에서는 `Vuepress` 로 프로젝트를 생성하고 작성하는 방법에 대해서 설명합니다.

## 프로젝트 생성
`create-vuepress-site generator` 를 사용해서 vuepress project 를 생성합니다.
이 패키지는 vuepress site 에 필요한 기본 디렉토리 및 파일들을 생성해줍니다.
```sh
## yarn
yarn create vuepress-site [optionalDirectoryName]
## npm
npx create-vuepress-site [optionalDirectoryName]
```
명령을 실행하면, VuePress's site metadata 를 입력하고 완료하면 프로젝트가 생성됩니다.
이 때, directoryName 은 입력하지 않으면 `docs` 로 생성됩니다.

*metadata*
- Project Name
- Description
- Maintainer Email
- Maintainer Name
- Repository URL

## 디렉토리 구조
프로젝트가 생성되면 다음과 같은 디렉토리 구조를 갖습니다.
기본적으로 node 패키지 구조를 갖고 있습니다. src 폴더 하위로 마크다운 문서들을 작성합니다. .vuepress 는 마크다운 문서들을 html 로 변환하기 위해 필요한 파일들입니다.
```
docs
├── package.json
└── src
    ├── .vuepress
    │   ├── components (Optional)
    │   ├── theme (Optional)
    │   │   └── Layout.vue
    │   ├── public (Optional)
    │   ├── styles (Optional)
    │   │   ├── index.styl
    │   │   └── palette.styl
    │   ├── templates (Optional, Danger Zone)
    │   │   ├── dev.html
    │   │   └── ssr.html
    │   ├── config.js (Optional)
    │   └── enhanceApp.js (Optional)
    ├── config
    │   └── README.md
    ├── guide
    │   ├── README.md
    │   └── using-vue.md
    └── index.md
```

## Page Routing
기본적으로 폴더가 하나의 uri path 가 되고 마크다운 문서가 하나의 html 로 변환됩니다.  
예를 들어 위 구조를 갖는 프로젝트의 경우, 다음과 같은 uri 경로를 갖습니다.

|Relative Path (src 가 root path)|Page Routing|
|---|---|
|/index.md|/|
|/guide/README.md|/guide/|
|/config/config.md|/config.html|

`/index.md` 나 `/guide/README.md` 에서 알 수 있듯이 README.md 파일이나 index.md 파일이 url 의 경로의 root로 routing 됩니다.

## 실행
다음과 같이 script 을 실행하면 개발모드로 실행할 수 있습니다.
```sh
# yarn
yarn dev
# npm
npm run dev
```
실행 후, 브라우저에서 `http://localhost:8080` 로 접속하면 site 를 볼 수 있으며, 마크다운 문서를 변경하면 바로 바로 reloading 되어 변경사항을 확인할 수 있습니다.


## Side bar 구성
왼쪽에 위치한 sidebar 의 메뉴는 `src/.vuepress/config.js` 파일의 `themeConfig.sidebar` 에서 설정할 수 있습니다.
`create-vuepress-site generator` 로 프로젝트를 생성하면 다음과 같이 sidebar를 설정합니다.
```js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    sidebar: [
        '/guide/': [
        {
          title: 'Guide',
          collapsable: false,
          children: [
            '',
            'using-vue',
          ]
        }
      ],
    ]
  }
}
```
`sidebar` 는 array 형태로 입력하는데 하나의 item 이 최상위 group 이 되게 됩니다.
위와 같이 설정하게 되면 guide 라는 이름을 가진 group 하나가 생성됩니다.
이 메뉴는 guide 폴더를 base 로 하게 되고 하위 메뉴로 guide 폴더의 `README.md` 파일과 `using-vue.md` 파일을 갖게 됩니다. (`''` == `README.me`)

하위 메뉴 이름은 마크다운 파일의 첫번째 Header(level 1에 해당하는) 로 셋팅이 되거나 
Header 가 없을 경우, 파일 이름이 표시됩니다. Guide 폴더의 `README.md`과 `using-vue.md` 의 경우, 각각 다음과 같은 header 로 시작하기 때문에 
```markdown
// README.md
# Introduction
// using-vuew.md
# Using Vue in Markdown
## Browser API Access Restrictions
```
side bar 가 다음과 같이 구성됩니다.
```
Guide
├── Introduction
└── Using Vue in Markdown 
```

하위 메뉴를 선택하면 선택한 메뉴와 맵핑되는 마크다운 문서가 변환된 html 이 contents 영역에 보이게 되고 sidebar 에는 마크다운 파일의 level2 에 해당하는 Header 들이 최하위 메뉴로 표시됩니다.
```
Guide
├── Introduction
└── Using Vue in Markdown 
   └─── Browser API Access Restrictions
```

## build
문서 작성이 완료 된 후, 배포하려면 다음 명령으로 빌드합니다.
```sh
#yarn
yarn build
#npm
npm run build
```
빌드 결과물이 생성되는 default 경로는 `src/.vuepress/dist` 이고 이를 변경하려면 `src/.vuepress/config.js` 파일에서 다음과 같이 설정할 수 있습니다.
```js
module.exports = {
  /**
   * Specify the output directory for `vuepress build`
   */
  dest: "dist",
}
```


# ref
[VuePress Document](https://vuepress.vuejs.org/guide/getting-started.html)