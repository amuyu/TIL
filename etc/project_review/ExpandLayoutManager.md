# ExpandLayoutManager
RecyclerView.LayoutManager 의 subclass 로 ReyclerView 의 item을 선택했을 때,
item과 item 사이의 공간이 펴지면서 item의 상세 내용이 표시되는 UI 이다.

## 사용 방법
ExpandLayoutManager 는 별다른 설정없이 `RecyclerView.setLayoutManager` 를 호출해 설정한다.
ExpandLayoutManager 클래스는 다른 LayoutManager 과 사용 방법이 다를게 없다.

ExpandLayoutManager 과는 다르게 RecyclerView 에서 표시하는 Item의 Layout은 기존과는 다르게 작성해야 한다.
Item의 Layout은 FrameLayout의 subsclass인 `SimpleAnimationView` 를 root 로 한다.
child layout으로 LinearLayout 으로 orientation=vertical 로 설정해 접혀있을 때,
view 영역과 펼쳐졌을 때 보여줄 영역을 작성한다.
펼쳐졌을 때 보여주는 영역의 id는 반드시 `view_to_animate`로 해야한다.

## SimpleAnimationView
FrameLayout 의 subclass 로 recyclerview.adapter 에서 클릭 이벤트가 발생했을 때,
ExpandLayoutManager로 이벤트를 전달한다. 그리고 view 가 접혀지고 펼쳐지는 동작을 수행한다.






LayoutHelper, ExpandModel

# ExpandModel
interface

# SimpleExpandModel

# LayoutHelper



# SimpleAnimationView

## 참고
[ExpandLayoutManager](https://github.com/Azoft/ExpandLayoutManager)
