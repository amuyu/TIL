4:34]
굉장히 긴 expire date 를 가진 refresh token 과 유효기간이 짧은 access token 을 가지고 있고요.


[4:35]
access token 이 만료되면
refresh token 은 또다른 앱 내의 데이터와 조합해서 서버와 통신해서 새로운 access token 을 가져오죠.


[4:36]
access token 이 만료된 것은 보통 시간을 가지고 알아내거나
한번 통신했을 때 401 Authorization Error 가 떨어지면 Refresh Token 을 이용해서 갱신하도록 하지요


[4:37]
refresh token 이 없거나 invalid 상태면 로그인이 안된 것으로 간주 하죠


[4:37]
보통은 서버에서 device 당 1개씩 refresh token 을 발급해서 관리해요.
