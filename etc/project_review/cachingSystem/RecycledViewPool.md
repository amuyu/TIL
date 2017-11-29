# Recyclerview
Recyclerview 에서 사용하는 pool

## 변수
### mScrap (SparseArray)
recycle 할 viewholder 를 scrap,
viewtype 별로, 저장하기 위해 SparseArray<ArrayList<ViewHolder>> 형태를 가진다.
key는 viewtype으로 value viewholder로 사용
### mMaxScrap (SparseIntArray)
scrap할 수 있는 최대 수치를 설정

## 함수
### setMaxRecycledViews
scrap 할 max 값을 설정한다.
mScrap 의 크기가 설정한 max보다 크면 제거한다.
### getRecycledView
recycle할 view를 호출한다.
mScrap 에서 viewtype 에 해당하는 view list를 가져와 마지막 위치의 view를 리턴한다.
### putRecycledView
scrap 할 view 의 viewtype 에 해당하는 scrapHeap 을 호출하고 max 값보다 작으면
scrapHeap 에 view를 scrap 한다.
### onAdapterChanged
old adapter는 detach 하고 new adapter 는 attach 한다.
swap adpater? adpater를 변경한다?
adapter 가 change된다는게 뭐지?
### getScrapHeapForType
mScrap 에서 viewtype 에 해당하는 scrap list를 호출
만약 list 가 없으면 list 를 추가함
### clear
mScrap.clear 함

### 정리
기본 동작은 put, get, clear
max 를 설정하도록 해서 max 값 이상 추가되지 않도록 함
SparseArray를 사용 (일종의 Hashmap)
