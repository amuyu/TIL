## AnimationDrawable 사용
### 개요
연속되는 Drawable 객체들을 프레임 단위 애니메이션으로 만들고자 할 때, 사용하는 클래스로 사용 방법이 간단하다.
### 정의
#### anim.xml
`<animation-list>` 로 구성된 XML을 정의한다. 
`animation-list`의 각 item은 애니메이션에서 하나의 frame 을 말한다.
```xml
<!-- Animation frames are wheel0.png through wheel5.png
     files inside the res/drawable/ folder -->
 <animation-list android:id="@+id/selected" android:oneshot="false">
    <item android:drawable="@drawable/wheel0" android:duration="50" />
    <item android:drawable="@drawable/wheel1" android:duration="50" />
    <item android:drawable="@drawable/wheel2" android:duration="50" />
    <item android:drawable="@drawable/wheel3" android:duration="50" />
    <item android:drawable="@drawable/wheel4" android:duration="50" />
    <item android:drawable="@drawable/wheel5" android:duration="50" />
 </animation-list>
```

### Animation 시작/정지
#### 시작
ImageView에 위에서 정의한 anim.xml을 셋팅하고 AnimationDrawable을 load하고 Animation을 시작한다.
```java
// Load the ImageView that will host the animation and
 // set its background to our AnimationDrawable XML resource.
 ImageView img = (ImageView)findViewById(R.id.spinning_wheel_image);
 img.setBackgroundResource(R.drawable.spin_animation);
 // Get the background, which has been compiled to an AnimationDrawable object.
 AnimationDrawable frameAnimation = (AnimationDrawable) img.getBackground();
 // Start the animation (looped playback by default).
 frameAnimation.start();
```

#### 정지
위와 같은 방법으로 다시 AnimationDrawable을 load 해서 Animation 정지를 호출한다.
처음에 AnimationDrawable을 가져올 때, 전역으로 저장하고 있다 정지를 호출해도 된다.
```java
// Get the background, which has been compiled to an AnimationDrawable object.
 AnimationDrawable frameAnimation = (AnimationDrawable) img.getBackground();
 // Start the animation (looped playback by default).
 frameAnimation.stop();
```

## 참고
- [Google AnimationDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimationDrawable.html)
- [ImageView에서 AnimationDrawable설정](http://gogorchg.tistory.com/entry/Android-ImageView-%EC%97%90%EC%84%9C-AnimationDrawable-%EC%84%A4%EC%A0%95)