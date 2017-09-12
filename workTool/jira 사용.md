## 이슈트래커
이슈를 관리하는 도구
## 이슈트래커 사용 장점
- 특정이슈 발견, 해결, 상태 관리
- 이슈에 대한 역할과 임무 부여, 커뮤니케이션 효과적
- 이슈 해결에 대한 히스토리 관리

## Jira 사용법
### 용어 사전
용어명|설명
---|---
Project|이슈를 등록할 가잔 큰 카테고리
Issue|해당 프로젝트의 버그나 개선사항, 문의사항으로 가장 작은 단위
Leader|프로젝트나 컴포넌트를 관리하는 사람
Reporter|이슈를 등록하는 사람
Assignee|이슈를 문의 받은 사람
Watcher|이슈를 참조하는 사람
Filter|이슈를 검색조건에 맞게 정하는 규칙
Type|이슈의 성격, 버그, 기능수정, 문의사항 등
Permlink|화면의 바로가기 url


### 이슈 생성
상단에 Create 버튼을 선택하여 이슈를 생성한다.
이슈 생성 시 작성해야 할 항목은 다음과 같다.
- project
- issue type : bug(문제점)), task(업무), story, improvement(개선), new feature(새 기능)
- summary
- assignee
- descriptioin
- component : design,develop
- fix version
- priority
  - 긴급도
  - 중요도
- sprint
- linked issue
- issue
- epic link
- status
- resolution(처리상태)


### 이슈 처리 프로세스 요약
1. PM이 Epic/User Story를 만들고, 해당하는 Task들을 생성
2. Task에 업무에 대한 설명을 적고, 업무 담당자에게 배분
3. 담당자가 해당 Task를 받은 후 'In Progress' 상태로 , 담당자는 진행 내용이나 의사 소통이 필요한 정보를 Comments로 적거나 파일을 첨부
4. 담당자는 업무가 끝나면 'Reslove Issue'상태로 PM에게 다시 이슈를 전달
5. PM은 이슈가 제대로 반영되었는지 확인한 후, 일감을 닫은 Close 상태로 변경

## 참고
[Jira를 통해 프로페셔널하게 프로젝트 협업하기](http://uxd.team.handstudio.net/post/64286399069/jira%EB%A5%BC-%ED%86%B5%ED%95%B4-%ED%94%84%EB%A1%9C%ED%8E%98%EC%85%94%EB%84%90%ED%95%98%EA%B2%8C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%98%91%EC%97%85%ED%95%98%EA%B8%B0)
[JIRA 사용 설명서 및 tips](https://www.lesstif.com/pages/viewpage.action?pageId=12943700)
