
### jwt

JSON Web Token (JWT) https://tools.ietf.org/html/rfc7519

json 객체를 사용해서 정보를 안정성 있게 전달한다. 어떻게?

#### 자가 수용적 (self-contained)

토큰에 대한 기본정보, 전달할 정보, 토큰이 검증됐다는 증명(signature) 를 포함

#### usage

회원인증 : 유저가 로그인 하면 유저의 정보에 기반한 토큰을 발급해서 유저에게 전달 그 후, 유저가 서버에 요청을 할 때 마다 JWT 를 포함하여 전달하고 서버는 클라이언트에게서 요청을 받을때마다, 해당 토큰이 유효하고 인증됐는지 검증

정보교류 : 두 개체 사이에서 안정성있게 정보를 교환하기에 좋은 방법, 정보가 sign 되어있기 때문에 정보를 보낸 이가 바뀌진 않았는지 정보가 조작되지 않았는지 검증

#### 구조

header , payload, signature 3가지 부분으로 나뉨

```
header.payload.siganture
```

##### header

type : JWT

alg : 해싱 알고리즘, 보통 `HMAC sha 256`, `RSA` 를 사용 signature 에서 사용

##### payload

payload 에는 토큰에 담을 정보가 있고, 정보의 한 조각을 claim 이라고 한다.

registered, public, private 클레임이 있다.

- registered 클레임 (공통으로 사용하기 위한 이름)

iss : 토큰 발급자

sub : 토큰 제목

aud : 토큰 대상자

exp: 만료시간

ids : 토큰이 발급된 시간

jti : 고유식별자

- public 클레임

클레임 이름을 uri 형식으로 한다.

- private 클레임

클라이언트 <-> 서버 협의하에 사용하는 클레임

##### signature

헤더의 인코딩 값과 payload 의 인코딩 값을 합친후 비밀키로 해쉬한다.

해쉬 한후 `base64`	 형태로 사용한다

## 참고

[jwt 설명](https://velopert.com/2389)
[jwt online](https://jwt.io/)
