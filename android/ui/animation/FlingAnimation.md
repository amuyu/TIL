# FlingAnimation
제스처 속도로부터 초기 운동량을 갖고 점점 속도가 느려지는 animation 이다.

## Creating a fling animation
FlingAnimation 을 만들기 위해서 애니메이션하는 객체와 객체의 속성을 제공해야 한다.
```java
FlingAnimation fling = new FlingAnimation(view, DynamicAnimation.SCROLL_X);
```

## Setting velocity
애니메이션을 시작할 때, 시작 속도를 정해야 한다.
시작 속도를 고정으로 하거나 touch gesture 의 속도를 기준으로 할 수 있다.
GestureDetector.OnGestureListener 와 VelocityTracker 를 사용해서 속도를 구할 수 있다.

## Setting an animation value range
속성에 대한 값을 특정 범위로 제한하려는 경우 setMinValue 와 setMaxValue 를 사용할 수 있다.

## Setting friction
얼마나 빨리 속도가 줄어들지 정의하는 값을 설정한다. default 는 1이다.

horizontal fling 예제
```java
FlingAnimation fling = new FlingAnimation(view, DynamicAnimation.SCROLL_X);
fling.setStartVelocity(-velocityX)
        .setMinValue(0)
        .setMaxValue(maxScroll)
        .setFriction(1.1f)
        .start();
```
