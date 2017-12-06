@seongug.jung
`Master Module`
- A Module
- B Module
- C Module
- D Module

D 모듈이 내용이 A,B,C 에 각각 종속성을 가지는데
이걸 대거로 해서 서로간의 의존성 설정 없이
A -> Master 주입
B -> Master 주입
C -> Master 주입
Master -> D 주입

으로 D 에서는 추상화된 Factory 객체 전달 받아서 lazy init 하는 방식으로 의존성을 없이 각각 동작하도록 했네요 -ㅅ-)
