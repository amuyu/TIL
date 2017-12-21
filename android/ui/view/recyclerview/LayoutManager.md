# RecyclerView.LayoutManager
LayoutManager 는 recyclerview 의 item views에 대한 수치 조절과 배치를 한다.
그리고 사용자에게 더 이상 보여주지 않는 view에 대해서는 recycle 할지를 결정한다.
LayoutManger 를 수정해서 Recyclerview 를 세로, 그리드, 가로로 스크롤 하는 리스트를 구현할 수 있다.

## Method
### setMeasuredDimensionFromChildren
자동 측정을 사용하는 경우 레이아웃이 계산된 후 호출된다. (ReyclerView#onMeasure)
bounding box 를 계산하기 위해 모든 children 을 탐색하고 setMeasuredDimension(rect, intent) 을 호출한다.
LayoutManagers는 이 method 를 override 해서 dimension을 다르게 처리한다.

### setMeasuredDimension
children bounding box 와 layout 의 width 와 height 으로부터 dimensions을 정한다.
GridLayoutManager의 경우 이 함수를 override 해서 grid 형태로 바꾼다.

### onLayoutChildren
Adapter 로부터 받은 모든 child views 들을 배치한다.
layoutmanager 는 item animation의 behavior 를 담당한다.
item 이 add/remove/disappearing/move 되는 동작에 대해 default animation 을 동작하도록 한다. (DefaultItemAnimator)

LayoutManager 에서 좀 더 멋진 animation 을 원한다면 supportsPredictiveItemAnimations 에서
true를 return 하도록 하고 onLayoutChildren 에 로직을 추가한다. Supporting predictive animations
는 onLayoutChildren 가 두 번 호출되게 한다. `pre` layout, `real` layout 에서 호출된다.

### onLayoutCompleted
레이아웃 계산이 끝나면 호출된다. 이 곳에서 pending scroll postion 같은 값들을 정리하기 좋다.


# ChildHelper
recyclerview 에서 children 을 관리하는데 도움을 주는 클래스이다.
recyclerview 를 wrap 하고 children 을 숨기는 기능을 가지고 있다.

ChildHelper 는 children과 관련된 method 들인 addView, removeView, hideView 등 을 제공한다.
RecyclerView로 가기 전 ChildHelper 를 거치면서 child view 의 view들의 index 와 hide 여부를 관리한다.

ChildHelper 는 Callback, Bucket, hiddenViews 를 멤버로 가지고 있다.
Callback 을 통해 RecyclerView 의 함수를 호출한다. (wrap)
Bucket은 Index를 offset하는 method 를 제공한다.

## Bucket
ChildHelper 의 inner class로 index를 offset 하는 메소드를 제공한다.
view 가 추가될 때 만다 view에 대한 index 를 bit로 변경해 값을 저장해서
view가 있는지의 여부와 view가 show인지 hide 인지 구분할 수 있다.



## Recycler
Your LayoutManager is given access to a Recycler instance at key points in the process when you might need to recycle old views, or obtain new views from a potentially recycled previous child.
### getViewForPosition
## Detach vs Remove
Detach is meant to be a lightweight operation for reordering views.
Remove is meant for views that are no longer needed. Any view that is permanently removed should be placed in the Recycler for later re-use,
## Scrap vs Recycle
Recycler has a two-level view caching system: the scrap heap and the recycle pool.
layoutmanager 에 view를 제공할 때, 먼저 scrap heap에 있는지 확인하고, 없으면 recycle pool 에서 view를 가져오고 recycle pool 에도
적절할 view가 없으면 새로 view를 생성한다.


# RecyclerView Animations Part 1
## removeView,,
리스트로 {a,b,c,d,e} 를 가지고 있고 view에는 리스트 중에서 4개{a,b,c,d} 만 보이는 상황에서
b를 지우게 되면(or 사라지게 되면) b는 view에서 제거되고 안보이던 e가 추가된다. {a,c,d,e} 이 때, e가 나타면서
새로 나타나는 애니메이션 효과(fade-in)을 주게 되면 리스트에 e가 추가된 것으로 착각하게 할 수 있다.
(e는 원래 있던 데이터이기 때문에...)
이를 처리하기 위해 recyclerview 에서 predictive layout logic 을 사용한다.
recyclerview 는 변화가 생기기 전 view의 위치과 변화 후에 view의 위치를 알아야한다.

1) prelayout
이를 쉽게 알기 위해 `two step layout process`를 사용한다. {prelayout, postlayout}
c가 제거됐고, 레이아웃을 다시 그리게 되면 layoutmanager는 c가 제거됐다는 사실을 알고 있고
c가 제거된 space를 채우려고 한다. recyclerview 는 아직 c가 있는 상태로 view를 리턴한다.
그리고 리턴한 view에는 `isItemRemoved` 라는 method를 가지고 있고 layout manager는
이 method 로 사라지는 view인지 확인할 수 있다.

2) postlayout
recyclerview 는 다시 레이아웃 하도록 layoutmanager에 요청할 수 있고
이 단계에서 Adapter에서 C는 제거되었지만 view에는 나타나있으므로 c가 여전히 있는 것처럼
행동할 수 있다.
이 두 layout 을 pass 해서 recyclerview 는 맞는 애니메이션을 알 수 있다.

## disappearingView
리스트로 {a,b,c,d} 를 가지고 있고 view에는 리스트 중에서 4개{a,b,c,d} 만 보이는 상황에서
a 뒤에 x를 추가하게 되면 view 에는 {a,x,b,c}가 보이게 되고 d는 view에서 사라지게 된다.
하지만 d는 실제 adapter에서 제거된 상태는 아니다. 이를 위해서
postlayout 단계에서 getScrapList라는 api 를 추가했다. 그리고 addview대신에
addDisappearingView

# RecyclerView Animations Part 2
predictive animation 하는 동안 (view는 아직 제거되지 않은 상태) layoutManager가
`getChildCount()`를 호출하면 Recyclerview는 제거된 후의 개수를 호출한다.
`addView`를 호출해도 index를 적절하게 offset 해서 ViewGroup#addView를 호출한다.
이러한 과정을 자세히 보려면 childhelper 를 살펴봐야 한다.
Adapter가 notify** events를 dispatch 하면 recyclerview 는 기록한다. 그리고
다음 레이아웃 pass전에 발생한 이벤트들을 반영한다.


# Anatomy of RecyclerView Part 1
## Aspects
ViewHolder recycling, hidden views, predictive animations stable ids
state, prelayout, scrap
## RecycledViewPool
The pool is a place for the views we know to be "dirty" and requiring rebinding.
## prelayout, postlayout, predictive animation
adapter 가 변경되고 Recyclerview는 layoutmanager에 두 개의 레이아웃을 요청한다.
prelayout 은 변경 전 어댑터의 상태에 해당하는 레이아웃을 배치한다. extra view를 구성한다.
postlayout 은 변경 어댑터의 상태에 해당하는 레이아웃이다.
prelayout 과 postlayout을 비교해서 적절한 animate를 줄 수 있다.

예를 들어 a,b,c 를 list로 갖는 view가 있다고 하자, layout 에는 a,b가 보이는 상태에서
b를 제거할 경우 다음과 같이 layout 변화가 생긴다.
previous layout : a | b
pre layout : a | b | c // b가 제거될거라는 걸 알고 있다.
post layout : a | c
## View Cache
RecyclerView.Recycler class 의 mCachedViews 는 "first level cache" 이다.
cache 에서 viewholder를 찾을 때, position 값이 중요하다, cache 에 있는 view는
rebinding 없이 그대로 사용하기를 기대한다. 이러한 내용을 정리해보면 다음과 같다.
If a ViewHolder was found nowhere, it is created and bound.
If a ViewHolder was found in pool, it is bound.
If a ViewHolder was found in cache, there is nothing to be done.
## Filling pool and cache
cache 가 찰 때 까지는 cache에 채운다. cache 가 가득차면 pool로 밀어낸다.
## Pool and cache in Action
## ViewCacheExtension
고정된 위치에 값이 변하지 않는 view에 대한 cache를 사용할 때,,,,, 이용하나보다.
Custom 으로 사용자가 `recyclerView.setViewCacheExtension` 해서 사용
## Hidden Views
predictive animation 을 위해 사용
childhelper 의 멤버변수,
## Scrap
layoutmanager 가 layout을 시작할 때, 모든 views 들을 scrap 한다.
(`detachAndScrapAttachedViews`)
그 다음 Scrap list에서 views를 찾아 layout 하고 필요없으면 버린다.
스크랩의 존재이유는 LayoutManager와 Recyclerview.Recycler 사이의 관심사가 분리되서이다.
특정 child가 유지해야 하는지 제거되는지 pool이나 cache에서 가져오는 등의 작업을 Recycler에서 하기 때문에 LayoutManger는 childview 들에 대해서 알 필요가 없다.
There is just one difference in the usage of changed scrap and attached scrap: attached scrap can be used in both pre- and post- layout, while changed scrap — only in pre-layout
## Stable ids
`notifyDataSetChanged()` 동작할 때 사용
장점은 첫 번째로, pool 이 넘칠 문제가 없다. 두 번째로, animations을 얻는다. (non-predictive animation)


## 참고
[building a recyclerview layoutmanager part1](http://wiresareobsolete.com/2014/09/building-a-recyclerview-layoutmanager-part-1/)
[RecyclerView Animations Part 1 - How Animations Work](http://www.birbit.com/recyclerview-animations-part-1-how-animations-work)
[Anatomy of RecyclerView, Part 1 = a Search for a ViewHolder](https://android.jlelse.eu/anatomy-of-recyclerview-part-1-a-search-for-a-viewholder-404ba3453714)_
