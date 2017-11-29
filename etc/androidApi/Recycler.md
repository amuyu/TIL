# Recyclerview.Recycler
A Recycler is responsible for managing scrapped or detached item views for reuse.

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
Set the maximum number of detached, valid views we should retain for later use.
### getScrapList
read only scrapviews 리스트를 리턴한다.
### validateViewHolderForOffsetPosition
Helper method for getViewForPosition
viewholder 에 대해서 isRemoved, indexout, isPreLayout, viewtype, id 를 확인한다.
position 정보를 사용해 adapter 로부터 관련 값을 뽑아내 비교한다.
### getViewForPosition
Obtain a view initialized for the given position.
// 0) If there is a changed scrap, try to find from there,
prelayout 상태일 때,
// 1) Find from scrap by position
### getScrapViewForPosition
Returns a scrap view for the position. If type is not INVALID_TYPE, it also checks if
ViewHolder's type matches the provided type.
mAttachedScrap, mChildHelper, mCachedViews 에서 찾는다.
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
