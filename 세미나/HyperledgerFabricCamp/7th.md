
# node.js chaincode 개발
node.js 로 chaincode 를 개발하기 위해서는 아래 환경이 구성되야 한다.
## install
- node.js engine : LTS (>= 8.9.0, < 9.x.x)
- npm (>= v5.5.1)
- gulp : npm install -g gulp

## 프로젝트 초기화
node 개발을 위한 폴더 초기화
```sh
mkdir $cc_dir_name
npm init
```

## fabric-shim interface
체인코드 작성을 위해 필요한 라이브러리
node.js 환경에서는 package.json 에 dependency 추가
```json
"dependencies": {
    "fabric-shin": "~1.4.0"
}
```
다음 명령으로 패키지 설치
```sh
npm install
```

## 기본 구조
필수 패키지 import 명령
```js
const hsim = require('fabric-shim');
const util = require('util');
```
chaincode 본체, chaincode 기본 method 구현
```js
var Chaincode = class {
    // Init : chaincode 초기화
    // invoke : 함수 호출
};
}
```
chaincode 시작점
```js
shim.start(new Chaincode());
```


## 명령어
stub.getFunctionAndParameters() - func 호출 시, 함수명, 파라미터 정보를 가져올 수 있음
stub.getState() - state db 조회
stub.putState() - state db insert/update
stub.deleteState() - state db delete


# java chaincode 개발
## install
java : 공식 지원 버전 명시 x, 예제에서는 1.8 사용
java 는 build tool로 gradle 을 사용함

## fabric-shim interface
node.js 는 package.json 에 `fabric-shim` 을 추가했던것 처럼 java에서는 gradle 에 dependency를 추가함
```gradle
dependencies {
    compile group: 'org.hyperledger.fabric-chaincode-java', name: 'fabric-chaincode-shim', version: '1.+'
    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```

## 프로젝트 초기화
```sh
mkdir $cc_dir_name
gradle init
gradle build
```

## java chaincode 기본구조
node.js 와 유사
java 특성상 main method 가 class 파일 안에 있음
class 는 `ChaincodeBase` 를 상속받아 작성
init 과 invoke 를 override 함

## 명령어
node.js 와 유사하나 언어 특성이 있음
stub.getFunction() : 함수명 호출
stub.getParameters() : 파라미터 호출
stub.putStringState() : statedb 에 값 입력 (String type)
stub.getStringState() : statedb 에 값 조회 (String type)
stub.delState() : statedb 값 제거


# 참고
http://fabric-shim.github.io