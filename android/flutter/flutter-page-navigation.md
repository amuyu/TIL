# 새로운 화면으로 이동하고, 되돌아오기  
Flutter 에서 어떻게 화면을 이동하는지 정리한다.  
`Navigator` 를 사용하며 다음 단계로 진행한다.

1. 두 개의 route를 생성합니다.
2. Navigator.push()를 사용하여 두 번째 route로 전환합니다.
3. Navigator.pop()을 사용하여 첫 번째 route로 되돌아 옵니다.  

> Route : Flutter에서 screen 과 page 는 route 로 불린다. Route는 Android의 Activity, iOS의 ViewController와 동일하다.  
  
## 1. 두 개의 route 를 생성
두 개의 route 를 생성한다. 각 route 는 버튼 하나만 있으며, 첫 번째 route 버튼을 누르면 두 번째 route 로 이동하고  
두 번째 route 버튼을 누르면 첫 번째 route 로 이동한다.  

```dart
class FirstRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('First Route'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Open route'),
          onPressed: () {
            // 눌렀을 때 두 번째 route로 이동합니다.
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Route"),
      ),
      body: Center(
        child: RaisedButton(
          onPressed: () {
            // 눌렀을 때 첫 번째 route로 되돌아 갑니다.
          },
          child: Text('Go back!'),
        ),
      ),
    );
  }
}
```

## 2. Navigator.push()를 사용하여 두 번째 route로 전환합니다.  
새로운 route 로 전환하기 위해 `Navigator.push()` 를 사용한다.
push 메서드는 Route 를 Navigator 에 의해 관리되는 route 스택에 추가한다.  

```dart
// Within the `FirstRoute` widget
onPressed: () {
  Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => SecondRoute()),
  );
}
```

### Navigator.pop()을 사용하여 첫 번째 route로 되돌아 옵니다.  
이 전 route 로 전환하기 위해 `Navigator.pop()` 을 사용한다.
pop 메서드는 Route 를 Navigator 에 의해 관리되는 route 스택에서 제거한다
```dart
// Within the SecondRoute widget
onPressed: () {
  Navigator.pop(context);
}
```
