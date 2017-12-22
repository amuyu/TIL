# LinearLayoutManager
LayoutManager 를 subclass 로 ListView 처럼 구현한 클래스

orientation, reverseLayout, stackFromeEnd 등의 기능을 제공한다.
이들의 값에 따라 children 을 배치하는 방향이나 프로세스가 달라진다.

## onLayoutChildren
## 알고리즘
children view을 배치한다.
recyclerview 의 displatchLayout 에서 호출,
view가 초기 레이아웃을 필요할 때, 데이터 변경이 발생할 때,
스마트폰에서 보이는 영역 layout 을 갱신한다.
// layout algorithm:
// 1) by checking children and other variables, find an anchor coordinate and an anchor
//  item position.
// 2) fill towards start, stacking from bottom
// 3) fill towards end, stacking from top
// 4) scroll to fulfill requirements like stack from bottom.
## process
저장된 item 없으면 모든 childview 를 제거한다. `removeAndRecycleAllViews`
reverseLayout인지 확인 `resolveShouldLayoutReverse`
anchorinfo 갱신 `updateAnchorInfoForLayout`
공백 크기 계산 `getExtraLayoutSpace`
prelayout  일 경우, view 를 감안해서 위치 조정
childview 들을 detach하고 scrap 한다 `detachAndScrapAttachedViews`
layoutState 셋팅 `updateLayoutStateToFillEnd`
layoutstate 에 정의된 layout 을 채운다. `fill` 근데 왜 두번 씩 호출할까?
    view들을 recycle 한다. `recycleByLayoutState`
    view 를 추가 한다. addView, addDisappearingView `layoutChunk`
        view layout size를 assign 한다 `layoutDecorated`
새로운 아이템에 대한 애니메이션을 준다. `layoutForPredictiveAnimations`
## tip
LayoutState 를 사용
## mPendingSavedState
액티비티 상태를 복원하는 과정에서 write `onRestoreInstanceState`
초기 레이아웃을 구성할 때 사용하고 사용 후에 null write `onLayoutChildren`
## mPendingScrollPosition
scroll 한 위치 write `scrollToPosition`,`scrollToPositionWithOffset`

## resolveShouldLayoutReverse
orientation 이 horizontal 인데 direction 이 rtl이면 reverseLayout 으로 자동 변경하기 위한 method
orientation : HORIZONTAL, VERTICAL
isLayoutRTL : Right to left, Left to Right
horizontal && rtl 이면 !reverseLayout
vertical 인데 ltr 이면 reverseLayout

# fill
주어진 layout 에 view를 추가
available 공간을 구하고 view를 하나씩 추가하면서 available 감소
LayoutChuckReuslt, LayoutState, RecyclerView.State
# LayoutState
Helper class that keeps temporary state while {LayoutManager} is filling out the empty space
### mAvailable
view 사용 공간?
## method
### hasMore
item 더 있는지 여부 확인
### next, nextViewFromScrapList
layout 해야 하는 다음 view를 얻는다.
### assignPositionFromScrapList, nextViewInLimitedList
mCurrentPosition 갱신


# AnchorInfo
Simple data class to keep Anchor information
## method
### assignCoordinateFromPadding
mCoordinate 갱신
### isViewValidAsAnchor
view 에 대한 유효성 확인, 없어지는 view인지 position 에 들어오는지
### assignFromViewAndKeepVisibleRect
mPosition 과 mCoordinate 갱신


# OrientationHelper
Helper class for LayoutManagers to abstract measurements depending on the View's orientation.
view 에 대한 size, start, end, padding, offset 구하는 클래스
## method
### getTotalSpaceChange
Returns the layout space change between the previous layout pass and current layout pass.


# LayoutChunkResult




# public LinearLayoutManager(Context context, int orientation, boolean reverseLayout)
setOrientation(properties.orientation);
setReverseLayout(properties.reverseLayout);
setStackFromEnd(properties.stackFromEnd);
setAutoMeasureEnabled(true);

## 참고
[building a recyclerview layoutmanager part1](http://wiresareobsolete.com/2014/09/building-a-recyclerview-layoutmanager-part-1/)
