# 코드체인
코드체인 테크톡 유튜브
1. Foundry
    * 블록체인을 쉽게 만들 수 있는 프레임워크
    * principles
        * 플러거블러티를 최대한 빼고 제일 좋은 것을 사용하도록 한다. 
    * 컨센서스
        * 메인 - 텐더민트
        * 많은 블록체인에 상용 중이라 검증된 기능
        * 옵티마이징
            * randomized leader - 텐더민트에서 리더를 어떻게 선출 하는지(tenderand)
            * 텐더민트에 참여하는 벨리데이터들이 사인하는 것을 하나로 줄여서 사이즈를 줄이겠다.
        * 공격자가 블록 생성자를 예측하지 못하도록 할 수 없을까에 대한 결과
        * 2/3 의 사이닝이 필요한데 사이즈를 줄이기 위한.. 구현중.
* 모듈 시스템
    * 모듈을 컴포져블하게 만들어 빌트인 된 어플리케이션을 쉽게 만들도록 하자
    * 아이솔레이션을 확실히 하여 모듈이 끼칠 수 있는 악영향을 최소화 하겠다.
        * 샌드박스 활용
        * 자바스크립트 코스모스 사용
    * 중기/장기 목표
        * 디터마이니즘을 보장한다.
        * 로직에 문제가 생겼을 때 성능 문제를 해결한다.
        * 노드간의 메시지 전달 방식
    * 모듈코드
        * 샌드박스
            * 리눅스
            * 웹어샘블리 모듈
            * 내가 원하는 프로그램 랭귀지 가능
    * 장점: 만들어진 모듈로 쉽게 블록체인을 구현할 수 있다.
* ics
    * foundry 에서 기본적으로 제공함
* 암호 두가지 커브
    * Ed25519/BLS12-381 두 가지 커브를 사용
메인넷 - 코드체인
메인 - 텐더민트
2.
SVDW method
3.
VeriSync
    * 전체 히스토리를 동기화 해야하는 문제
    * 목표
        * 가장 처음 받아야하는 데이터 중 필요 없는 것은 skip
        * 필요한 데이터만 받돼, 
        * 스냅샷을 잘개 쪼개서 일부분만으로 검증이 가능하도록 함
    * 방법
        * 신뢰할 수 있는 헤더를 검증할 수 없어서 신뢰할 수 있는 곳으로부터 받아ㅗㅁ
    * 머클 파트리시아 트리 사용
        * 값을 가진 노드 = leaf
        *  특징
            * 파트리시아 트리의 특징
    * chunck
        * 핵심
            * 스냅샷을 청크 단위로 쪼개서 검증 받음
            * 청크 루트의 해시와 청크루트를 가짐
            * 4뎁스까지 한 청크에 넣음
        * 특정
            * 청크의 터미널 노드들을 모았을 때 청크의 청크루트는 머클루트와 일치해야 한다. -> 검증가능
        * 과정
            * 청크를 다운받아 머클루트와 비교해 같은지 확인
        * 구현
            * 이슈
                1. 벨리테이터 셋을 다이너믹하게 바꾸는데 한시간 마다 벨리데이터들이 일 하고 보상을 받음 -> 보상을 계산하는데 문제가 잇었음
                    1. 현재 블록의 시작 지점에서 과거 블록에 보상함
                2. 비트코인 타임록 스크립트 관련