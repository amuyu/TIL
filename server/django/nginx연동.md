
# uwsgi 설치
```sh
pip install uwsgi
```

# uwsgi로 djagon 앱 실행
```sh
uwsgi --http :8000 --module mysite.wsgi   # mysite 는 wsgi.py 가 있는 디렉토리
```
# nginx 설치
```sh
brew install nginx
```

# uwsgi_params 복사

# mysite_nginx.conf 작성
```nginx
# helloworld_nginx.conf

# upstream(proxy) 설정
upstream helloWorld {
    #1 uWSGI를 이용한 django 서버가 listening 할 ip주소와 port 번호를 적어주시면 됩니다. upstream에 다른 외부 서버를 연결할수도 있지만 여기서는 로컬에 있는 django에 보내니 주소가 127.0.0.1이고 포트는 8001로 설정하였습니다.
    server 127.0.0.1:8001;
    #server unix:///Users/amuyu/Documents/studyWorkspace/django-project/helloWorld/helloWorld.sock;
}

# configuration of the server
server {
    #2 django가 아니라 외부에서 어떤 port를 listening 할지 정해줍니다.
    listen      8000;
    #3 실행하는 서버의 IP주소 혹은 Domain을 적어주시면 됩니다.
    #4 이 server_name을 여러개 만들어서 subdomain도 각각 다르게 처리 가능합니다.
    server_name 127.0.0.1;
    charset     utf-8;

    # max upload size
    client_max_body_size 75M;   # adjust to taste

    #5 Django media파일 경로
    location /media  {
        alias /Users/amuyu/Documents/studyWorkspace/django-project/helloWorld/media;
    }

    #6 Django static파일 경로
    location /static {
        alias /Users/amuyu/Documents/studyWorkspace/django-project/helloWorld/static;
    }

    #7 media와 static을 제외한 모든 요청을 upstream으로 보냅니다.
    location / {
        #8 uwsgi_pass [upstream name](위에 uptream으로 설정한 block의 이름)
        uwsgi_pass  127.0.0.1:8001;
        #9 uwsgi_params의 경로
        include /Users/amuyu/Documents/studyWorkspace/django-project/helloWorld/uwsgi_params;
    }
}
```

# mysite_nginx.conf 링크

# nginx 실행

# uwsgi 실행
```sh
uwsgi --socket :8001 --plugin python --module helloworld.wsgi
```

# ini 작성
```ini
# mysite_uwsgi.ini file
[uwsgi]

# Django-related settings
# the base directory (full path)
chdir           = /path/to/your/project
# Django's wsgi file
module          = project.wsgi
# the virtualenv (full path)
home            = /path/to/virtualenv

# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 10
# the socket (use the full path to be safe
socket          = /path/to/your/project/mysite.sock
# ... with appropriate permissions - may be needed
# chmod-socket    = 664
# clear environment on exit
vacuum          = true
```


# nginx
Apache처럼 웹서버 환경을 만들어주는 소프트웨어입니다.
웹서버란 전체 서버 아키텍처의 앞단에서 HTTP 요청에 따라서 그 요청을 넘기고 그에 해당하는 file 혹은 리소스를 넘겨주는 역할을 하는 부분입니다.
brew install nginx
/usr/local/etc/nginx/nginx.conf
nginx
nginx -s stop
nginx -s reload

## 설정
/etc/nginx,/usr/local/nginx/conf, /usr/local/etc/nginx
sites-enabled와 nginx.conf를 통해서 nginx의 설정이 가능합니다.
모든 설정파일들을 건드려보지 않았기에 설정 할 때 필요한 몇가지 파일 및 폴더들만 보겠습니다.

nginx.conf : nginx와 그 모듈들이 작동하는 방식에 대한 설정 파일입니다. sites-enabled안에 각각 서버에 대한 conf파일들을 만들고 이 안에 첨부하여 웹서버를 운영할 수 있습니다. conf 파일안을 보시면 http, server, location, upstream과 같이 나누어져 있는데 이를 블록이라 하며 server는 가상 서버 혹은 일반 서버를 호스팅 할 때 사용되며 location의 경우 특정 폴더 밑 파일에 대한 경로를 지정해주고 upstream의 경우 Reverse Proxy 설정을 위해서 사용됩니다.

sites-enabled : 위에서 말한 nginx.conf에 첨부해서 실제로 서버를 운영할 설정 파일들이 들어있는 폴더입니다. 실제로 코드를 보면 nginx.conf에서 여기 폴더에 있는 모든 파일들을 불러옵니다.

fastcgi_params, scgi_params, uwsgi_params : uwsgi와 같이 웹 서버와 애플리케이션 서버 사이에서 인터페이스 역할을 해줄 때 필요한 파일들입니다.

파일을 어떻게 사용하는지에 대한 간단한 설명은 다음 포스팅에서 Nginx-uWSGI-Django 연결 편에서 사용해볼 예정입니다.


## 명령어
// 시작
$ sudo service nginx start
$ sudo systemctl start nginx
$ sudo /etc/init.d/nginx start

// 재시작
$ sudo service nginx restart
$ sudo systemctl restart nginx
$ sudo /etc/init.d/nginx restart

// 중지
$ sudo service nginx stop
$ sudo systemctl stop nginx
$ sudo /etc/init.d/nginx stop

// 상태
$ sudo service nginx status
$ sudo systemctl status nginx

// 설정 reload
$ sudo service nginx reload
$ sudo systemctl reload nginx
$ sudo nginx -s reload

// configuration file syntax check
$ sudo nginx -t

upstream 을 ip 로  하면 안되는데 ip 로 하는 경우, socket 명령으로 uwsgi 를 돌리면 된다.
http 로 하려고 해서 안됏음
socket 파일로 하면 된다.

[Nginx uWSGI Django 연결하기](https://twpower.github.io/linux/2017/04/13/41(Nginx-uWSGI-Django-연결하기).html)
[uwsgi-docs](http://uwsgi-docs.readthedocs.io/en/latest/tutorials/Django_and_nginx.html)
[Ubuntu에서 apt-get을 통해 nginx 설치하기 및 간단한 정리](https://twpower.github.io/linux/2017/04/08/39(Ubuntu에서-apt-get을-통해-nginx-설치하기-및-간단한-정리).html)
[Python, Django 등의 작업환경 설정하기](https://wiki.chann.kr/project/django-python-initial-setting)
[Deploy Django Project in nginx with uWSGI on Mac OS X](https://www.sean-lan.com/2016/09/16/django-uwsgi-nginx/)
