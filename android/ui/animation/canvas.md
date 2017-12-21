# Canvas and Drawables
2D graphics 를 그리는 방법은 다음과 같다.
- View 에 그리기
- Canvas 에 그리기

dynamic 한 변화가 없거나 간단한 graphics를 그릴 때는 view 에 그리는게 좋다.
video games 처럼 계속 다시 그려야 한다면 canvas에 그리는게 좋다.


## Draw with a canvas
canavs 클래스를 사용해서 특별한 그리기나 그래픽 애니메이션의 제어를 사용하는 앱의 요구를 충족할 수 있다.
canvas 는 그래픽이 그려지는 실제 표면에 인터페이스로 사용되고 그리기 작업을 수행할 수 있다.
캔버스는 창에 배치되는 기본 bitmap 객체로 그린다.


## Drawing on a view
많은 양을 처리하거나 high frame rate 를 필요하지 않는 다면 View.onDraw callback 으로 그릴 수 있다.

view에 그리기 위해서 먼저, View의 subclass를 만들고 onDraw(canvas) callback 을 구현한다
미리 정의된 canvas 객체를 사용해서 draw 하면된다.

Android Framework 에서는 onDraw를 필요할 때만 호출하기 때문에 다시 그리기 위해서는
invalidate()를 호출해야 한다.


## Drawing on a SurfaceView
SurfaceView 는 surface를 worker thread 에 제공하는 것이 목적이다. 이 방법으로 앱은
시스템에서 view를 그릴 준비가 될 때까지 기다릴 필요가 없다. worker thread는 자신의 페이스로
canvas에 그릴 수 있다.

SurfaceView 의 subclass를 만들고 SurfaceHolder.Callback interface를 구현해야 한다.

worker thread 에서 surfaceholder 에 접근해서 surface 에 그릴 수 있는데 다음과 같은 방법으로 다가간다

1. Use locakCnavas() to retrieve the canvas.
2. Perform drawing operations on the canvas.
3. Unlock the canvas by calling unlockCanvasAndPost(canvas)


# Drawables
Android 에서는 shapes 와 images 그리는 라이브러리를 제공한다.
표준 클래스를 사용하는 것 외에도 Drawable 을 정의하고 인스턴스화 하는 두 가지 방법이 있다.

- Using an resource image saved in your project.
- Using an XML resource that defines the drawable properties.


## Creating drawables from resource images
resources 로부터 이미지를 참조할 수 있다.

ImageView 에서 image 를 사용하는 방법
```java
ImageView i = new ImageView(this);
i.setImageResource(R.drawable.my_image);
```
Drawable 을 직접 다루는 방법
```java
Resources res = mContext.getResources();
Drawable myImage = res.getDrawable(R.drawable.my_image);
```
XML 의 ImageView 에서 사용하는 방법
```xml
<ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/my_image" />
```


## Creating drawables from XML resources
XML 에 drawable 을 정의한 후,
```xml
<transition xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/image_expand">
    <item android:drawable="@drawable/image_collapse">
</transition>
```
Resources.getDrawable() 을 호출해서 Drawable 을 inflate 한다.
```java
Resources res = mContext.getResources();
TransitionDrawable transition =
    (TransitionDrawable) res.getDrawable(R.drawable.expand_collapse);

ImageView image = (ImageView) findViewById(R.id.toggle_image);
image.setImageDrawable(transition);

// Then you can call the TransitionDrawable object's methods
transition.startTransition(1000);
```


## Shape drawables
ShapeDrawable 객체는 2차원 graphic을 그리기 위한 좋은 선택이다.
```java
public class CustomDrawableView extends View {
  private ShapeDrawable mDrawable;

  public CustomDrawableView(Context context) {
    super(context);

    int x = 10;
    int y = 10;
    int width = 300;
    int height = 50;

    mDrawable = new ShapeDrawable(new OvalShape());
    // If the color isn't set, the shape uses black as the default.
    mDrawable.getPaint().setColor(0xff74AC23);
    // If the bounds aren't set, the shape can't be drawn.
    mDrawable.setBounds(x, y, x + width, y + height);
  }

  protected void onDraw(Canvas canvas) {
    mDrawable.draw(canvas);
  }
}
```



## Canvas.drawRect
drawRect(float left, float top, float right, float bottom, Paint paint)
# Path

# rect
rect.inset
