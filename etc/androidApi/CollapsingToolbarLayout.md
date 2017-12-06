# CollapsingToolbarLayout
CollapsingToolbarLayout는  스크롤에 따라 축소 및 확장하는 Appbar를 구현한 클래스다.

레이아웃이 완전히 보일 때는 확장된 큰 영역을 보여주다가
화면이 스크롤되면서 layout이 축소되고 작아지는 영역을 보여준다.

이 layout을 사용하기 위해서는 AppCompatTheme를 사용해야 한다.



## method
### onAttachedToWindow
window 에 appbar 를 배치하기 위한 사전작업을 한다.
```java
((AppBarLayout) parent).addOnOffsetChangedListener(mOnOffsetChangedListener);
// appbarlayout 의 vertiicalOffset 변화 event를 호출받는다.
ViewCompat.requestApplyInsets(this);
// View.onApplyWindowInsets() 돌아가도록 요청한다.
// windowinset 셋팅은 appbar layout이 정확한 위치에 배치되도록 하는데 필요하다.
```

### OffsetUpdateListener#onOffsetChanged
AppbarLayout 이 변화할 때, OnOffsetChangedListener가 호출된다.
offset 값으로 scrim 을 적절하게 조절한다. 이 리스너를 등록하지 않으면 AppbarLayout은 화면 위로 스크롤된다.

scroll 에 따라 'mCurrentOffset' 값을 변화시킨다.
mCurrentOffset 값으로 현재 bar 의 height 을 구해 scrim 을 보여주는 상태를 변화시킨다.

### ViewOffsetHelper#updateOffsets
`offsetChanged` 에서 받은 offset 을 사용해서 toolbar 의 offset 을 조절한다.

### draw
mContentScrim, mCollapsingTextHelper, mStatusBarScrim 을 draw 한다.

- mContentScrim : scroll postion 에 따라 달라지는 tool bar에 대한 scrim
- mCollapsingTextHelper : title text
- mStatusBarScrim : scroll position 에 따라 달라지는 statusbar 에 대한 scrim

### drawChild
contentscrim 을 toolbar 뒤에 draw 위해 drawChild 를 override 해서 scrim을 먼저 draw한다.
이 때 호출하지 않으면 toolbar가 scrim을 덮어버린다..
이상한 점은 draw 에 있는 contentscrim draw하는 부분을 주석처리해도 drawChild를 사용해서 draw한다는 점....

### onMeasure
topInset 이 있는 경우, topInset 을 포함해서 heaight 을 조절한다.

### onLayout
childView 에 offset 설정
mDrawCollapsingTitle 배치

setCollapsedBounds 에서 solaapseBounds를 설정하는데 이 때, Top의 위치는
scroll 하면서 dummyView 가 height 이 줄어드는게 그 크기로 위로 이동한다는 점을 유의하면 될 듯 하다.

## etc
### setFitsSystemWindows
상태표시줄과 같은 시스템 View 를 고려할지 설정하는 부분
