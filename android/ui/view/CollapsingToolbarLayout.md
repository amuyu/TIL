# CollapsingToolBarLayout
CollapsingToolBarLayout은 보통 CoordinatorLayout과 AppBarLayout의 child 로 사용합니다.
이 레이아웃은 스크롤 이벤트(android.support.v4.widget.NestedScrollView)를 받아서
헤더부분을  Collapse 하거나 펼치거나 합니다.
## build.gradle
android design 라이브러리 추가
```groovy
dependencies {
    compile 'com.android.support:design:22.2.1'
}
```

## contentScrim
collapse 되었을 때, drawable 설정
## scrollFlag
헤더에 대한 스크롤 동작 설정
- scroll : 스크롤 이벤트에 반응할 view에 설정
- enterAlways : 아래쪽 방향으로 스크롤 할 때 마다 발생
- exitUntilCollapse : minHeight 까지 축소
- enterAlwaysCollapse : minHegiht 으로 시작해 맨위로 스크롤 될 때만 전체 높이로 확장
## collapseMode
- pin : layout 이 collapse 되면 툴바는 화면위에 고정된다.
- parallax : 툴바가 축소되는 동안 parallax 모드로 동작하도록 한다. ex) 툴바가 축소되면 이미지도 작아지면서 사라진다.
## behavior
이 속성이 있어야 스크롤링 할 때 이벤트가 발생한다.

# 참고
[Android Support Design CollapsingToolbarLayout 사용하기](http://iw90.tistory.com/228)
[CoordinatorLayout 활용](http://freehoon.tistory.com/38)
