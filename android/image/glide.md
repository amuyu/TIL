# Glide
## gradle
```gradle
repositories {
  mavenCentral() // jcenter() works as well because it pulls from Maven Central
}

dependencies {
  compile 'com.github.bumptech.glide:glide:3.7.0'
  compile 'com.android.support:support-v4:19.1.0'
}
```
## Proguard
```proguard
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}
-keepresourcexmlelements manifest/application/meta-data@value=GlideModule
```
## GlideModule
GlideModule 을 사용할 때
```
<manifest ...>
    <!-- ... permissions -->
    <application ...>
        <meta-data
            android:name="com.mypackage.MyGlideModule"
            android:value="GlideModule" />
        <!-- ... activities and other components -->
    </application>
</manifest>
```
## 참고
[Glide](https://github.com/bumptech/glide)
