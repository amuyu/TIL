# AdapterDelegate
## source
[AdapterDelegates](https://github.com/sockeqwe/AdapterDelegates)
## summary
하나의 리스트에 여러 형태의 item view 구성을 도와주는 라이브러리
RecyclerView 의 경우, 여러 형태의 view를 구성하기 위해
viewtype, viewholder, createviewholer, bindviewholder 를 수정하는데 이에 대한 코드를 Delegate 코드로 분리함

## 정리
### AdapterDelegate
This delegate provide method to hook in this delegate to RecyclerView.Adapter lifecycle.
abstract class
### AdatperDelegatesManager
This class is the element that ties {@link RecyclerView.Adapter} together with {@link AdapterDelegate}.
AdapterDelegate 관리
#### GenericType
Data Source Type을 Generic으로 사용
#### SparseArrayCompat
AdapterDelegate 리스트를 SparseArrayCompat 을 사용
Lint 에서 HashMap 보다는 SparseArray 를 추천함 (안드로이드에서는 performance가 더 높다고 한다)
#### viewtype
delegates 의 position 이 하나의 view type 이 된다. (delegate == viewtype)

## 배울 점
### Exception 처리
에러에 대해서 throw Exception 처리
### 주석
클래스, method 에 상세하기 주석
