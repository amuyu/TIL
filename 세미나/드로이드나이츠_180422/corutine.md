# 코루틴은 어떻게 동작하는가?
코루틴은 디컴파일이 안된다...
github/nocode

# coroutine
light weight thread
subroutine : 단일 시작, 특정 종료
coroutine : 단일 시작, 임의 멈춤, 해당 재개, 특정 종료


## 그럼 어떻게 멈췄다 다시 시작하나요?
.... suspend, state, Labeling(suspend 호출 위치), coroutineImpl, Continuation 파라미터(인수인계문서)
StateMachine
누가 호출, 어디까지 실행, 어떤 값들(input, local v, return v) => state
상태를 저장하면 다시 시작할 수 있지 않을까?

Continuation Passing Style
인수인계 데이터
Continuation 에 값이 추가됨 (상태저장)
상태복원

## resume
실행 재개 콜백

# coroutine context
어떤 스레드에서 실행할 것인가

# coroutine builder
kotlin coroutine builder
