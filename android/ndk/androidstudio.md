
# Android studio로 Android ndk 코드 작성하기
## 준비
- NDK Build는 Gradle 2.5 이상에서 동작
- NDK r10e 필요
- SDK Build Tools 는 19.0.0 이상 필요
- Studio는 1.3 Preview 이상
- Java 버전은 1.7

## build.gradle
gradle-experimental 수정
```groovy
//classpath 'com.android.tools.build:gradle:2.2.0'
classpath "com.android.tools.build:gradle-experimental:0.8.0"
```

## app/build.gradle
```groovy
apply plugin: 'com.android.model.application'
// model 추가
model {
  android {

  }
}
```

## ndk build task
uvccamera 소스 참고
```groovy
tasks.withType(JavaCompile) {
	compileTask -> compileTask.dependsOn ndkBuild
}

String getNdkBuildPath() {
	Properties properties = new Properties()
	properties.load(project.rootProject.file('local.properties').newDataInputStream())
	def ndkBuildingDir = properties.getProperty("ndk.dir")
	def ndkBuildPath = ndkBuildingDir
	if (Os.isFamily(Os.FAMILY_WINDOWS)) {
		ndkBuildPath = ndkBuildingDir + '/ndk-build.cmd'
	} else {
		ndkBuildPath = ndkBuildingDir + '/ndk-build'
	}
	return ndkBuildPath
}

task ndkBuild(type: Exec, description: 'Compile JNI source via NDK') {
	println('executing ndkBuild')
	def ndkBuildPath = getNdkBuildPath();
	commandLine ndkBuildPath, '-j8', '-C', file('src/main').absolutePath
}

task ndkClean(type: Exec, description: 'clean JNI libraries') {
	println('executing ndkBuild clean')
	def ndkBuildPath = getNdkBuildPath();
	commandLine ndkBuildPath, 'clean', '-C', file('src/main').absolutePath
}

clean.dependsOn 'ndkClean'

```

# 참고
[Experimental Plugin User Guide](http://tools.android.com/tech-docs/new-build-system/gradle-experimental#TOC-Latest-Version)
[Android studio로 ndk 코드 작성하기](http://thdev.net/635)
[android-ndk sample](https://github.com/googlesamples/android-ndk)
[C 및 C++ 코드를 프로젝트에 추가](https://developer.android.com/studio/projects/add-native-code.html?hl=ko)
