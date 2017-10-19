# todoapp
[todoapp](https://github.com/googlesamples/android-architecture.git)
## todo-mvp-clean
clean architecture 와 MVP 패턴을 를 배울 수 있는 프로젝트이다.

### Clean Architecture 살펴보기
#### UseCase
presenter 에서 domain 에 접근할 때 UseCase를 사용한다.
UseCase는 비동기로 동작하기 위해 Callback, ThreadPool 을 사용한다.
##### Repository 접근하기
Repository에 접근하기 위해서 정의한 Usecase를 사용한다.
##### UseCaseHandler
UseCase 를 바로 호출하는게 아니라 UseCaseHandler를 사용하여 실행되는 스케줄을 관리를 한다.
##### UiCallbackWrapper
callback 호출은 handler를 통해서 한다.
##### UseCaseScheduler
실행되는 UseCase의 thread 를 관리한다.
##### RequestValues
사용자가 요청할 때, 조회에 필요한 값 (비즈니스 로직)
##### ResponseValue
사용자가 조회 후, 응답으로 받을 값 (비즈니스 로직)
#### Entity
##### Task
id, title, description, completed

### UI
option menu 를 poopup menu를 사용해서 호출한다.


## 배울점
### 패키지 관리
크게 화면으로 분류를 한다. 그 다음 화면 폴더 안에서 architecture 로 패키지를 세분화한다.

### Dependency Injection
Injection 클래스를 사용해서 필요한 클래스를 주입한다. Dagger로 대체 가능하다
data 라는 pakage에 생성해서 사용한다. flavor에 따라 변경될 수 있도록 한다.

### Adapter
list 의 adpater를 사용할 때, Adapter 클래스 내부에
add, remove 등을 구현했었는데 여기서는 list 를 교체한다.
view에서 데이터에 접근해 호출할 때, 조회한 데이터와 adpater 클래스 데이터를
중복으로 관리하거나 중복을 줄이기 위해 조회한 데이터를 바로 adapter에 넣어주는 방식으로
했었는데 여기서는 그렇지 않다.
왜? 일단은 architecture에서는 view와 domain 영역을 분리한다.
adapter는 view의 영역에 해당하기 때문에 이런 구현이 되는 듯하다.

### Presenter
presenter 의 생성을 Activity에서 한다
생성하고 사용하기에는 Fragment 가 편하다. 이렇게 하는 이유는?
추측하기로는 domain에 접근하는 클래스들을 fragment 마다 생성하지 않아도 된다?(싱글톤을 쓰면 상관은 없는데...)
presenter는 비즈니스 로직에 접근하기 위한 클래스로 view와는 독립적이기 위해?
#### start
view에서 start를 호출하면 데이터 조회를 시작한다.

### loadTasks(Repository)
데이터 조회가 사용자가 action 을 취해하는 강제 업데이트인지 아닌지를 구분한다.
강제 업데이트가 아닌 경우, memory 에서 호출하도록 한다.

### filter
데이터 필터는 조회 후, 목록에서 거른다.

### onSupportNavigateUp
AppCompatActivity 의 method 로 navigationBar 에서 navigate Up을 선택할 때 호출한다.

### edittext > textview
edittext 로부터 text를 가져올 때, 코드 상에서 TextView 로 받아온다.
이렇게 하는 이유가 있나?

### isAdded
Return true if the fragment is currently added to its activity.
Fragment 에서 호출하는 함수

### 비즈니스 룰 체크
Task 에 대한 empty 체크를 할 때, 생성하기 전에 title, description의 각각의 값을 확인해서 체크하는게 아니라
Task 클래스에 미리 empty 체크를 만들어놓고 객체 생성후, empty 메소드를 호출한다.

### ListView.ClickListener
Listener 인터페이스를 추가해서 클릭 이벤트를 처리한다.

### TasksRepository.getTask
cache, local, remote  에서 task를 호출한다.
하지만 cache 나 local 에 추가하지는 않는다.
cache 나 local 동기화는 전체 리스트를 가져왔을 때만 이루어진다.

### Strings.isNullOrEmpty
string null 과 empty 체크

### onResume 에서 갱신
다른 화면에서 데이터가 변경되더라도 다시 화면으로 왔을 때, 데이터를 가져오기 때문에
데이터가 갱시된다.

### filter factoring
filter 기능을 가진 클래스를 구현
type 에 따라 filter 할 수 있는 인터페이스와 클래스를 만들어서 사용
UseCase 에서 filter 해서 presenter 에 전달

### Contract
실제 비즈니스 로직에 맞게 함수들을 구성한다.

### UseCase 패턴 구현
비동기 실행과 스케줄 관리를 할 수 있도록 구현이 되어 있다.


## todo-mvp-rxjava
rxjava를 사용하는 경우에는 usecase 가 필요가 없나? 아니면 clean-architecture 에서 파생된게 아닌가
filterfactory도 필요없다.

## 배울점
### CompositeSubscription 사용
복수의 Subscription 관리, 한꺼번에 unsubscribe 하기 편하다.
