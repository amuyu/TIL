# android component architecture - GithubBrowserSample
Dagger.android 를 사용함
## Application
### HasActivityInjector
Application 에서 사용하며, 이렇게 셋팅해놓으면 Activity 단위에서
`DaggerComponent.builder()..` 의 호출없이 좀 더 쉽게 inject 호출할 수 있음
inject 호출 시, 사용
Application 에 interface 구현
구현 후,
```java
DaggerYourApplicationComponent.create()
        .inject(this);
```
이렇게 하면 Activity 에서 이렇게 호출하면 됨
```java
public class YourActivity extends Activity {
  public void onCreate(Bundle savedInstanceState) {
    AndroidInjection.inject(this);
    super.onCreate(savedInstanceState);
  }
}
```
가이드에 나오는 내용..
[dagger.android](https://google.github.io/dagger//android.html)

### @Component.Builder
Component.builder 에 대한 인터페이스 추가

### ViewModelModule
AppMoule에 viewmodelmoudle 이 들어간다

### AppInjector
DaggerAppComponent.builder()를 호출해서 inject 해주는 클래스

## Single Activity
### NavigationController 사용
Fragment에서 다른 fragment 로 이동할 때 사용

## SearchFragment
### lifecycle
onCreateView 에서 view를 생성하고,
onActivityCreated 에서 view 초기화 등, 셋팅작업
### AutoClearedValue
A value holder that automatically clears the reference if the Fragment's view is destroyed.
### data 갱신
searchviewmodel.results.observe 해서 갱신되면
DataBoundListAdpater의 replace를 호출함


## GithubViewModelFactory
ViewModelProvider.Factory 를 구현

## SearchViewModel

## DataBoundListAdapter
Data Binding & DiffUtil 사용한 Adapter
replace 사용, viewModel에서 받은 list와 adapter의 list를 비교해서 데이터를 갱신함 - diffutil 사용


## etc
### manifest 분리
debug 에 대한 별도 manifest 사용
