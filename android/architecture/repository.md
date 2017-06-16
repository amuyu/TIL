# Repository (google-todo-sample 참고)
local, remote, cache
Repository는 데이터를 가져올 수 있는 곳을 local과 remote로 나눈다
처음 데이터 호출 시에는 remote 데이터를 가져와 local과 cache에 각각 저장하고 cache 데이터를 리턴한다
그 다음 호출 부터는 데이터 갱신이 필요할 경우에만 remote 데이터를 호출하고
그렇지 않은 경우 cache 데이터가 있으면 cache 데이터를 리턴하고
cache 데이터가 없는 경우 local 데이터를 호출한다.
만약 local 데이터가 없으면 remote 데이터를 가져온다.

# Repository class 생성
Repository class는 single 패턴을 사용하여 어디서든 사용할 수 있도록 한다
Injection 클래스를 사용하면 테스트 등이 용이하다.

# 구조
DataSource - 인터페이스 (remote, local, repository)
Repository - cache, remote, local 관리
ui - repository - local,cache,remote

# inject

## 참고
[todo-app](https://github.com/googlesamples/android-architecture/tree/todo-mvvm-databinding/)
