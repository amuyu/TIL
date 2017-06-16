
# layout
```xml
<layout>
</layout>
<data></data>
```
# setup
## gradle.build
dataBinding 요소 추가
```gradle
android {
    ....
    dataBinding {
        enabled = true
    }
}
```
# Activity Binding
```java
MainActivityBinding binding = DataBindingUtil.setContentView(this, R.layout.main_activity);
MainActivityBinding binding = MainActivityBinding.inflate(getLayoutInflater());
```
# Fragment Binding
```java
RequestMainBinding.inflate(inflater, container, false);
```
# Adapter Binding 은 연결
