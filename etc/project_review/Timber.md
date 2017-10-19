# Timber
log 저장하는 라이브러리 프로젝트
## tree(abstract)
로그 호출 인터페이스
### ThreadLocal
쓰레드 단위로 로컬 변수를 할당한다,
쓰레드 영역에 변수를 설정한다.
쓰레드 기준으로 동작하는 기능 구현시 , 유용하게 사용가능하다
Tree에서 ThreadLocal을 사용한 이유는 뭘까? 다른 thread 에서 tag를 채가지 않도록 하기 위해서
tag 입력은 기존의 입력처럼 Logger(tag, message) 형태로 사용하도록 하는게 나을듯
## TREE_OF_SOULS
tree 호출(리스트) - adapter 역할
## DebugTree
실제 동작하는 tree
## Test
Robolectric, festassert 사용
shadowlog, LogAssert

## 배울점
### 멀티쓰레딩
volatile, synchronized, threadlocal
### list
unmodifiableList
