# 코틀린 프로젝트 생성
## 새 프로젝트 생성
1. 안드로이드 스튜디오 실행해서 'Start a new Android Studio project' 를 선택한다.  
![new project](https://github.com/amuyu/TIL/blob/master/blog/img/kotlin/1.png?raw=true)
2. New Project 화면에서 앱 기본 정보를 입력한다.
![configure](https://github.com/amuyu/TIL/blob/master/blog/img/kotlin/2.png?raw=true)
3. Target을 설정한다. (특별히 정해진 게 없으면 default 값으로 선택)
4. 'Empty Activity' 로 선택 후, default Activity를 생성한다.

## 생성된 Java 코드 코틀린 파일로 변환
1. Help - Find Action... 을 선택한다.
2. 'convert java file to kotlin' 을 입력 후, action 을 선택한다.
3. kotlin 으로 변환된 코드를 확인할 수 있다.

## 코틀린 사용을 위한 셋팅
코틀린 파일 변환 후, 'Sync Project with Gradle Files' 를 선택하면
(Help - Find Action... 에서 Sync Project 로 검색하거나 아이콘을 선택한다.)  

MainActivity.Kt 파일 상단에 'Kotlin not configured'라는 메시지가 나타난다. 메시지 오른쪽에 'configure'를 선택한다.
![configure](https://github.com/amuyu/TIL/blob/master/blog/img/kotlin/3.png?raw=true)

그러면 Configure Kotlin In Project 라는 설정창이 오픈된다. 기본 셋팅 그대로 놔두고 OK를 선택한다.  
![configure](https://github.com/amuyu/TIL/blob/master/blog/img/kotlin/4.png?raw=true)  
그러면 자동으로 gradle 파일 설정을 변경하는데 내용은 다음과 같다. 수동으로 입력해도 상관없다.

#### app/build.gradle
```groovy
apply plugin: 'kotlin-android'
...
dependencies {
  compile "org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
}
```
#### project/build.gradle
```groovy
buildscript {
    ext.kotlin_version = '1.1.2-3'
    ...
    dependencies {
        ...
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
```

# 간단한 코틀린 코드 작성 (Kotilin Android Extensions)
이제 프로젝트에서 코틀린 파일을 사용할 준비가 완료되었다.
그럼 activity_main.xml 의 textView 의 text를 변환하는 간단한 코드를 작성해보자. kotlin 에서는 findViewId 사용없이 view 에 접근할 수 있는 기능 `Kotilin Android Extensions`  을 제공하는데 이를 사용해보자.  

## textview 접근
textView 에 id를 추가한다.  
acitivy_main.xml
```xml
<TextView
        android:id="@+id/textView"
        ...
        />
```
gradle 파일에 다음 plugin 을 추가한다.
#### app/build.gradle
```groovy
apply plugin: 'kotlin-android-extensions'
```

gradle 파일 sync 후, MainActivity.kt 파일에서 onCreate 메소드 내부에서 textView 라고 입력하면 'textView for Activity in kotlinx.android.syntax' 자동완성이 뜨는데 이를 선택한다.  
![auto](https://github.com/amuyu/TIL/blob/master/blog/img/kotlin/5.png?raw=true)
그러면 import 가 자동으로 추가된다.  
```java
import kotlinx.android.synthetic.main.activity_main.*;  // ; 생략
```
그러면 findviewid이 없이 바로 textView에 접근을 할 수 있게 되었다. 텍스트를 변경해보자.
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        textView.setText("Start Kotlin")
}
```
다음과 같이 입력 후, 앱을 실행해보면 'Hello world!'가 'Start Kotlin'으로 변경된 것을 확인할 수 있다.  
![result](https://github.com/amuyu/TIL/blob/master/blog/img/kotlin/6.png?raw=true)
