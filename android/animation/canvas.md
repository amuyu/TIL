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




## Canvas.drawRect
drawRect(float left, float top, float right, float bottom, Paint paint)
# Path

# rect
rect.inset
