
## flutter 설치하기
[Flutter 설치](https://flutter.dev/docs/get-started/install/macos)

### Get the Flutter SDK
1. Flutter SDK 를 다운받는다. (ex: flutter_macos_2.0.5-stable.zip)
2. 압축을 푼다.
```shell
cd ~/development
unzip ~/Downloads/flutter_macos_2.0.5-stable.zip
```
3. path 를 설정한다.
```shell
export PATH="$PATH:`pwd`/flutter/bin"
```

### Run flutter doctor
다음 명령을 실행하면 설정을 완료하기 위해 필요한 것들을 확인할 수 있다.
```shell
flutter doctor
```
xcode, android studio, chrome 등등



-----


## IOS Setup

### Install Xcode

ios flutter app을 개발하기 위해서 xcode 가 필요하다.

1. 최신 버전의 xcode 를 설치한다. 
2. 설치한 버전의 xcode 를 사용하도록 설정한다.

```shell
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -runFirstLaunch
```

3. xcode license 에 동의한다.

### Set up the iOS simulator

flutter app 을 테스트할 iOS simulator 를 준비한다.

1. simulator 를 실행한다.

```shell
open -a Simulator
```

2. `Hardware > Device` 메뉴에서 64-bit 장치 (iPhone 5s or later) 를 사용하는지 확인한다.

### Create and run a simple Flutter app

flutter app 을 만든 후, Simulator 로 실행한다.

1. flutter app 을 만든다.

```shell
flutter create my_app
```

2. `my_app` 으로 이동한다.

```shell
cd my_app
```

3. Simulator 로 실행한다.

```shell
flutter run
```

----

## Android setup

### Install Android Studio

1. [Android Studio](https://developer.android.com/studio) 를 다운로드하고 설치한다.
2. Android Studio 를 실행해서 최신 Android SDK, Android Command-line Tools, Android SDK Build-Tools 를 설치한다.

### Set up your Android device

flutter app 을 실행하기 위해서 Android 4.1 (API level 16) 이상의 device 가 필요하다.

1. device 에서 usb debugging 모드를 설정한다.
2. usb cable 을 연결한다.
3. 터미널에서 `flutter devices` 명령을 실행하면 연결된 android device 를 확인한다.

### Set up the Android emulator

1. 컴퓨터에서 `VM acceleration` 을 활성화 한다.
2. Android Sudio 를 실행하고 `AVD Manager` 를 클릭하고 `Create Virtual Device...` 를 선택한다.
3. Android version 과 system image 를 선택한다. x86 이나 x86_64 image 를 추천한다.
4. hardware 가속을 활성화 하려면 Hardware - GLES 2.0 을 선택한다.
5. Device 를 생성을 완료하면, AVD Manager 에서 Virtual Device 를 실행할 수 있다.

---

## Codelabs - first-flutter-app-pt1

### StatefulWidget

Stateful widgets 은 lifetime 동안 변경될 수 있는 state 를 관리한다.

#### State

State 는 widget 에서 읽을 수 있는 정보이고 이 정보는 변화한다.

> ```
> State is information that (1) can be read synchronously when the widget is
> /// built and (2) might change during the lifetime of the widget.
> ```





---

## flutter dependency 다운
```
flutter pub get
```
https://flutter-ko.dev/docs/get-started/codelab#2%EB%8B%A8%EA%B3%84-%EC%99%B8%EB%B6%80-%ED%8C%A8%ED%82%A4%EC%A7%80-%EC%9D%B4%EC%9A%A9%ED%95%98%EA%B8%B0

## flavor
- main_dev.dart : development flavor
- main_qa.dart : qa team
- main_produection.dart : final product

https://binary-studio.com/2020/04/17/flutter-2/




# ref

[samples](https://flutter.github.io/samples/#)