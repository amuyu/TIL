sop : same origin policy
protocol, host, port





# cross domain 이슈 우회 하는 방법
- JSONP
- Reverse Proxy
- Flash Socket

# cors 의 등장
cross origin resource sharing
브라우저에서 외부 도메인 서버와 통신하기 위한 방식

## 동작 방식
- simple request
- prelight request
- auth request




# nginx 설정
[https://serverfault.com/a/431580](https://serverfault.com/a/431580)

## 1
```
add_header Access-Control-Allow-Headers *;
```

## 2
```
add_header Access-Control-Allow-Headers *;
add_header Access-Control-Allow-Origin *;
add_header Access-Control-Allow-Credential true;
add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
```

에러나는 경우에는 왜 그런지 모르겠지만 정상 응답을 주는 경우에는 cors 에러가 발생하지 않고 진행이 되네요