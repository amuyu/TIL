sop : same origin policy
protocol, host, port





# cross domain 이슈 우회 하는 방법
- JSONP
- Reverse Proxy
- Flash Socket

# cors 의 등장
cross origin resource sharing
브라우저에서 외부 도메인 서버와 통신하기 위한 방식

## cors 접근 제어 시나리오
1. preflight request 
`OPTIONS` 요청과 함께 서버에 다른 도메인의 리소스에 요청이 가능한지 확인 작업을 한다.
요청이 가능하다면 실제 요청을 보낸다.
```
Access-Control-Request-Method: POST	# 실제요청의 메서드
Access-Control-Request-Headers: X-PINGOTHER, Content-Type # 실제요청의 추가헤더
```
2. preflight response
서버가 메서드와 헤더를 받을 수 있음을 알려준다.
프리플라이트를 보내면 사전, 실제 요청 두번이 매번 왔다갔다 하므로 브라우저가 캐싱을 해두고 똑같은 요청을 보낼때, 사전 요청을 보내지 않고 바로 본 요청을 보낸다.
preflight request가 완료되면 실제 요청을 전송한다.
3. main request
실제 요청


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

# spring 설정
```java
@RestController
@RequestMapping("/account")
public class AccountController{
	
    @CrossOrigin
    @RequestMapping("/{id}")
    public Account retrieve(@PathVariable Long id){
    	// ...
    }
    
    
    @RequestMapping(method = RequestMethod.DELETE, value = "/{id}")
    public void remove(@PathVariable Long id){
    // ...
    }
}

@CrossOrigin(origins = "http://domain1.com, http://domain2.com")
@RequestController
@RequestMapping("/account")
public class AccountController{
	
    @RequestMapping("/{id}")
    public Account retrieve(@PathVariable Long id){
    	// ... 
    }
    
    @RequestMapping(method = RequestMethod.DELETE, value = "/{id}")
    public void remove(@PathVariable Long id){
    	// ...
    }
}
```


# ref
[The 'Access-Control-Allow-Origin' header contains multiple values](https://minholee93.tistory.com/entry/ERROR-The-Access-Control-Allow-Origin-header-contains-multiple-values)
[CORS, Preflight, 인증 처리 관련 삽질](https://www.popit.kr/cors-preflight-인증-처리-관련-삽질/)
[CrossOrigin Annotation 정리](https://velog.io/@modsiw/Spring-CrossOrigin-Annotation-정리)