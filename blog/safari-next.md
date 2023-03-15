React 구동 후, Safari 에서 만나는 에러들

로컬에서 next 구동 시, 다음과 같은 에러가 나면서 접속이 안된다

> Refused to connect to ws://192.168.10.122:3000/_next/webpack-hmr because it appears in neither the connect-src directive nor the default-src directive of the Content 
Security Policy.

가이드 하는대로 ContentSecurityPolicy 에 설정을 추가하면 된다
```
// connect-src or default-src
connect-src ws://192.168.10.122:3000 http://192.168.10.122:3000;
```


> Unhandled Promise Rejection: null
