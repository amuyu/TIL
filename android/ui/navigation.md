# 쓸만한 것
## navitate() + pupUpTo
특정 위치까지 되돌아간 후, 다음 화면으로 이동한다.
```xml
<action
    android:id="@+id/action_to_login"

    app:pupUpTo="@id/splash"
    app:pupUptoInclusive="true"/>
```

## naviate() + signleTop
같은 화면을 연속으로 쌓지 않고 화면을 교체한다.
```xml
<action
    android:id="@+id/action_to_login"

    app:launchSingleTop="true"/>
```

## backward navigation
```kotlin
val controller = findNavController()
controller.navigateUp()                         // Up 버튼 동작
controller.popBackStack()                       // back 버튼 동작
controller.popBackStack(R.id.home, false)       // 특정 목적지까지 back 동작
```

# 궁금한 것
## shared elements?
Acitivity, Fragment 간에 SharedElements 기능도 지원한다.
shared elements 는 view를 전달하는 효과를 갖는다. (animation), `transitions` 를 지원한다.

### transitions(전환)
list -> detail 화면으로 바뀌는 것 같은 일종의 화면 전환을 의미한다. 
애니메이션(모션)을 사용하면 사용자가 두 화면의 관계를 이해하기 쉽다
naviation 은 이러한 표현을 돕는다.

```kotlin
// 전달하는 쪽
val extras = FragmentNavigatorExtras(
    imageView to "imageView"
)
findNavController().navigate(R.id.detailAction, null, null, extras)
// 전달받는 쪽?
sharedElementEnterTransition = TransitionInflater.from(context).inflateTransition(android.R.transition.move)
```

xml 에 `transitionName` 속성을 전달하는 쪽, 받는 쪽에 동일한 이름으로 셋팅한다.
```xml
<ImageView
    android:src="@color/colorAccent"
    android:id="@+id/imageView"
    android:transitionName="imageView"
    android:layout_width="200dp"
    android:layout_height="200dp"/>
```


# ref
[navigation-x](https://speakerdeck.com/fornewid/navigation-x)
[google ref](https://developer.android.com/guide/navigation/navigation-getting-started)
[Transitions in Android Navigation Architecture Component](https://medium.com/@serbelga/shared-elements-in-android-navigation-architecture-component-bc5e7922ecdf)
[navigation advanced sample](https://github.com/googlesamples/android-architecture-components/tree/master/NavigationAdvancedSample)