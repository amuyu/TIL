# apache와 nginx
# 1
nginx+wsgi
아파치+mod_wsgi
nginx + 아파치 + mod_wsgi
[장고 서버는 뭘로 돌리나요](https://fbdjango.wordpress.com/2011/08/10/183432721693235_233193713383802/)

# WSGI
Web Server Gateway Interface
웹서버와 웹어플리케이션이 어떤 방식으로 통신하는가에 관한 인터페이스
WSGI 어플리케이션은 uWSGI라는 컨테이너에 담아 어플리케이션을 실행하게 되며, uWSGI가 각각의 웹서버와 소통하도록 설정하면 끝이다

# uwsgi 와 gunicorn 비교
## 1
- 보통 퍼포먼스를 보면 아무래도 pure C기반의 uwsgi가 pure python인 gunicorn보다 빨라보입니다. 간혹 uwsgi가 느리다는 비교가있는데 이는 gunicorn은 워커를 gevent로 하면 몽키패치를 지가 app서버에 해버리는데 반해 uwsgi는 app서버 코드에 몽키패치를 해줘야함을 모르고 테스트할 결과로 보여지네요.
- 사용성 측면에서 uwsgi가 좀더 복잡하고 코어 갯수에 따라 configure를 잘못하면 오히려 성능저하가될 우려가 보이네요.
- 도큐먼트는 gunicorn이 훨깔끔
출처: http://devspark.tistory.com/entry/gunicorn-vs-uwsgi [개발자 이야기]


## 2
uWsgi 를 선호한다는 이야기
아파치가 nginx 보다 설정이 쉽다는 이야기
[Django 로 서비스할 때 gunicorn과 uwsgi 어떤 걸 보통 쓰시나요? 그리고](https://fbdjango.wordpress.com/2014/12/24/183432721693235_783189061717595/)
