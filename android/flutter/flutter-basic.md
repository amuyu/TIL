
# 기본 위젯
* Text : 스타일 텍스트 
* Row, Column : 가로, 세로 방향 flex 위젯
* Stack : 페인트 순서로 서로 위에 배치할 수 있다.
* Container : 직사각형 시각적 요소

# Material Components
* MaterialApp : material app 을 시작
* Navigator : 화면 전환
* AppBar 
* Scaffold 

# handling gesture
* GestureDetector : `onTap`, `onPressed`
[gestures](https://flutter.dev/docs/development/ui/advanced/gestures)

# Changing widgets in response to input
StatefulWidgets 을 사용해서 state 를 관리할 수 있다.
state 를 수정해서 widget 을 변경할 수 있다.

# layout
## 기본 구성
- visible widget
- layout widget
- page

## layout patterns (배치)
- Row : ListTile
- Column : ListView

### align
- Main Axis : space Evenly, center, min
- Cross Axis

## Sizing Widget
- expaned

## common layout widgets
widget catalog : https://flutter.dev/docs/development/ui/widgets
search api : https://api.flutter.dev/index.html

### Standard widgets
- container : padding, borders, margin 을 사용할 수 있습니기
- gridview
- listview
- stack  

### Material widgets
- Card
- ListTile : 최대 3줄의 텍스트와 앞뒤 icon 을 행으로 구성합니다.

# Box Constraint 다루기
flutter 에서 위젯은 기본 RenderBox 객체에 의해 렌더링 됩니다. 
렌더링 박스는 부모에 의해 constraint 가 주어집니다.
constraint 는 최소 너비 최대 너비 그리고 높이로 구성됩니다.

## Unbounded constraints
특정한 상황에서 박스에 주어지는 constraint 는 제한이 없거나 무한하다.
제한이 없는 constraint 를 가지는 렌더링 박스를 보게 되는 일반적인 경우는 플렉스 박스(Row, Column)와 ListView 및 다른 ScrollView 내부입니다. 

# 상태 관리
## ephemeral state 와 app state 차이
### ephemeral state
UI state or local state 라고도 부른다.
하나의 widget 에서만 사용한다.
### app state
앱의 여러 부분에서 공유하는 state 를 말한다.
example:
* User preferences
* Login info
* Notifications in a social networking app
* The shopping cart in an e-commerce app
* Read/unread state of articles in a news app

## Simple app state management
안좋은예
```
// BAD: DO NOT DO THIS
void myTapHandler() {
  var cartWidget = somehowGetMyCartWidget();
  cartWidget.updateWith(item);
}
```
좋은 예
```
// GOOD
void myTapHandler(BuildContext context) {
  var cartModel = somehowGetMyCartModel(context);
  cartModel.add(item);
}
```
InheritedWidget, InheritedNotifier, InheritedModel
간단하게 사용할 수 있는 provider (event)
* ChangeNotifier
* ChangeNotifierProvider
* Consumer

## ChangeNotifier
변화를 구독할 수 있다. observable 

## ChangeNotifierProvider
ChangeNotifier 를 제공하는 widget 이다
provider 가 여러개라면 MultiProvider 를 사용할 수 있다.
```
ChangeNotifierProvider(create: (context) => BabyModel())
MaterialApp(
      title: 'Momma',
      theme: ThemeData(
        primaryColor: Colors.yellow,
      ),
      home: LoginPage()
    );
```

## Consumer
ChangeNotifier 의 알림을 받는다.







# ref
[building layout - ko](https://flutter-ko.dev/docs/development/ui/layout)
[building layout - en](https://flutter.dev/docs/development/ui/layout/tutorial)