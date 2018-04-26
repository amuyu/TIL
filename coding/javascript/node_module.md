# npm
`npm init` 을 사용하면 package.json 을 자동으로 생성해준다.

# babel
`babel-cli`, `babel-preset-env`, `babel-preset-statge-2`, `babel-plugin-transform-runtime`
## 설치
```sh
// install
npm install --save-dev babel-cli
```
## 설정
pakage.json 에 script 추가
es6 관련 에러가 발생해서 `babel-preset-statge-2`, `babel-plugin-transform-runtime` 추가했다
```json
{
  "name": "my-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src -d lib --presets=env,stage-2 --plugins=transform-runtime"
  },
  "devDependencies": {
    "babel-cli": "^6.0.0"
  }
}
```
## 실행
```sh
babel-node index.js --presets es2015
```
## build
```sh
npm run build
```


# readline
키보드 입력

# package.json
- bin : a map of command name to local file name.



## 참고
[npm으로 package.json 생성하기](https://blog.outsider.ne.kr/674)
[Node.js 모듈을 npm 저장소에 배포하기](https://blog.outsider.ne.kr/829)
[Node.js 프로젝트 시작하기](http://simsi6.tistory.com/29?category=258737)

### babel
[babel document](https://babeljs.io/docs/setup/#installation)
[babel-cli document](https://babeljs.io/docs/usage/cli/)
[babel 에 대한 모든것](https://jaeyeophan.github.io/2017/05/16/Everything-about-babel/)
[es6-npm-publish](https://booker.codes/how-to-build-and-publish-es6-npm-modules-today-with-babel/)
### command
[npm-name](https://github.com/sindresorhus/npm-name-cli)
[github/meow](https://github.com/sindresorhus/meow)
[github/ttystudio](https://github.com/chjj/ttystudio)
[앱추천](https://firebearstudio.com/blog/node-js-command-line-apps-utilities.html)
[github/node-progress](https://github.com/visionmedia/node-progress)
