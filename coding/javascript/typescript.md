## typescript 빌드
typescript 설치
```sh
npm install typescript
```
tsconfig.json 작성
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "outDir": "src/library",
    "rootDir": "ts",
    "sourceMap": true,
    "target": "es2015",
    "jsx": "react",
    "allowJs": true
  },
  "include": [
    "./ts/**/*"
  ]
}
```
vscode 에서 tscwatch 실행
.vscode 폴더에 tasks.json 작성
```json
{
  "version": "0.1.0",
  "command": "tsc",
  "isShellCommand": true,
  "args": [
    "-w",
    "-p",
    "."
  ],
  "showOutput": "silent",
  "isBackground": true,
  "problemMatcher": "$tsc-watch"
}
```
메뉴에서 작업 > 실행 하면 ts 코드가 변경될 때마다 컴파일 됨




## build
`awesome-typescript-loader` : 웹팩이 ts 표준 설정파일인 tsconfig.json을 활용해서 컴파일하게 도와주는 로더
`source-map-loader`는 웹팩이 여러 라이브러리를 관리하는 과정에서 소스맵 데이터의 연속성을 유지하기 위해 필요하다. 소스맵이 변경될때마다, 웹팩에게 알려준다.

index.d.ts :외부 모듈의 타입 정보를 d.ts 파일에 작성하는 방법

## migrating javascript



[타입스크립트 코리아 : React with TypeScript 세미나](https://www.inflearn.com/course/react-with-typescript/)
[타입스크립트 코리아 : 기초 세미나](https://www.inflearn.com/course/타입스크립트-코리아-1705-기초-세미나/)
[TypeScript React Conversion Guide](https://github.com/Microsoft/TypeScript-React-Conversion-Guide)
[react-typescript](https://github.com/codebusking/workshop-react-typescript)
[Migrating from JavaScript to TypeScript](https://engineering.huiseoul.com/migrating-from-javascript-to-typescript-5f32a81099e4)
[Migrating from JavaScript](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html)
[typescript로 npm 패키지 만들기](https://rhostem.github.io/posts/2017-08-14-typescript-npm-package/)
[Webpack & Typescript Env](https://github.com/udsdevteam/udt_kanban/wiki/Webpack-&-Typescript-Env)
[d.ts 작성하는 방법](https://www.slideshare.net/gloridea/dts-74589285)
[TypeScript-React-Conversion-Guide](https://github.com/Microsoft/TypeScript-React-Conversion-Guide#typescript-react-conversion-guide)
[타입스크립트 연습](https://react.vlpt.us/using-typescript/01-practice.html)