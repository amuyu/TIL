
# BasicRxJavaSample [github](https://github.com/googlesamples/android-architecture-components/tree/master/BasicRxJavaSample)
This is an api sample to showcase how to implements observable queries in Room, with RxJava's Flowable object.
The sample app shows an editable user name, stored in database
## 살펴보기
### Injection
This injects UserDataSource and ViewModelFactory.
UserViewModel을 주입하기 위함
#### UserDataSource
Interface for accessing user data(db)
There are getUser, insertOrUpdateUser, deleteAllUsers methods
##### LocalUserDataSource
Using the Room database as a data source
### UserActivity
CompositeDisposable 를 사용해서 view를 갱신하는 stream 이벤트를 관리한다.
### ViewModelFactory
UserViewModel 을 생성하기 위한 Factory
UserViewModel 은 생성할 때, UserDataSource 를 전달인자로 필요로 하기 때문에 Factory 클래스 추가가 필요함
### UserViewModel
ViewModel 클래스 사용, RxJava를 사용해서 db 조회 후, view 를 갱신함  
Completable 사용, 결과만 전송하는 이벤트
getUserName 을 보면 Flowable<String>을 리턴하는데 LiveData 가 아니어도 데이터가 변경되면 emit 한다
이상하네 LiveData 를 추가하면 되는데 안하면 안되네...
