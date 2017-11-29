## Paging Library
room 과 같이 쓰면 좋다
Twist query data for pagination (일반 쿼리로 paging 쿼리 처럼 사용 가능하다.)
Query by page (스크롤 이벤트 감지에 따라 처리가 필요한데 알아서 해준다.)
Check item diff (페이지 이동 중, 데이터 갱신이 발생했을 때,, 중복 등의 처리해준다.)
### 구성
PagedListAdapter, PagedList, DataSource
#### dataSource
데이터를 load
#### PagedList
Data 를 들고 있음.... DataSource 를 만들어서 자동으로 Data를 읽어들임
Configure 필요(page size, prefetch distance)
LivePageListProvider ,, 위의 것들을 Build 해준다.
#### PagedListAdapter
중보긍로 가져오는 값을 체크해준다.
#### 예시
Room 에서 LivePagedListProvider로 변환해준다.
Query 를 Provider에 넣어주
```java
// DataSource
@Query("...")
LivePagedListProvider<T> get();

// LiveData
LiveData<PagedList<T>> lists = PagedList.Config.Builder()
  .setPageSize(50)
  .setPrefetchDistance(50)
  .build()

// PagedListAdapter  
DiffCallback 을 정의해야 값의 중복을 체크해준다.(DiffUtil)
```



## 참고
[paging](https://developer.android.com/topic/libraries/architecture/paging.html#overview)
