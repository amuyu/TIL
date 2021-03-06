
# Handling Lifecycles
Lifecycle class는 lifecycle state 대한 정보를 가지고 있다.
lifecycle 이벤트들은 framework와 lifecycle class에 전달된다
lifecycle state : initialized, destroyed, created, started, resumed
## lifecycle-aware components
LiveData, ViewModel, LifeCycleActivity/Fragment
## LifecycleOwner
LifeCycle 관련 인터페이스를 가지는 클래스 getLifecycle()
LifecycleActivity, LifecycleFragment 에서 getLifecycle() 인터페이스로 사용한다.
## LifecycleRegistry
addObserver를 통해서 LifeCycle 이벤트를 observe 할 수 있다

#### download
viewmodel 을 사용하기 위해서 dependencies 를 추가한다.
*project/build.gradle*
```groovy
allprojects {
    repositories {
        jcenter()
        maven { url 'https://maven.google.com' }
    }
}
```
*app/build.gradle*
```groovy
compile "android.arch.lifecycle:runtime:1.0.0-alpha1"
compile "android.arch.lifecycle:extensions:1.0.0-alpha1"
annotationProcessor "android.arch.lifecycle:compiler:1.0.0-alpha1"
```

======
# LiveData
lifecycle 에 따라 동작하는 data holder class 이다.
LiveData는 lifecycle이 STARTED or RESUMED state일 때 활성화된다. (onActive/onInactive)
-> 다른 앱 실행 중 에는 전달하지 않음
그리고 observer에게 데이터를 전달한다.
여러 fragment/activity 에서 observe 할 수 있다
## observe
```java
LiveData.get(getActivity()).observe(this, location -> {
   // update UI
});
```

## setValue
setValue : 값을 update 하고 변화를 observer에게 알린다
## advantages
- No memory leaks: lifecycle이 destroy 되면 객체들도 자동으로 clean 된다
- No crashes due to stopped activities: inactive state 에서는 변경하지 않는다
- Always up to date data: lifecycle이 다시 시작되면 최신 데이터를 갱신한다
- Proper configuration change: configuration 이 change 될 때, 알아서 데이터를 가져온다
- No more manual lifecycle handling: lifecycle에 맞춰 handling 할 필요가 없다
## Transformations of LiveData
LiveData를 다른 형태로 변형해서 전달할 수 있다.
*Transformations.map()*
```java
LiveData<User> userLiveData = ...;
LiveData<String> userName = Transformations.map(userLiveData, user -> {
    user.name + " " + user.lastName
});
```
*Transformations.switchMap()*
```java
private LiveData<User> getUser(String id) {
  ...;

}

LiveData<String> userId = ...;
LiveData<User> user = Transformations.switchMap(userId, id -> getUser(id) );
```
Transformation을 사용할 때, lifecycle에 따라 데이터가 업데이트 되게 하려면 다음과 같이 구현해야 한다.
```java
class MyViewModel extends ViewModel {
    private final PostalCodeRepository repository;
    private final MutableLiveData<String> addressInput = new MutableLiveData();
    public final LiveData<String> postalCode =
            Transformations.switchMap(addressInput, (address) -> {
                return repository.getPostCode(address);
             });

  public MyViewModel(PostalCodeRepository repository) {
      this.repository = repository
  }

  private void setInput(String address) {
      addressInput.setValue(address);
  }
}
```


======
# ViewModel
## 생성
ViewModelProviders를 사용해서 생성
lifecycle이 finish 할 때까지 유지
```java
ViewModelProviders.of(this).get(MyViewModel.class)
```
### 생성자가 필요한 경우
ViewModelProvider.Factory 를 사용해서 생성자를  전달받을 수 있다.
Factory 구현
```java
public class ViewModelFactory implements ViewModelProvider.Factory {
    private final UserDataSource mDataSource;
    public ViewModelFactory(UserDataSource dataSource) {
        mDataSource = dataSource;
    }

    @Override
    public <T extends ViewModel> T create(Class<T> modelClass) {
        if (modelClass.isAssignableFrom(UserViewModel.class)) {
            return (T) new UserViewModel(mDataSource);
        }
        throw new IllegalArgumentException("Unknown ViewModel class");
    }
}
```
호출
```java
ViewModelProviders.of(this, ViewModelFactory).get(UserViewModel.class);
```
## Update UI
```java
public class MyActivity extends AppCompatActivity {
    public void onCreate(Bundle savedInstanceState) {
        MyViewModel model = ViewModelProviders.of(this).get(MyViewModel.class);
        model.getUsers().observe(this, users -> {
            // update UI
        });
    }
}
```
## onCleared
lifecycle finish 가 되면 onCleared가 호출됨
## 궁금증
BaseObserver와 같이 사용할 수 있는가..  
ovservable fields를 사용하면 같이 사용할 수 있을듯. 근데 또, LiveData를 생각하면
ui - viewmodel(baseobserver) - presenter -
(databinding + viewmodel) class 하나 만들자
## 적용
ViewModel 은 bundle 같이 초기에 넘어오는 데이터로 셋팅해서 사용할 수 있을 듯? (BaseObserver 를 사용하는 경우?)


# 참고
[Single Activity](https://mymusictaste.github.io/2017/04/15/android-single_activity_architecture.html)
[Lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle.html)
