# 설정
gradle 설정
```gradle
dependencies {
    compile "com.splitwise:tokenautocomplete:2.0.8@aar"
}
```
# 구현
## Subclass TokenCompleteTextView
TokenCompleteTextView 를 상속받는 view 클래스를 생성 (부모 클래스는 MultiAutoCompleteTextView)
### getViewForObject
가지고 있는 리스트 중에서 선택했을 때, 보일 view를 셋팅
### defaultObject
임의의 텍스트가 완성되었을 때, 객체를 생성해서 view를 셋팅하도록 설정
## 객체
Serializable 인터페이스를 사용한 객체를 생성
## Responding to user selections
TokenListener 를 통해서 추가/제거에 대한 이벤트 수신
```java
public static interface TokenListener {
    public void onTokenAdded(Object token);
    public void onTokenRemoved(Object token);
}
```
## Programatically add and remove objects
addObject, removeObject 를 사용해서 추가 제거 할 수 있음
## Letting users click to select and delete Tokens
token 클릭 이벤트에 대한 스타일을 정할 수 있다.
### None
### delete
### Select
#### Custom view
token 에 선택했을 때 view 를 다음과 같이 설정 가능
```java
public class TokenTextView extends TextView {
    ...

    @Override
    public void setSelected(boolean selected) {
        super.setSelected(selected);
        setCompoundDrawablesWithIntrinsicBounds(0, 0, selected ? R.drawable.close_x : 0, 0);
    }
}
```
## token 가져오기
view.getObjects() 호출해서 가져온다.
