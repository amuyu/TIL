# Tip
## Manager 확인
OpenCV Manager가 설치되어있는지 확인한다
OpenCVLoader.initAsync(OpenCVLoader.OPENCV_VERSION_2_4_8, this, mLoaderCallback)
## OpenCV init
initDebug를 호출해서 OpenCV를 초기화 해야한다.
```java
if (!OpenCVLoader.initDebug()) {
			Log.v(TAG, "init OpenCV");
		}
```
