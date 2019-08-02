# paging library 그것이 쓰고 싶다
카카오플레이스
## paging
SQLite - query 2MB 제한
server-client : paging은 server 에서 담당, server 에서 restapi 설계
ex) google, 10개만 주고 paging

# Android
Infinite Scrolling
## 기존 구현
스크롤 리스트 하단에 도달,, 하단 도달 전 미리 페이지 항목을 구현
하단에서 더보기 호출하면 > rest api 를 호출해서 다음 스크롤 데이터 추가
## Paging library
### PagedList,
lazyed list, chunk 단위로 로딩, 필요 시 데이터 로딩, 백그라운드 스레드 지원
Unbounded List, Coutable List
#### Unbounded List
리스트의 전체 사이즈가 상관 없을 때
#### Coutable List
전체 사이즈를 알 수 있을 때
### DataSource
PagedList 에 데이터 제공
PageKeyedDataSource, ItemKeyedDataSource, PositionalDataSource
#### PageKeyedDataSource
response 에 key 있을 때
#### ItemKeyedDataSource
현재 리스트의 마지막 Item 으로 호출
#### PositionalDataSource
startPosition, size 연속해서 가져올 필요가 없다 Countable List 와 사용하기 좋다
### PagedListAdapter
DiffUtil 을 사용해서 변경된 데이터만 업데이트

# 생각해 볼 것들
## Room 을 사용하지 않을 때
update, delete, reorder 에 대한 처리
## PagedList 의 아이템을 직접 수정할 수 없음
## Cursor의 쿼리 결과가 2MB를 넘지 않도록

# tip
reddit rest api 살펴보자 (PageKeydDataSource, ItemKeyedDataSource, PositionalDataSource)
github rest api

# blog
https;//medium.com/@jungil.han
