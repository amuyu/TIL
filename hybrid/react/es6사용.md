## babel
// https://velopert.com/814
```sh
npm install --save-dev babel-core babel-loader babel-preset-react babel-preset-es2015
```





```sh
npm install --save-dev babel-core babel-cli babel-preset-latest
```
.babelrc
```
{
  "presets": ["latest"]
}
```
package.json
```json
  "main": "./lib",
  "scripts": {
    "build": "babel src --out-dir lib"
  }
```

[react 에서 es6 사용](https://stackoverflow.com/a/42942214/6759520)
