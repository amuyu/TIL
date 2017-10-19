# MVVM-with-AAC [MVVM-with-AAC](https://github.com/ZeroBrain/MVVM-with-AAC)
This source is using Dagger, Databinding, Room, ViewModel, LiveData and MVVM based.
Stetho 를 사용함
kotlin extension 사용

## LottieApplication
LottieApplication 생성 시, LottieApplicationComponent를 생성하고
LottieApplicationComponent를 Activity 에서 가져다가 쓸 수 있게 함
LottieListActivity 에서 appComponent를 주입함
## LottieApplicationDagger.kt - LottieApplicationComponent
App에서 공통적으로 사용하는 모듈들을 주입함 (okhttp, database 등)
LottieApplicationComponent 에서 lottieListComponent(@Subcomponent) 도 추가 주입할 수 있도록 함
DaggerLottieListComponent.builder()를 호출하지 않아도 됨
LottieApplicationComponent 와 LottieApplicationModule 를 정의
java에서는 따로따로 생성했던 것 같은데, kotlin에서는 함께 작성?
filepath method 에 object 대입?
### LottieApiModule
okHttpClient, retrofit 생성
### LottieDatabaseModule
database 생성(room)
### LottieDaoModule
LottieDao 생성


## LottieDagger.kt - LottieListComponent
subcomponent, LottieAppComponent 에 이어 호출
LottieListComponent를 생성하기 위한 Dagger 클래스를 만들지 않아도 된다.
### LottieListModule
LottieModel 생성, inject 클래스
LottieListViewModel 생성
ViewModelProvider 생성 - ViewModelProvider.Factory 를 사용해서 ViewModel 생성함


## LottieListActivity
Application 클래스의 appcomponent를 호출해 subcomponent와 결합 후, inject
lazy 사용
### addObserver?
LifecycleRegistry의 addObserver 를 호출
```kotlin
lifecycle.addObserver(lottieListViewModel)
```
viewModel 에서 lifecycle event 에 따라 직접 컨트롤
```kotlin
@OnLifecycleEvent(Lifecycle.Event.ON_CREATE)
    fun onInit() {
        lottieModel.refresh()
}

@OnLifecycleEvent(Lifecycle.Event.ON_DESTROY)
fun onDestroy() {
    lottiesDisposable.takeIf { !it.isDisposed }?.dispose()
}
```
### act_lottie_list.xml
EditText 에서 onTextChanged 속성에 method 호출을 사용 방법
```xml
<EditText
  android:onTextChanged="@{(text, _1, _2, _3)-> viewmodel.onSearch(text)}"/>
```
RecyclerView 에 items 적용 방법 app:lottieitems 사용 recyclerview 에 list 셋팅
binding 으로 알아서 adapter와 연결해준다
```xml
<android.support.v7.widget.RecyclerView
    app:lottieitems="@{viewmodel.lotties}" />
```


## LottieListAdapter
onCreateViewHolder 에서 그리드 뷰의 width 를 계산해서 셋팅함
```kotlin
return LottieViewHolder(lottieItemBinding.apply {
            val columnWidth = root.resources.displayMetrics.let { Math.min(it.widthPixels, it.heightPixels) / columnCount }
            root.layoutParams = root.layoutParams.apply {
                width = columnWidth
                height = columnWidth
            }
        })
```


## lottieListViewModel
BehaviorProcessor subject 사용
FlowableForLifecycle.FlowableForLifecycle 사용 > custom function
subscribeIgnoreError 사용 > Extension Function 사용
onSearch 사용
@OnLifecycleEvent 사용, onCreate 와 onDestroy에 어떤 작업을 할 지 구현함
### rxJava
distinctUntilChanged 사용, 데이터 변화가 생기면 emit
sample 사용, 일정 시간 안에 발생한 최근 이벤트를 emit


## LottieModel
lottiedao, lottieApi, downloadApi, filePath 사용해서
refresh(api), getData(db), search(db) 동작 수행
LottieModel 에서 리스트를 가지고 있는게 아니라 Flowable<List<Lottie>>, LiveData<List<Lottie>> return type 으로 사용


## RxUtil.kt
boilerplate code

## 배울점
### subcomponent 사용
ApplicationComponent에 이어 LottieListComponent라는 subcomponent 를 사용
appcomponent에서 apimodule, databasemodule 을 생성함
### scope 사용
ApplicationScope를 사용
```kotlin
@Scope
@Retention(AnnotationRetention.RUNTIME)
annotation class ApplicationScope
```
@retention 은 컴파일러 옵션을 표시하는 annotation 임
