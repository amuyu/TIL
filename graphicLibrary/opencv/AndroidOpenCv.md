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


## 참고
[javacv](https://github.com/bytedeco/javacv)
[Android NDK + OpenCV 카메라 예제 및 프로젝트 생성방법(CMake 사용)](https://webnautes.tistory.com/1054)
[opecv download](https://github.com/opencv/opencv/releases)
[Android 에서 Opencv 설치하고 간단한 예제 실행해보기!!](http://melonicedlatte.com/android/2018/04/07/032920.html)
[Android ndk](https://github.com/googlesamples/android-ndk/)