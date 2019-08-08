
# Lint로 코드 개선
Lint 를 사용해서 앱을 실행하거나 테스트 사례를 작성하지 않고도 코드의 구조적 문제를 식별하고 수정할 수 있습니다.
각각의 문제는 설명 메시지 및 심각도 수준과 함께 보고 되므로 개선이 시급한 순서대로 신속히 우선순위를 결정할 수 있습니다.
Android Studio 를 사용할 경우 앱을 빌드할 때마다 구성된 Lint와 IDE 검사를 실행합니다.

#### lint.xml 파일
배제하고 싶은 Lint 검사를 지정하고 문제 심각도 수준을 사용자 지정하는데 사용하는 구성 파일

#### Lint 도구
명령줄 또는 Android Studio를 사용하여 실행할 수 있는 정적 코드 스캔 도구

#### Lint 검사 결과
Android Studio 콘솔 또는 Inspection Results 창에서 결과를 확인할 수 있습니다.

## Gradle 로 Lint 실행
```sh
gradlew lint
```

## Lint 파일 구성
`lint.xml` 파일에서 Lint 검사 기본 설정을 지정할 수 있습니다.
Android 프로젝트의 루트 디렉토리에 파일을 넣습니다.
`lint.xml` 파일은 하나 이상의 하위 `<issue>` 요소가 포함된 인클로징 `<lint>` 상위 태그로 구성됩니다.
Lint 는 각 `<issue>` 에 고유한 `id` 특성 값을 정의합니다.

sample
```xml
<?xml version="1.0" encoding="UTF-8"?>
<lint>
    <!-- Disable the given check in this project -->
    <issue id="IconMissingDensityFolder" severity="ignore" />

    <!-- Ignore the ObsoleteLayoutParam issue in the specified files -->
    <issue id="ObsoleteLayoutParam">
        <ignore path="res/layout/activation.xml" />
        <ignore path="res/layout-xlarge/activation.xml" />
    </issue>

    <!-- Ignore the UselessLeaf issue in the specified file -->
    <issue id="UselessLeaf">
        <ignore path="res/layout/main.xml" />
    </issue>

    <!-- Change the severity of hardcoded strings to "error" -->
    <issue id="HardcodedText" severity="error" />
</lint>
```

## Lint 비활성화
자바 클래스 또는 메서드에서 Lint 검사를 비활성화하려면 `SuppressLint` 주석을 추가합니다.
```java
@SuppressLint("NewApi")
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.main);
```

xml 에서 비활성화하려면 `tools:ignore` 특성을 사용합니다.
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    tools:ignore="UnusedResources" >

    <TextView
        android:text="@string/auto_update_prompt" />
</LinearLayout>
```

## Gradle 로 Lint 옵션 구성
Gradle 용 Android 플러그인을 사용하면 모듈 수준의 `build.gradle` 파일에서 lintOptions {} 블록을 사용하여
검사 실행 또는 무시 등과 같은 특정 Lint 옵션을 구성할 수 있습니다.
```
android {
  ...
  lintOptions {
    // Turns off checks for the issue IDs you specify.
    disable 'TypographyFractions','TypographyQuotes'
    // Turns on checks for the issue IDs you specify. These checks are in
    // addition to the default lint checks.
    enable 'RtlHardcoded','RtlCompat', 'RtlEnabled'
    // To enable checks for only a subset of issue IDs and ignore all others,
    // list the issue IDs with the 'check' property instead. This property overrides
    // any issue IDs you enable or disable using the properties above.
    check 'NewApi', 'InlinedApi'
    // If set to true, turns off analysis progress reporting by lint.
    quiet true
    // if set to true (default), stops the build if errors are found.
    abortOnError false
    // if true, only report errors.
    ignoreWarnings true
  }
}
...
```

## inspection profile
Android Studio 에는 Android 업데이트를 토앻 업데이트 된 Lint 및 다른 검사 파일들이 있습니다.
이 프로필을 그대로 사용가거나 편집 및 활성화/비활성화할 수도 있습니다.

inspections 대화상자 액세스:
1. Analyze > Inspect Code 
2. Inspection Profile 의 Specify Scope 에서 More


# 궁금한점
안드로이드의 경우, lint tool 을 제공해주는데, 일반 자바도 그런 툴이 있을까?
구글링 했을 때, 주로 안드로이드 관련 글들이 위주 자바는 못 찾겠음. 


# ref
[Lint로 코드 개선](https://developer.android.com/studio/write/lint?hl=ko)
[googlesamples](https://github.com/googlesamples/android-custom-lint-rules)
[Writing your first lint check](https://medium.com/@vanniktech/writing-your-first-lint-check-39ad0e90b9e6)