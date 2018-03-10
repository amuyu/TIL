# quick start
model, serializer, viewset, routers

## Project Setup
project 생성
```sh
django-admin startproject tutorial
```
project 폴더 이동
app 생성
```sh
django-admin startapp quickstart
```
database 동기화
```sh
python manage.py migrate
```
admin 계정 추가
```sh
python manage.py createsuperuser --email admin@example.com --username admin
```

project layout

```sh
$ pwd
<some path>/tutorial
$ find .
.
./manage.py
./tutorial
./tutorial/__init__.py
./tutorial/quickstart
./tutorial/quickstart/__init__.py
./tutorial/quickstart/admin.py
./tutorial/quickstart/apps.py
./tutorial/quickstart/migrations
./tutorial/quickstart/migrations/__init__.py
./tutorial/quickstart/models.py
./tutorial/quickstart/tests.py
./tutorial/quickstart/views.py
./tutorial/settings.py
./tutorial/urls.py
./tutorial/wsgi.py
```

## Model 추가
models.py 에 필요한 Model 을 추가한다.
## Serializer
models 객체와 querysets 같은 복잡한 데이터를 native python datatypes로 바꾸는 역할을 한다. 이 작업으로 JSON, XML로 표현하기 쉽다.
데이터들 중에서 serialized/deserialized 할 것들을 정의한다.
serializer 클래스는 `Form` 클래스와 비슷한 validation flags 와 다양한 fields 가 있다.

### ModelSerializer
`ModelSerializer` 는 정의한 model 에 가깝게 자동으로 Serializer 를 생성해준다.
클래스 생성후, field를 하나하나 정의할 필요 없이 inner clas로 Meta 클래스를 작성해
model 과 field 를 추가하면 된다.

- An automatically determined set of fields.
- Simple default implementations for the create() and update() methods.
`HyperlinkedModelSerializer` 를 사용한 Serializer

### ModelSerializer의 기능
It will automatically generate a set of fields for you, based on the model.
It will automatically generate validators for the serializer, such as unique_together validators.
It includes simple default implementations of .create() and .update().

`fields = '__all__'` model 의 모든 field 를 추가한다.
`exclude = ('users',)` field 를 제외한다.

```python
from django.contrib.auth.models import User, Group
from rest_framework import serializers


class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ('url', 'username', 'email', 'groups')


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ('url', 'name')
```
## ViewSet
`viewsets`를 사용한 간단하게 Model 을 화면에 표시
`GenericAPIView` 를 상속받는다.
queryset 과 serializer_class 를 지정
## router 설정
API URLs 추가
DefaultRouter를 사용한다.
```python
from django.conf.urls import url, include
from rest_framework import routers
from tutorial.quickstart import views

router = routers.DefaultRouter()
router.register(r'users', views.UserViewSet)
router.register(r'groups', views.GroupViewSet)

# Wire up our API using automatic URL routing.
# Additionally, we include login URLs for the browsable API.
urlpatterns = [
    url(r'^', include(router.urls)), # default router
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```
## Settings
'rest_framework' to INSTALLED_APPS 추가

Viewsets

## Run
```sh
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

## 참고
[quickstart](http://www.django-rest-framework.org/tutorial/quickstart/)
[예제로 배우는 python 프로그래밍](http://pythonstudy.xyz/python/article/307-Django-템플릿-Template)
[restframework quickstart](http://www.django-rest-framework.org/tutorial/quickstart/)
[django 설치부터 배포까지](https://tutorial.djangogirls.org/ko/)
[괜찮은 Django Rest Framework 강좌를 찾아서 소개합니다](http://raccoonyy.github.io/django-rest-framework-tutorial-by-devissue/)
  - basic view, model
  - admin register
  - pagination,
  - login
