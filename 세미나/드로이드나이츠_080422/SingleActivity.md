# 필요성
## Don't keep Activity
- 수많은 QA 이슈를 낳는 존재
- Result 처리 이슈
- 저사양 단말이 많은 지역적 특성
## I HATE Fragments!
IllegalStateException!!
`onSaveInstanceState` 액티비티가 메모리에서 복원되는 과정에서 발생
## Map Based Application
- Map 초기화 과정에서 발생하는 높은 리소스 비용
- Map 동작을 위한 반복적인 코드의 호출
## 확작성의 문제
- 배달, 배송, 택시, 결제, 멤버쉽 등등 서비스 무한 확장
- 새로운 기능마다 액티비티 구현 시 데이터 교환 문제
- 1개 수정에 수많은 사이드 이펙트
## 팀 매니지먼트의 문제
- 약 30명 for Passenger (개발자)
- 지금도 매달 1~2명씩 조인
- On-boarding 과정에서 소모되는 비용
- 각 엔지니어링, 도메인 별로 전문성 강화가 필요

# 고려
## State Machine과 View Updator
- View 를 갱신하기 위한 Core
- 상태는 뭔지?
- Updator?
## UI 변경의 자유도
- 쉽게 UI 조합 및 변경 가능
- 낮은 러닝커브 가능
- UX 기반 아키텍쳐
## 맵의 단일화
- Map 의 접근하는 단일 컨트롤러
## 데이터의 교환
- 뷰간 데이터 교환 방법은?
- Activity 를 호출할 때는?
- Don't keep Activity?
## 기타
- Testable
- 빌드 속도의 개선
- 모듈라이제이션 지원

# 실제 구현
## State Machine 과 View Updator
## View based Screen
- CustomView Inflate
- Create-Destory
- Save-Resotre 사용
## View Updator
- 독립적으로 로직 수행 후 Broadcasting
- ViewModel 이 결과를 받아서 View 갱신
- Databinding 활용
- View 는 껍데기
## 라우터완 노드
- ux 기반 라우터 테이블
- 화면 다위를 관리하는 노드
- 노드의 상태 저장/복원 지원
## 맵 접근성
- 단일 컨트롤러
- 논리적 Layering
- View와 독립적 동작
## 다양한 인터렉션 대응
- back key 이벤트
- 액티비티와의 통신
- 데이터 일시 저장과 복원
- DeepLink 처리
- Rx, Activity Lifecycle

# 실제 구현
## State Machine
리덕스와 유사
## 라우터와 노드
우버 rips? lips?
트리 구조로 view 관리
노드 : 라우터, 뷰, 뷰모델 관리
라우터 : 어떤 노드를 화면에 표시할 지,,,,
## 노드
ux에 따라서 라우터를 변경하면 된다
노드는 독립적으로 운용한다
## Layering Map
Map Feature 별로 Layer 로 구분
## Activity Result Manager
onActivityResult 를 사용하지 않고,,,별도의 리스너 사용
## Abount Learning Curve
- Dagger, Node, Router, ViewModel, VIew, Layout 자동 생성 플러그인 개발(Android Studio)
- Router 학습 프로젝트

# 코드
모듈이 64개 ㄷ

# 제안
Conductor by BludLineLab
RIBs by Uber
Coordinator by Square







# etc
architecture fixed >
