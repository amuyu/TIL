김석용 : jsonkim
gdg slack : https;//gdgkr.herokuapp.com
www.meetup.com/gdg-incheon/

# 카카오톡 오픈 채팅
https://open.kakao.com/0/glguhij
https://pf.kakao.com/_cxlxexgm
https://open.kakao.com/0/g8rsgb


# Android Architecture Components 소개와 응용
## Lifecycle
lifecycle 에 대한 control 이 쉽다.
### LifecycleOwner
LifeCycleActivity/LifeCycleFragment 에서
`getLifecycle()`을 호출해서 사용할 수 있음
```java
lefecycle.getCurrentState().isAtLeast();
// isAtLeast : Compares if this State is greater or equal to the given state.
```
### LifecycleObserver
생명주기에 해당하는 메소드 실행 가능
스스로 라이프 사이클을 핸들링할 수 있음.
```java
@OnLifecycleEvet(Lifecycle.Event.ON_START)
@OnLifecycleEvet(Lifecycle.Event.ON_STOP)
```



## LiveData
데이터 자동 갱신
Activity 나 Fragment 가 없어도 사용 가능하다.
`observe` : 데이터 변화 상태를 감지 (lifecycle 이 onStart 이상이어야 동작)
```java
protected void onActive();  // 1+ active observe
protected void onInactive();  // 0 active observe
```


## ViewModel
Data holder by Activity & Fragment
lifecycle이 finish 할 때까지 유지
Activity 나 Fragment 와 상관이 없어져서 Test 가 쉽다.
```java
ViewModelProviders.of(this);  // this : Activity or Fragment
```


## Room
Boilerplate-free
compile time 에 error check
Observable (LiveData, rxjava2)
Annotation 기반 (@Dao, @Entity, @Database)
### Room with Query
query 문이 틀릴 경우 compile time 에서 에러로 뱉는다.
```java
@Query("SELECT * FROM user WHERE uid :uid")
User findByUid(String uid);
```
LiveData와 rxjava2 같이 사용하면 power가 늘어난다.



## Paging Library
room 과 같이 쓰면 좋다
Twist query data for pagination (일반 쿼리로 paging 쿼리 처럼 사용 가능하다.)
Query by page (스크롤 이벤트 감지에 따라 처리가 필요한데 알아서 해준다.)
Check item diff (페이지 이동 중, 데이터 갱신이 발생했을 때,, 중복 등의 처리해준다.)
### 구성
PagedListAdapter, PagedList, DataSource
#### dataSource
데이터를 load
#### PagedList
Data 를 들고 있음.... DataSource 를 만들어서 자동으로 Data를 읽어들임
Configure 필요(page size, prefetch distance)
LivePageListProvider ,, 위의 것들을 Build 해준다.
#### PagedListAdapter
중복으로 가져오는 값을 체크해준다.
#### 예시
Room 에서 LivePagedListProvider로 변환해준다.
Query 를 Provider에 넣어주
```java
// DataSource
@Query("...")
LivePagedListProvider<T> get();

// LiveData
LiveData<PagedList<T>> lists = PagedList.Config.Builder()
  .setPageSize(50)
  .setPrefetchDistance(50)
  .build()

// PagedListAdapter  
DiffCallback 을 정의해야 값의 중복을 체크해준다.(DiffUtil)
```



## AAC 적용은 어떻게?
### 구글에서 말하는 요렇게 해라
UI - ViewModel - Repository - DataSource
데이터가 자동으로 변화하는 Architecture
현재의 구조가 탄탄하다면 Do not rewrite app!
### 발표자가 생각하는 MVP with AAC
UI - Presenter(LifeCycle) - ViewModel(LiveData) - Repository - DataSource
### 발표자가 생각하는 MVP with LiveData
UI - Presenter(LifeCycle, LiveData) - Repository - DataSource
### 발표자가 생각하는 MVP without LiveData
UI - Presenter(LifeCycle) - Repository(Observer) - DataSource
