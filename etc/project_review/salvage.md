
# Salvage
Generic view recycler and ViewPager PagerAdapter implementation.
[salvage](https://github.com/JakeWharton/salvage)
pager의 view type을 구별해서 view를 재사용할 수 있도록 구현한 라이브러리



## RecycleBin
view 의 재사용을 위해 만든 클래스, AbsListView 에 구현된 RecycleBin 참고
ActiveViews, ScrapViews
ActiveViews : displaying view
ScrapView : old View
### Adapter 에서 사용하는 함수
setViewTypeCount, getScrapView, addScrapView, scrapActiveViews
### setViewTypeCount
viewType의 count 개수 만큼 view를 scrap 할 array 생성, 초기화의 느낌
### getScrapView(position, viewType)
position에 해당하는 view get
#### retriveFromScrap()
scrapviews 는 sparsearray 로 position 정보를 key값으로 저장한다.
값이 일치하는 view 가 있으면 return, 없으면 null
### addScrapView
pager adapter 에서 item 이 destroy 될 때, view를 저장한다.
scrapviews 에 view 와 position 을 저장한다.
### scrapActiveViews
Activeviews 를 scrapviews 로 이동한다.
### pruneScrapViews
activeviews 빼고 old view 들은 다 제거한다.

### 정리
view를 재사용 하려면 어떻게 해야할 까?
view들을 저장하고 불러온다.

### 의문점
activeViews 에는 값을 넣은 적이 없다....
원래 AbsListView에서는 `fillActiveViews` 에서 activeview 에 값을 넣어 주었다.
여기서는 이 부분이 생략된듯 listView와는 다르게 pager의 경우, 한개의 아이템만을 보여주기 때문에
굳이 저장할 필요가 없을지도...
