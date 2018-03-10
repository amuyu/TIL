# django 사용 환경 구축
OS : Ubuntu
WebServer : Nginx  (apache 같은거)
AppServer : uWSGI (gunicorn, uWSGI)
[gunicorn vs uwsgi](http://devspark.tistory.com/entry/gunicorn-vs-uwsgi)
Django + DRF(Django REST Framework)
# DRF(Django REST Framework)


# Django REST framework
기능이 엄청 많다
CBV로 API 를 만들 수 있다.
속도가 느리다?

# 설치
djangorestframework

# test Apps
```sh
django-admin startproject {name}
python manage.py migrate
python manage.py runserver
python manage.py startapp blog
python manage.py makemigrations blog
python manage.py migrate blog
python manage.py runserver
python manage.py createsuperuser
python manage.py runserver
```

# 장고 관리자?

# shell
shell 에서 클래스들을 호출해 보고 할 수 있다.
```sh
python manage.py shell
```

### swagger
rest_framework_swagger
[swagger 적용](https://devissue.wordpress.com/2015/02/01/pycharm과-함께-django와-restframework를-활용한-웹-사이트-구축하기/)


# architecture
mtv 구조
django 에서 view 란 view에 보여질 데이터를 가공하는 단계

## 참고
[django 저는 이렇게 씁니다.](https://www.slideshare.net/perhapsspy/django-42665652)
[React with Redux 와 Django를 이용한 SPA 개발](http://webframeworks.kr/tutorials/react/react-django-full-stack-spa/)
[Django REST framework 시작하기](https://cjh5414.github.io/django-rest-framework/)
[Nginx uWSGI Django 연결하기](https://twpower.github.io/linux/2017/04/13/41(Nginx-uWSGI-Django-연결하기).html)
[Django 스터디 3주차 | GET/POST , Apache/Nginx , Django로 Poll App 만들기](https://medium.com/djangogirlsseoul-codecamp/django-스터디-3주차-get-post-apache-nginx-django로-poll-app-만들기-c638dc0c8b3b)
[django rest swagger](https://django-rest-swagger.readthedocs.io/en/latest/)
[Documents](https://docs.djangoproject.com/en/2.0/)
[Django 기본 05 - 모델(Model), 모델필드, 필드옵션](https://wayhome25.github.io/django/2017/03/20/django-ep5-model/)
[facebook 장고그룹 내용을 크롤링 하는 사이트](https://fbdjango.wordpress.com)
[django-github](https://github.com/marcgibbons/django-rest-swagger)
