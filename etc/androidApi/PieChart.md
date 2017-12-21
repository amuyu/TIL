# PieChart
pieChart 는 ViewGroup 의 subclass로 childView로 PieView 와 PointerView를 갖는다.
하나의 pie는 선택되었을 때 보여줄 label과 전체에서 갖는 비중 value와 pie에 대한 색깔 정보를 갖는다.

PieChart 는 chart 에 표시할 item 을 추가하고 입력한 내용을 표시한다.
PieChart 는 scroll과 fling 으로 회전을 시킬수 있고 회전이 정지되면 pointerview 가 가리키는
아이템 label을 화면에 표시한다.

PieChart 의 크기는 `View.onSizeChanged` 가 호출될 때, padding 을 고려해 w와 h을 구한다.
그리고 PieView는 원 모양을 그리기 위해서 w, h 을 비교해 작은 쪽 사이즈에 맞춰 PieView가 그려질
영역을 설정한다. PointerView 는 PieView 의 center 로부터 적절히 떨어진 곳에 위치하도록
영역을 설정한다. 각각의 영역을 계산한 후, `View.layout` 명령을 호출해 childView 의 크기와 위치를
설정한다.

Pie를 그리기 위해서 `canvas.drawArc` 를 사용하고 아이템이 추가될 때 마다 pie를 그리기 위해
필요한 startAngle, endAngle 을 계산한다.

PointerView 는 `canvas.drawLine` 을 사용해 pie 를 가리키기 위한 선을 그린다.

PieChart 는 chart 영역을 스크롤 할 수 있다. 스크롤 시, chart가 회전하도록 만들기 위해서
`GestureDetector`, `Scroll`, `ValueAnimator`, `ObjectAnimator` 를 사용한다.
