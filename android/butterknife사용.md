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


## 참고
[butterknife github](https://github.com/JakeWharton/butterknife)