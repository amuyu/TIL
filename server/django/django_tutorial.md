# [Tutorial 1](http://www.django-rest-framework.org/tutorial/1-serialization/)
project 를 생성하고 model, serializers, views 를 구현한다.
ModelSerializer 와 ModelViewset 을 사용하지 않고 구현한다.

# Creating a model to work with
`Snippet` 모델을 만들고 migration 한다.
```
python manage.py makemigrations snippets
python manage.py migrate
```

# Creating a Serializer class
serializer 클래스를 사용해서 `Snippet` 객체들을 json과 같은 형태로 serializing 하고 deserializing 한다.
## serializer/deserializer 확인
### serializer
model 을 만들고 그 model 에 해당하는 serializer class를 만든다.
```sh
serializer = ModelSerializer(model)
```
### deserializer
serializer 된 문자열을 Python native datatypes 으로 변환 후, parsing 한다.
```sh
from django.utils.six import BytesIO

stream = BytesIO(content)
data = JSONParser().parse(stream)
```
## Using ModelSerializers
`ModelSerializers` 를 사용해서 위에서 구현한 class 를 간결하게 작성할 수 있다.

# Writing regular Django views using our Serializer
snippets 목록과 새 snippet 을 만들기 위한 view를 만든다.
기본적인 Django views를 확인할 수 있다.


# [Tutorial 2](http://www.django-rest-framework.org/tutorial/2-requests-and-responses/)
# Request objests
Request 핵심기능은 `reuqest.data` 이다.
```
request.POST  # Only handles form data.  Only works for 'POST' method.
request.data  # Handles arbitrary data.  Works for 'POST', 'PUT' and 'PATCH' methods.
```
# Response objects
Response 객체는 렌더링 되지 않은 content 를 취하고 content negotiation을 사용해 올바른 content 유형을
결정해 클라이언트에 반환한다.

# Wrapping API views
api views 는 `Request` 객체를 받고 `Response` 객체에 컨텍스트를 추가하여
content 를 잘 보여줄 수 있도록 몇 가지 기능을 제공한다.
api views 를 wrap 하는 방법
- `@api_view` 를 사용
- `APIView` 클래스를 사용

api view 를 사용하면 `JSONResponse` 를 return 할 필요가 없다.
request 에도 `JSONPaser` 를 사용해서 변환할 필요가 없다.
`Response` object 를 사용하면 된다.
api 내용을 가지고 html view도 만들어준다.

# Adding optional format suffixes to our URLs
format suffixes 를 사용하면 single content type 을 얻기 위해 url 을 추가할 필요가 없다.


# [Tutorial 3](http://www.django-rest-framework.org/tutorial/3-class-based-views/)
APIView 클래스를 사용해 view 만들기
`APIView` 상속받아 클래스로 구현하는 경우 위에서 request 에서 HTTP methods 별로
분기처리 했던 부분을 구현할 필요가 없어 더 보기가 좋아졌다.

## Using mixins
`mixin` 클래스를 사용하면 코드가 더 간결해진다.
`ListModelMixin` `CreateModelMixin` `RetrieveModelMixin` `UpdateModelMixin`
를 사용해서 get, put, delete 에 대한 간결하게 할 수 있다.

이 코드는 다시 `ListCreateAPIView`, `RetrieveUpdateDestroyAPIView` 로 더 줄여질 수 있다.

# [Tutorial 4 - Authentication & Permissions](http://www.django-rest-framework.org/tutorial/4-authentication-and-permissions/)
여태까지 코드는 누구나 snippets 을 편집하고 삭제할 수 있었다.
snippets 을 만든 사용자만 편집할 수 있도록 구현하자

## Adding information to our model
`snippet` model 에 user를 추가할 수 있게 foreignkey 를 추가한다
```python
owner = models.ForeignKey('auth.User', related_name='snippets', on_delete=models.CASCADE)
```
`userserializer` 를 추가하고 snippet field 를 추가한다.
```python
snippets = serializers.PrimaryKeyRelatedField(many=True, queryset=Snippet.objects.all())
```


## Associating Snippets with Users
지금까지 코드에서는 사용자와 Snippet 인스턴스를 연결할 방법이 없다.
두 객체를 연결하기 위해서는 `perform_create` 가 필요하다.
snippet view 에서 `perform_create()` method 를 override 한다.
그 다음 SnippetSerializer 를 수정한다.

```python
def perform_create(self, serializer):
    serializer.save(owner=self.request.user)
```


## Adding required permissions to views
다음을 APIview 에 추가하면 auth 한 경우에는 write/read 권한이 주어지고
auth 안 한 경우에는 read 권한이 주어진다.
```python
permission_classes = (permissions.IsAuthenticatedOrReadOnly,)
```


## Adding login to the Browsable API
url 에 `api-auth` 를 추가하면 login과 logout 추가된다.


## Object level permissions
owner 에게만 permission 을 주고 싶을 때는 다음과 같은 코드를 추가한다.
```python
from rest_framework import permissions

class IsOwnerOrReadOnly(permissions.BasePermission):
    """
    Custom permission to only allow owners of an object to edit it.
    """

    def has_object_permission(self, request, view, obj):
        # Read permissions are allowed to any request,
        # so we'll always allow GET, HEAD or OPTIONS requests.
        if request.method in permissions.SAFE_METHODS:
            return True

        # Write permissions are only allowed to the owner of the snippet.
        return obj.owner == request.user
```
그리고 view 에 만든 클래스를 추가한다.
```python
permission_classes = (permissions.IsAuthenticatedOrReadOnly,
                      IsOwnerOrReadOnly,)
```


# [Tutorial 5: Relationships & Hyperlinked APIs](http://www.django-rest-framework.org/tutorial/5-relationships-and-hyperlinked-apis/)
 `hyperlink` 를 사용해 API view에 link 를 추가한다.


## Creating an endpoint for the root of our API
api root view 를 만든다.
UserList 와 SnippetList View를 호출할 수 있도록 view 와 url 에 추가한다.
```python
@api_view(['GET'])
def api_root(request, format=None):
    return Response({
        'users': reverse('user-list', request=request, format=format),
        'snippets': reverse('snippet-list', request=request, format=format)
    })

url(r'^$', views.api_root),
url(r'^users/$', views.UserList.as_view(), name='user-list'),
url(r'^snippets/$', views.SnippetList.as_view(), name='snippet-list'),
```
detail 에 대한 link 도 만든다.


## Hyperlinking our API
serializers 부모 클래스를 `HyperlinkedModelSerializer` 변환해서 링크를 만든다.
`PrimaryKeyRelatedField` 는 `HyperlinkedRelatedField`로 바꾸고
`HyperlinkedIdentityField`와 `url` 를 사용해서 각각의 링크를 생성한다.


## Adding pagination
pagination 을 사용해서 default page 스타일을 구현한다.
```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10
}
```


# [Tutorial 6: ViewSets & Routers](http://www.django-rest-framework.org/tutorial/6-viewsets-and-routers/)
`viewsets` 와 `routers` 를 사용하면 더 쉽게 개발할 수 있다.

## Refactoring to use ViewSets
`UserList`와 `UserDetail` 을 하나의 `UserViewSet`로 구현할 수 있다.
`SnippetList`, `SnippetDetail` and `SnippetHighlight` 도 하나의 클래스로 구현이 가능하다.

ViewSet 으로 변경한 내용을 urls 에도 반영한다.
ViewSet 에서 HTTP method 를 선택해서 user_list, usre_detail 등으로 나누어 처리할 수 있다.

클래스는 하나로 줄어들었는데 뭔가 번거롭다.


## Using Router
router 를 사용하면 url 마다 view 를 셋팅하지 않아도 된다.
DefaultRouter 는 api root 도 자동으로 만들어준다.


# [Tutorial 7: Schemas & client libraries](http://www.django-rest-framework.org/tutorial/7-schemas-and-client-libraries/)
schema 는 document 를 자동으로 생성하는데 유용한 도구이다.
api view 를 만들기위한 정보만 뽑아낸 view
