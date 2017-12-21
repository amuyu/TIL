# PorterDuff.mode
porterduff.mode 가 안 먹힐 때
```java
view.setLayerType(View.LAYER_TYPE_SOFTWARE,null);
```
stackoverflow 답변 중,,
> PorterDuff.Mode.CLEAR doesn't work with hardware acceleration. Just set
view.setLayerType(View.LAYER_TYPE_SOFTWARE,null);
Works perfectly for me.

[stackoverflow](https://stackoverflow.com/a/44607874/6759520)


# 참고
[ColorFilter로 ImageView에 어두운 효과주기](http://swalloow.tistory.com/89)
[PorterDuff.mode](https://developer.android.com/reference/android/graphics/PorterDuff.Mode.html)
[Unsupported Drawing Operations](https://developer.android.com/guide/topics/graphics/hardware-accel.html#unsupported)
