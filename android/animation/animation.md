# Animation and Graphics Overview
Android 는 2D/3D graphics를 drawing 하고 aniation 을 적용하기 위한 다양한 API 를 제공한다.

## Overview
### Animation
Android framework 에서는 두 가지 animation system 을 제공한다. :property animation과 view animation
두 가지 시스템 모두 쓸만하지만 property animation 을 더 많이 사용한다. 좀 더 유용하고 좀 더 기능이 많기 때문에
두 가지 시스템 외에도 Drawable animation 을 사용할 수 있다.
drawable resource 를 load 하고 frame에 display 한다.

Property Andimation 은 객체의 모든 속성을 애니메이션으로 만들 수 있다.
View Animation 은 사용하기 쉽고 어플리케이션에서 사용하기 충분한 기능을 제공한다.
Drawable Animation 은 drawable resources 에 대해 표현하기 쉽다.

### 2D and 3D graphics
앱을 만들 때, graphcial demands에 대해 정확히 파악하는 것이 중요하다. 그래픽 작업들은 다양한 기술들로 만들어진다.
정적인 어플과 동적인 게임에 대한 graphics와 animation 들은 많이 다르게 구현된다.
적절한 선택을 하기 위해 몇 가지 options들을 살펴보자.

#### Canvas and Drawables
Android 는 View widgets 들을 제공한다. 이 widgets 들은 모양이나 동작들을 수정할 수 있다.
Canvas 클래스와 Drawable 객체를 사용해서 2D rendering 을 할 수 있다.

#### Hardware Acceleration
performance를 높이기 위해서 hardware accelerate 를 사용할 수 있다.

#### OpenGL
고성능을 요구하는 그래픽 작업을 하는 ()게임 같은 어플리케이션에서는) NDK 를 사용하는 것이 좋다.

## Property Animation
property animation 를 사용해서 거의 모든 것을 animate 할 수 있습니다.
property animation 은 속성에 대한 값을 특정한 시간 동안 변화시킵니다.
어떤 것을 animate 하기 위해서는 속성을 선택하고 화면에서 위치를 정하고 얼마나 animate 할지를 정해야 합니다.

Property animation 시스템에서 정의할 수 있는 내용
- Duration : default length is 300ms
- Time interpolation : anitiaon의 경과 시간에 따라 값이 변하도록 정할 수 있습니다.
- Repeat count and behavior
- Animator sets
- Frame refresh delay : The default is set to refresh every 10ms

## How Property Animation Works
ValueAnimator 는 TimeInterpolator 와 TypeEvaluator 를 사용해 animation 하는 동안
속성에 대한 값의 변화를 추적한다.

animation 을 시작하기 위해서 ValueAnimator를 만들고 animate 하려는 속성에 대한 시작과 끝 값을 준다.

animation 을 시작하면 animation 동안에 ValueAnimator는 elapsed fraction 을 계산한다.
ValueAnimator가 elapsed fraction 을 계산하는 동안 TimeInterpolator는 interpolated fraction 을
계산한다. interpolated fraction 을 계산하면 ValueAnimator는 TypeEvaluator를 호출해서 animating 하는
속성에 대한 값을 계산한다.

## How Property Animation Differs from View Animation
View animation system 응 view 객체만 animate 할 수 있다.
view animation 은 view 의 scaling, rotation 과 같은 몇 가지만 할 수 있다.
그리고 view animation 은 view 그려지는 위치만 수정할 수 있다. (실제 뷰의 위치가 바뀌지는 않는다.)
하지만 view animation 은 설정하기 쉽고 코드도 간결하다. 간단한 Animation 을 적용할 때 View Animation 을 사용할 수 있다.

## API Overview
Animator class 는 animation 을 만들기 위한 기본적인 구조를 제공한다.
이 클래스는 최소한의 기능을 제공하기 때문에 animating values 를 완전히 지원하기 위해서는 확장해야 한다.
다음은 Animator를 상속받는 subclass 들이다.

ValueAnimator - 애니메이션 하는 속성에 대한 값을 계산하는 주요 타이밍 엔진
ObjectAnimator - 계산 + Animation, VlueAnimator의 subclass 로 ValueAnimator보다 사용하기 더 쉽다.
AnimatorSet - 동시에 하거나 순차적으로 하거나 딜레이 후에 애니메이션을 할 수 있다.

Evaluators 는 속성에 대해 값을 어떻게 계산할지 알려주는 클래스들이다.

IntEvaluator
FloatEvaluator
ArgbEvaluator
TypeEvaluator

TimeInterpolator 는 특정한 값을 시간의 함수로 어떻게 계산하는 방법을 정의한다.

AccelerateDecelerateInterpolator
AccelerateInterpolator
AnticipateInterpolator
AnticipateOvershootInterpolator
BouceInterpolator
CycleInterpolator
DecelerateInterpolator
LinearInterpolator
OvershootInterpolator
TimeInterpolator

## Animating with ValueAnimator
ValueAnimator는 animate 할 값에 대한 type 과 duration 등에 정할수 있다.
```java
ValueAnimator animation = ValueAnimator.ofFloat(0f, 100f);
animation.setDuration(1000);
animation.start();
```
animate 할 custom type  도 정할 수 있다.
```java
ValueAnimator animation = ValueAnimator.ofObject(new MyTypeEvaluator(), startPropertyValue, endPropertyValue);
animation.setDuration(1000);
animation.start();
```
AnimatorUpdateListener를 추가해서 animation 에 대한 값을 사용할 수 있다.
```java
animation.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
    @Override
    public void onAnimationUpdate(ValueAnimator updatedAnimation) {
        // You can use the animated value in a property that uses the
        // same type as the animation. In this case, you can use the
        // float value in the translationX property.
        float animatedValue = (float)updatedAnimation.getAnimatedValue();
        textView.setTranslationX(animatedValue);
    }
});
```


## Animating with ObjectAnimator
ObjectAnimator는 ValueAnimator의 subclass 이며 ValueAnimator의
타이밍 엔진과 값 게산을 대상 객체의 명명된 속성에 애니메이션을 적용하는 기능과 결합한다.

ObjectAnimator는 객체를 지정하고 속석 이름을 함게 사용할 수 있다.
```java
ObjectAnimator animation = ObjectAnimator.ofFloat(textView, "translationX", 100f);
animation.setDuration(1000);
animation.start();
```

ObjectAnimator를 사용하기 위해서는 다음을 해야한다.
- object property 는 setter function 을 가져야 한다.
- anitaion 시작 값을 얻기 위해서 getter function 을 가져야 한다.
- getter, setter methods의 type은 같아야 한다.
- animate 하는 객체나 속성을 화면에 redraw하기 위해 invalidate 메소드를 콜하려면 onAnimationUpdate() callback 에서 호출한다.


## Choreographing Multiple Animations with AnimatorSet
Animations 들을 사용할 수 있도록 AnimatorSet 을 제공한다.
```java
AnimatorSet bouncer = new AnimatorSet();
bouncer.play(bounceAnim).before(squashAnim1);
bouncer.play(squashAnim1).with(squashAnim2);
bouncer.play(squashAnim1).with(stretchAnim1);
bouncer.play(squashAnim1).with(stretchAnim2);
bouncer.play(bounceBackAnim).after(stretchAnim2);
ValueAnimator fadeAnim = ObjectAnimator.ofFloat(newBall, "alpha", 1f, 0f);
fadeAnim.setDuration(250);
AnimatorSet animatorSet = new AnimatorSet();
animatorSet.play(bouncer).before(fadeAnim);
animatorSet.start();
```


## Animation Listeners
중요한 이벤트들에 listen 할 수 있다.

- ValueAnimator.AnimatorUpdateListener
  - onAnimationUpdate - animation frame 마다 불린다.

```java
ValueAnimator fadeAnim = ObjectAnimator.ofFloat(newBall, "alpha", 1f, 0f);
fadeAnim.setDuration(250);
fadeAnim.addListener(new AnimatorListenerAdapter() {
public void onAnimationEnd(Animator animation) {
    balls.remove(((ObjectAnimator)animation).getTarget());
}});
```


## Animation Layout Changes to ViewGroups
LayoutTransition class 로 ViewGroup 내에서 layout 변화를 animate 할 수 있다.

다음 상수들과 함께 animator를 set 할 수 있다.
- APPEARING
- CHANGE_APPEARING
- DISAPPEARING
- CHANGE_DISAPPEARING

`animateLayoutChanges` 속성을 사용해서 default animate를 줄 수 있다.

```xml
<LinearLayout
    android:orientation="vertical"
    android:layout_width="wrap_content"
    android:layout_height="match_parent"
    android:id="@+id/verticalContainer"
    android:animateLayoutChanges="true" />
```


## Animating Views
property animations 에서 변화시키는 속성들
- translationX, translationY
- rotation, rotationX, rotationY
- scaleX, scaleY
- pivotX, pivotY
- x and y
- alpha

```java
ObjectAnimator.ofFloat(myView, "rotation", 0f, 360f);
```


## Animating with ViewPropertyAnimator
ViewPropertyAnimator 는 몇몇 속성들을 animate 하는 간단한 방법이다.
Multiple ObjectAnimator objects
```java
ObjectAnimator animX = ObjectAnimator.ofFloat(myView, "x", 50f);
ObjectAnimator animY = ObjectAnimator.ofFloat(myView, "y", 100f);
AnimatorSet animSetXY = new AnimatorSet();
animSetXY.playTogether(animX, animY);
animSetXY.start();
```
One ObjectAnimator
```java
PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat("x", 50f);
PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat("y", 100f);
ObjectAnimator.ofPropertyValuesHolder(myView, pvhX, pvyY).start();
```
ViewPropertyAnimator
```java
myView.animate().x(50f).y(100f);
```


## Declaring Animations in xml
property animation 은 xml에서 선언할 수 있다. `res/animator/` directory 에 저장해야 한다.
```xml
<set android:ordering="sequentially">
    <set>
        <objectAnimator
            android:propertyName="x"
            android:duration="500"
            android:valueTo="400"
            android:valueType="intType"/>
        <objectAnimator
            android:propertyName="y"
            android:duration="500"
            android:valueTo="300"
            android:valueType="intType"/>
    </set>
    <objectAnimator
        android:propertyName="alpha"
        android:duration="500"
        android:valueTo="1f"/>
</set>
```
XML resources에서 AnimatorSet 객체를 inflate 하기 위해서는 다음과 같이 호출 한다.
```java
AnimatorSet set = (AnimatorSet) AnimatorInflater.loadAnimator(myContext,
    R.anim.property_animator);
set.setTarget(myObject);
set.start();
```



# 참고
[Animation and Graphics Overview](https://developer.android.com/guide/topics/graphics/overview.html)
