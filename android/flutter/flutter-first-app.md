# 첫 flutter 앱 작성
flutter 로 첫 앱을 만든다.
이 코드랩에서 다음을 배울 수 있다.
* flutter 프로젝트 생성 및 실행
* 외부 패키지 사용
* stateful widget 추가
* listView 만들기
* listView 의 item click event 처리

# 외부 패키지 사용
`pubspec.yml` 파일에 사용하려는 패키지를 추가한다.
패키지는 [pub.dev](https://pub.dev) 에서 찾을 수 있다.
```yaml
dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^1.0.2
  english_words: ^4.0.0   # add this line
```
패키지 추가 후, 다음 명령으로 패키지를 다운받는다.
```shell
flutter packages get
dart pub get	// pubspec.lock 파일 생성
```

# stateful widget 구현
stateful widget 을 구현하려면 state, statefulwidget 클래스가 필요하다.
statefulwidget 은 state 클래스의 인스턴스를 생성하며 객체 자체는 변경 불가능하고 삭제 후, 다시 생성할 수 있다.

먼저 statefulwidget 을 만들고 state 클래스를 생성한다.
ide 에서 `stful` 로 statefulwidget 을 만들면 statefulwidget 이름을 작성하면 알아서 state 클래스가 자동으로 업데이트 된다.

StatefulWidget 은 state 클래스를 생성하는 것 외에는 하는 일이 없다. ???

# listView 만들기
`ListView.builder` 를 사용해서 ListView 를 만들 수 있다.
ListView.builder 의 `itemBuilder` 속성에서 widget 을 return 해서 itemView 를 만든다.
```dart
ListView.builder(
      padding: const EdgeInsets.all(16),
      itemBuilder: (BuildContext _context, int i) {
        if (i.isOdd) {
          return Divider();
        }
        final int index = i ~/ 2;
        if (index >= _suggestions.length) {
          _suggestions.addAll(generateWordPairs().take(10));
        }
	WordPair pair = _suggestions[index];
        return ListTile(
		title: Text(
			pair.asPascalCase,
			style: _biggerFont,
		),
	);
      },
    );
```

# 화면 이동
`Navigator` 를 사용해서 화면 간 이동을 할 수 있다.
`Navigator.push` 를 호출하여 경로를 스택에 푸시하면 화면이 변경되어 새 경로가 표시된다.

# ref
[first-flutter-app-pt1](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt1#6)
[first-flutter-app-pt2](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2)
현