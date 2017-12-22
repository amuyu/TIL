# Recycler
재사용을 위해 scrapped/detached view 들을 관리한다.

viewholder를 다음의 리스트들로 mAttachedScrap, mChangedScrap, mCachedViews 관리한다.


## 변수
### mAttachedScrap (ArrayList)
### mChangedScrap (ArrayList)
### mCachedViews (ArrayList)
### recycledViewPool
### ViewCacheExtension
ViewCacheExtension is a helper class to provide an additional layer of view caching that can
ben controlled by the developer.

## 함수
### clear
clear scrap views out of this recycler.
mScrapviews 는 clear 하고 mCachedView 의 view를 recycledviewpool 로 넘긴다

### setViewCacheSize
recyclerviewpool 로 넘겨 재사용 하기 전에 Cache 할 size 를 잡아둔다.
`updateViewCacheSize`를 호출한다.

### getScrapList
read only scrapviews 리스트를 리턴한다.
LinearLayoutManager 의 경우, 이 리스트를 가지고 layout 을 배치한다.

### validateViewHolderForOffsetPosition
viewholder 에 대해서 isRemoved, indexout, isPreLayout, viewtype, id 를 확인한다.
position 정보를 사용해 adapter 로부터 관련 값을 뽑아내 비교한다.
viewholder 에 대한 유효성 검사

### bindViewToPosition(view, position)
item 과 view를 연결하는 작업을 한다.
view는 onCreateViewHolder 로 생성했거나 getViewForPosition 으로 찾았을 것이다.
getViewForPostion 으로 받는 view는 이미 bind 되어 있기 때문에 다른 position에 연결할 상황이 아니면 호출할 필요가 없다.

item과 view를 연결하기 위해 position(view에 연결할 item 의 위치) 에 대해서 offsetPosition 을 구하고
(prelayout 과 postlayout 에 따라서 item의 position 이 달라지는 듯)
adapter 의 bindViewHolder 를 호출한다. 그리고 view 의 layout parameter 를 설정하는 작업을 한다.

### getViewForPosition(position, dryRun)
position 에 대한 view를 가져온다. 아마도 Recycler 클래스의 핵심이지 않을까 싶다.

먼저, prelayout 일 경우에 다음을 호출한다. `getChangedScrapViewForPosition`
`getChangedScrapViewForPosition` 는 mChagedScrap 에서 같은 position 을 갖는 ViewHolder 를 찾는다.

그 다음 `getScrapViewForPosition` 을 호출해 mAttachedScrap, childeHelper.hiddenView, mCachedViews 에서
ViewHolder 를 찾는다. Viewholder 를 찾았다면 viewholder 가 유효한지 확인한다.

여기서도 못 찾으면 mAttachedScrap, mCachedViews 에서 adatper 에서 호출한
item의 id와 type 으로 viewholder 를 찾는다.

여기서도 못 찾으면 mViewCacheExtension 에서 view를 찾아 holder 찾는다.

여기서도 못 찾으면 RecycledViewPool 에서 찾는다.

여기서도 못 찾으면 adapter.createViewHolder 를 호출해서 ViewHolder 를 만든다.

그리고 holder 에 대해서 animation, bind, layoutparame 을 설정하고 view를 리턴한다.

### scrapView(View view)
holder 의 flag 에 FLAG_REMOVED 와 FLAG_INVALID 가 있으면 mAttachedScrap 에 추가하고
그렇지 않으면 mChangedScrap 에 추가한다. `detachAndScrapAttachedViews` 에서 호출한다.

`detachAndScrapAttachedViews` 는 onLayoutChildren 에서 호출된다.
일시적으로 모든 views 을 scrap 하고 detach 하는 메소드이다.

### getChangedScrapViewForPosition(position)
mChangedScrap 에서 해당 위치의 viewholder 를 찾는다. 찾게 되면 holder의 flag를
FLAG_RETURNED_FROM_SCRAP 으로 변경한다.

### getScrapViewForPosition, getScrapViewForId
mAttachedScrap, mChildHelper, mCachedViews 에서 viewholder 를 찾는다.


### bindViewToPosition
Binds the given View to the position. The View can be a View previously retrieved via
{@link #getViewForPosition(int)} or created by
{@link Adapter#onCreateViewHolder(ViewGroup, int)}.
### convertPreLayoutPositionToPostLayout
### attachAccessibilityDelegate
### invalidateDisplayListInt
### recycleView
Recycle a detached view.
### scrapView
Mark an attached view as scrap.

# 참고
## state
Contains useful information about the current RecyclerView state like target scroll
position or view focus.
### mInPreLayout
mRunPredictiveAnimations이 true 일 때, true 로 변경되는 듯,,, in {onMeasure, dispatchLayout}
processAdapterUpdatesAndSetAnimationFlags 호출 후, mRunPredictiveAnimations이 값이 변경된다.


=============
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
