## Getting Started with the NDK
### 


### Android Studio 사용
Android Studio 1.3 RC3 이상 Gradle 2.5 버전 이상에서 Android NDK를 지원
Android NDK를 빌드하기 위해서는 Gradle 2.5 버전을 사용해야 한다.
#### NDK 설치
Android Studio 1.3 Preview 1.0 이상부터 Android SDK에서 NDK를 다운로드 받을 수 잇다. SDK Manager에서 NDK를 선택하여 설치한다
윈도우의 경우 아래 경로에 설치된다.
```
c:\User\사용자이름\AppData\Local\Android\sdk\ndk-bundle
```
#### Javah 설정
jni에서 사용할 헤더파일을 만들 때 필요한 javah 설정
안드로이드 스튜디오에서 `File - Settings - Tools - External Tools`에서 **javah**를 추가한다
```
※ Tool settings
Program :  jdk 폴더내 bin에 있는 javah.exe  Path
Paramaters :  -classpath $ModuleFileDir$\build\intermediates\classes\debug -v -jni $FileClass$
Working directory :  $ProjectFileDir$\app\src\main\jni
```
#### ndk-build 설정
javah와 마찬가지로 `File - Settings - Tools - External Tools`에서 추가한다
```
※ Tool settings
Program :  %ANDROID_NDK%ndk-build.cmd # ndk 압축 해제 한곳 위치에 ndk-build.cmd 파일
Paramaters :  blank
Working directory :  $ProjectFileDir$\app\src\main\jni
```
#### gradle 설정
App을 Build할 때, NDK를 이용하여 Native Library도 같이 Build할 수 있도록 build.gradle을 수정해야 한다.
```gradle
import org.apache.tools.ant.taskdefs.condition.Os

// Project Structure에서 설정한 NDK 경로를 읽어들여 Return합니다.
def getNdkBuildPath() {
  Properties properties = new Properties()
  properties.load(project.rootProject.file('local.properties').newDataInputStream())

  def command =  properties.getProperty('ndk.dir')
  if (Os.isFamily(Os.FAMILY_WINDOWS)) {
    command += "\\ndk-build.cmd"
  } else {
    command += "/ndk-build"
  }

  return command
}

android {
  sourceSets.main {
    // Compile된 Native Library가 위치하는 경로를 설정합니다. 
    jniLibs.srcDir 'src/main/libs'
    
    // 여기에 JNI Source 경로를 설정하면 Android Studio에서 기본적으로 지원하는 Native
    // Library Build가 이루어집니다. 이 경우에 Android.mk와 Application.mk를
    // 자동으로 생성하기 때문에 편리하지만, 세부 설정이 어렵기 때문에 JNI Source의
    // 경로를 지정하지 않습니다.
    jni.srcDirs = []
  }
  
  ext {
    // 아직은 Task 내에서 Build Type을 구분할 방법이 없기 때문에 이 Property를
    // 이용해 Native Library를 Debugging 가능하도록 Build할 지 결정합니다.
    nativeDebuggable = false
  }

  // NDK의 ndk-build 명령을 이용하여 Native Library를 Build하기 위한 Task를 정의합니다.
  //noinspection GroovyAssignabilityCheck
  task buildNative(type: Exec, description: 'Compile JNI source via NDK') {
    if (nativeDebuggable) {
      commandLine getNdkBuildPath(), 'NDK_DEBUG=1', '-C', file('src/main').absolutePath
    } else {
      commandLine getNdkBuildPath(), '-C', file('src/main').absolutePath
    }
  }
  
  // App의 Java Code를 Compile할 때 buildNative Task를 실행하여 Native Library도 같이
  // Build되도록 설정합니다.
  tasks.withType(JavaCompile) {
    compileTask -> compileTask.dependsOn buildNative
  }  

  // NDK로 생성된 Native Library와 Object를 삭제하기 위한 Task를 정의합니다.
  //noinspection GroovyAssignabilityCheck
  task cleanNative(type: Exec, description: 'Clean native objs and lib') {
    commandLine getNdkBuildPath(), '-C', file('src/main').absolutePath, 'clean'
  }

  // Gradle의 clean Task를 실행할 떄, cleanNative Task를 실행하도록 설정합니다.
  clean.dependsOn 'cleanNative'
  
  buildTypes {
    debug {
      // Debug Build시에 Native Library Debugging이 가능한 APK를 생성하도록 설정합니다.
      jniDebuggable true
    }
  }
}
```

#### 설정 이후, 코드 작성해서 빌드 하면 된다


### 참고
- [Getting Started with the NDK](https://developer.android.com/ndk/guides/index.html)
- [Android studio NDK 설정](http://dwfox.tistory.com/22)
- [Android NDK 다운로드 및 설치하기](http://thdev.net/626)
- [안드로이드 스튜디오(NDK)에서 JNI 사용하기](http://yucaroll.tistory.com/1)
- [External tool javah 스크립트 만들기](http://hatti.tistory.com/entry/NDK-External-tool-javah-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [Android Studio Project에 NDK 적용하기(Part1)](https://www.davidlab.net/ko/tech/using-the-android-ndk-with-android-studio-part1/)