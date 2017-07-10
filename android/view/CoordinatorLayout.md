# CoordinatorLayout
터치 이벤트와 관련된 여러가지를 제어할 수 있게 해준다
자식뷰의 스크롤 변화 상태를 다른 자식뷰들에게 전달 해주는 역할을 한다
ex) 플로팅버튼 + 스낵바를 같이 띄우는 경우 스낵바가 떠오르는 경우 플로팅 버튼이 자동으로 올라간다
스크롤 할 때 각 구성요소가 화면 밖으로 스크롤 되거나 상단에 고정에 됬다가 내려오도록 제어가 가능하다

# behavior
스크롤 시 다양한 반응을 위한 테크닉인 Behavior라는 개념이 도입되었다.
- android.support.design.widget.AppBarLayout$ScrollingViewBehavior
- android.support.design.widget.BottomSheetBehavior

# 스크롤 이벤트
CoordinatorLayout 은 layout_behavior 에 정의된 레이아웃으로 스크롤 정보를 전달하는 역할을 한다
그러면 AppBarLayout 의 ScrollingViewBehavior 가 정보를 받아서 AppBarLayout 을 변형하도록 한다

## 속성
### layout_behavior
해당뷰와 스크롤 이벤트를 연결하기 위한 속성 동일 레벨 계층뷰에 스크롤을 처리할 클래스명 기입

# AppBarLayout
스크롤 되는 뷰의 스크롤 이벤트를 처리한다.
## 속성
### layout_scrollFlags
- scroll : 스크롤이 되어야 할 뷰에 반드시 설정
- enterAlways : 아래로 스크롤시 해당뷰가 나타난다
- exitUntilCollapsed : 해당 뷰에 minHeight을 정의, 최소 높이를 지정한 값에서 더이상 스크롤 되지 않는다.
- enterAlwaysCollapsed : 해당 뷰에 minHeight 속성의 크기로 시작해 맨위로 스크롤이 될때만 전체높이로 확장하게 된다.


# CollapsingToolBarLayout
Toolbar를 확장하는 용도로 사용하는 레이아웃
## 속성
### layout_collapseMode
- pin : CollapsingToolbarLayout 이 완전히 축소되면 툴바는 화면위에 고정되고 보여진다.
- parallax : 스크롤되는 동안 스크롤과 약간 어긋나도록 화면이 보여진다.
### layout_collapseParallaxMultiplier
0.0~1.0 사이의 값을 통해 스크롤시 값에 도달하게 되면 지정한색이 오버레이되어 자연스럽게 ToolBar로 변화 애니메이션을 나타낼 수 있다.

# tip
## tablayout behind content
```xml
app:layout_behavior="@string/appbar_scrolling_view_behavior"
```

## 참고
[Android Design Support Library(II) – CoordinatorLayout](http://www.kmshack.kr/2015/09/android-design-support-libraryii-coordinatorlayout/)
레이아웃 속성들에 대해 설명
[CoordinatorLayout Behavior를 이용해 FooterView 숨기기/보여주기](http://gun0912.tistory.com/42)
behavior를 사용한 이벤트 처리
[Floating Action Buttons 가이드](https://github.com/codepath/android_guides/wiki/Floating-Action-Buttons)
[Demos the new Android Design library](https://github.com/chrisbanes/cheesesquare)
[mastering-coordinator](http://saulmm.github.io/mastering-coordinator)
[CoordinatorLayout과 behavior 관계](http://www.kmshack.kr/tag/coordinatorlayout/)
