
## Clean-Architecture-Android
euro2016 팀정보
[github](https://github.com/erikcaffrey/Clean-Architecture-Android)

### Architecture
TeamsActivity > TeamsPresenter > (Domain)UseCase > Repository > Data(Entity)
#### Entity
- ImageEntity : RestApi response
- TeamEntity : RestApi response
#### Data
- LocalApi : TeamEntity 를 호출하는 인터페이스
- LocalImpl : 파일로부터 데이터 호출, LocalApi 인터페이스 구현
#### Repository
Repository 와 DataSource 를 분리함
TeamRepository > Repository
TeamsLocalApiDataSource > DataSource
LocalImpl > LocalApi
- DataSource : TeamEntity 를 호출하는 인터페이스
- TeamLocalApiDataSource : LocalApi 에 접근하기 위한 객체 (Repository 에서 사용)
- TeamDataSourceFactory : DataSource 객체를 생성하기 위한 Factory
- Repository : Team 을 호출하는 인터페이스 (presenter 계층에서 사용)
- TeamsRepository : Team 을 호출하기 위한 객체
- TeamEntityJsonMapper : json message 에서 TeamEntity로 변환
- TeamToTeamEntityMapper : TeamEntity 에서 Team으로 변환
#### Model
- Team
#### UseCase
- UseCase : RxJava 를 사용한 UseCase 구현
- UseCaseObserver : RxJava 의 Observer interface 이름을 알아보기 쉽게 변경
- GetEuroTeams : Team 리스트를 가져오는 UseCase
- TeamListObserver : UseCase 의 Listener 에 해당하는 클래스
#### mvp
- TeamsPresenter : TeamsActivity 의 비즈니스 로직 정의
- TeamViewModel : View 에서 데이터를 표시하는 Model
- TeamViewModelToTeamMapper : Team Model 을 TeamViewModel 로 변환
- Presenter : view와 presenter 가 interface 가 아니라 presenter 클래스이다.
   (지금까지 Contract interface 안에 View와 Presenter interface를 정의했다.)



### injection
Application 에서 Main에 해당하는 Component를 생성하고 Activity 에서는
mainComponent를 호출한 불러와 주입
#### mainComponent
각 Activity 에서 주입할 수 있도록 주입 함수 추가
#### MainModule
@Singleton 사용
Context 는 Application 으로 사용
Repository 생성


### View
#### BaseActivity
Activity 에서 공통적으로 사용하는 부분 구현
setupToolbar, bindViews, initView(abstract)



### Test
#### Dependencies
테스트를 위해 사용하는 라이브러리
```gradle
compile junit:junit:4.12
compile org.mockito:mockito-all:1.10.19
compile org.hamcrest:hamcrest-all:1.3
compile org.robolectric:robolectric:3.0
```



### 배울점
- gson library @SerializedName 사용
- network 를 사용하지 않고 Local 로 json 파일을 저장해 개발하는 방법도 있음
- Mapper 사용, pattern 의 일종인가?
- UseCase 사용 Presenter 구현

## tag
#Dagger, #Architecture
