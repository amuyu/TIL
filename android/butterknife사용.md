#butterknife
Field and method binding for Android views which uses annotation processing to generate boilerplate code for you
## 설정
### build.gradle (project)
Configure your project-level build.gradle to include the `android-apt` plugin
```gradle
buildscript {
  repositories {
    mavenCentral()
   }
  dependencies {
    classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
  }
}
```
### build.gradle (module)
Apply the `android-apt` plugin in your module-level build.gradle and add the Butter Knife dependencies
```gradle
apply plugin: 'android-apt'

android {
  ...
}

dependencies {
  compile 'com.jakewharton:butterknife:8.4.0'
  apt 'com.jakewharton:butterknife-compiler:8.4.0'
}
```
## com.android.support:support-annotations:24.1.0 과 Annotation 충돌이 날경우
```java
androidTestCompile ('com.android.support:support-annotations:24.1.0') {
        exclude group: 'com.android.support'
    }
    하거나
configurations.all {
resolutionStrategy {
    force 'com.android.support:support-annotations:23.1.1'
}
}
```
### 참고
[Warning:Conflict with dependency 'com.android.support:support-annotations'](http://stackoverflow.com/a/36897759/6811452)

## bindView
```java
@BindView(R.id.fl_auto) LinearLayout fl_auto;
```
## Acitivity, Fragment, RecyclerView Bind
화면 별 Bind 방법
```java
// Activity
ButterKnife.bind(this);

// Fragment
ButterKnife.bind(this,rootView);

// RecyclerView.holder
ButterKnife.bind(viewholder, rootView);
```
##  클릭이벤트
```java
@OnClick(R.id.test);
public void onClick(View v) {

}
```
## prougard
```gradle
# Retain generated class which implement Unbinder.
-keep public class * implements butterknife.Unbinder { public <init>(...); }

# Prevent obfuscation of types which use ButterKnife annotations since the simple name
# is used to reflectively look up the generated ViewBinding.
-keep class butterknife.*
-keepclasseswithmembernames class * { @butterknife.* <methods>; }
-keepclasseswithmembernames class * { @butterknife.* <fields>; }
```

## Tip
### ViewHolder 사용 셋팅
1개의 fragment에서 2개의 view를 사용
[Answer](http://stackoverflow.com/a/24922397/6811452)
### List or array view 셋팅
view list section 에 bind Setting
```java
@InjectViews({ R.id.first_name, R.id.middle_name, R.id.last_name })
List<EditText> nameViews;
```
[Answer](http://stackoverflow.com/a/24270313/6811452)
### ViewHolder 사용 시, onclick 이벤트 binding
viewholder 클래스 내부에 click 이벤트를 정의하고 넘겨받는 방식으로 구현
```java
public class ViewHolder {
		@BindView(R.id.tv_item1) TextView mtv_item1;
		@BindView(R.id.tv_item2) TextView mtv_item2;
		@BindView(R.id.tv_item3) TextView mtv_item3;

		private OnClickListener mClickListener;

		public void setClickListener(OnClickListener mClickListener) {
			this.mClickListener = mClickListener;
		}

		@OnClick({R.id.tv_item1, R.id.tv_item2, R.id.tv_item3})
		public void onClick(View v) {
			mClickListener.onClick(v);
		}
	}
```

## 참고
[butterknife github](https://github.com/JakeWharton/butterknife)
