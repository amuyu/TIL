# Repository (google-todo-sample 참고)
local, remote, cache


# 개요
Repository는 데이터를 가져올 수 있는 곳을 local과 remote로 나눈다
처음 데이터 호출 시에는 remote 데이터를 가져와 local과 cache에 각각 저장하고 cache 데이터를 리턴한다
그 다음 호출 부터는 데이터 갱신이 필요할 경우에만 remote 데이터를 호출하고
그렇지 않은 경우 cache 데이터가 있으면 cache 데이터를 리턴하고
cache 데이터가 없는 경우 local 데이터를 호출한다.
만약 local 데이터가 없으면 remote 데이터를 가져온다.


# 구성
- DataSource :  사용자(Presenter)가 접근할 수 있는 Interface, 비즈니스 로직
- Repository (UserInterface) : 사용자가 접근하는 Interface 객체, presenter, usecase에서 호출됨
- RemoteDataSource : 원격 저장소
- LocalDataSource : 로컬 저장소 (Sqlite)
- cache : 빠르게 조회하기 위한 메모리 저장소
## dependency
ui > repository > local,cache,remote


## cacheIsDirty
cache가 유효한 상태를 표시한다.
cache의 데이터가 유효하지 않으면 네트워크로부터 데이터를 조회한다.
### cache 데이터 갱신 시점
패턴에서는 별도로 갱신을 요청하지 않고 사용자가 갱신 시점을 정하도록 한다.
이 프로젝트에서는 option menu 에서 사용자가 갱신을 요청할 때, 갱신한다.


# 조회 프로세스\
## 원격 데이터 조회
cache/local에 데이터가 없거나 업데이트가 필요할 때 원격 데이터를 조회한다.
원격 데이터 조회 > cache 저장 > local 저장 > 데이터 콜백
## 로컬 데이터 조회
cache 데이터가 없을 때, 로컬 데이터를 조회한다.
로컬 데이터 조회 > cache 저장
## cache 조회
cache 데이터 조회한다.



# Repository 클래스 예제
Repository class는 single 패턴을 사용하여 어디서든 사용할 수 있도록 한다
Injection 클래스를 사용하면 테스트 등이 용이하다.
```java
public class Repository implements DataSource {
    private DataSource localDataSource;
    private DataSource remoteDataSource;
    private HashMap<> cache;
}
```




# inject

## 참고
[todo-app](https://github.com/googlesamples/android-architecture/tree/todo-mvvm-databinding/)
