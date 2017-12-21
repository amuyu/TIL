# Canvas drawing 이 제대로 동작하지 않을 때
Custom View 에서 다음과 같이 호를 그린 후, 그 안에 비트맵이 위치하도록 구현을 하고 있었다.
```java
protected void onDraw(Canvas canvas) {
  ...
  canvas.drawArc(rect,startAngle,sweepAngle,true, paint);
  paint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
  canvas.drawBitmap(bitmap, null, arcRect, paint);
  ...
}
```
구현 후, 앱을 빌드해보니 PorterDuff.Mode.SRC_IN 이 적용되지 않아 bitmap만 표시되었다.
(arcRect 영역의 사각형 모양으로..)
방법을 찾다가 다음의 방법으로 원하는 결과를 얻을 수 있었다.
```java
protected void onDraw(Canvas canvas) {
  ...
  Bitmap output = Bitmap.createBitmap((int)rect.width(),(int)rect.height(), Bitmap.Config.ARGB_8888);
  Canvas c = new Canvas(output);
  c.drawArc(rect,startAngle,sweepAngle,true, paint);
  paint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
  c.drawBitmap(bitmap, null, arcRect, paint);
  canvas.drawBitmap(output, null, rect, null);
  ...
}
```
근데 canvas를 새로 생성해서 그리고 다시 view 의 canvas 에 그리는 방식이 좋은 방법은
아닐 것 같아 좀 더 방법을 찾던 중, 이런 내용을 발견했다.
> PorterDuff.Mode.CLEAR doesn't work with hardware acceleration. Just set
view.setLayerType(View.LAYER_TYPE_SOFTWARE,null);
Works perfectly for me.

이 글을 통해 하드웨어 가속모드에서 지원하지 않는 API 가 있다는 것을 알게 되었고 다시 처음의 코드로 돌아가
생성자 위치에서 layerType을 `LAYER_TYPE_SOFTWARE` 로 변경해보았다.
```java
public View(Context context) {
    super(context);
    setLayerType(View.LAYER_TYPE_SOFTWARE, null);
}
...
protected void onDraw(Canvas canvas) {
  ...
  canvas.drawArc(rect,startAngle,sweepAngle,true, paint);
  paint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
  canvas.drawBitmap(bitmap, null, arcRect, paint);
  ...
}
```
그리고 빌드 해보니 원하던대로 호안에 bitmap 이 들어간 결과를 얻을 수 있었다.
하드웨어 가속모드에서 지원하지 않는 API를 사용할 경우, `setLayerType(View.LAYER_TYPE_SOFTWARE, null)`
를 호출해 하드웨어 가속을 끄고 drawing 을 하면 된다.

## 참고
[stackoverflow](https://stackoverflow.com/a/44607874/6759520)
[Unsupported Drawing Operations](https://developer.android.com/guide/topics/graphics/hardware-accel.html#unsupported)
