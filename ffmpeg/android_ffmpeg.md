## Android FFmpeg
### Android FFmpeg 빌드
#### 윈도우 + Cygwin 환경
윈도우와 리눅스의 파일 시스템 동작이 다르고 Cygwind이라는 에뮬레이터를 사용하기 때문에
빌드 시, 문제 가 발생한다. (예를 들어 심볼릭 링크 같은 기능) 빌드 시, 발생하는 에러를
막기 위해 설정 파일들을 수정하도록 한다

#### configure 수정
다음과 같이 입력된 내용을
```bash
SLIBNAME_WITH_MAJOR='$(SLIBNAME).$(LIBMAJOR)'
LIB_INSTALL_EXTRA_CMD='$$(RANLIB) "$(LIBDIR)/$(LIBNAME)"'
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_VERSION)'
SLIB_INSTALL_LINKS='$(SLIBNAME_WITH_MAJOR) $(SLIBNAME)'
```
요렇게 수정
```bash
SLIBNAME_WITH_MAJOR='$(SLIBPREF)$(FULLNAME)-$(LIBMAJOR)$(SLIBSUF)'
LIB_INSTALL_EXTRA_CMD='$$(RANLIB) "$(LIBDIR)/$(LIBNAME)"'
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_MAJOR)'
SLIB_INSTALL_LINKS='$(SLIBNAME)'
```
심볼릭 링크 파일 생성을 복사하는 것으로 수정
원래
```bash
ln_s="ln -s -f"
```
요렇게 수정
```bash
ln_s="cp -f"
```
여기서 TEMP DIR 경로를 수정해도 되고 빌드 스크립트에서 수정해도 된다.
(참고한 블로거의 글을 보면 빌드 스크립트에서 변경을 추천한다고 한다.)
원래 셋팅
```bash
# set temporary file name
: ${TMPDIR:=$TEMPDIR}
: ${TMPDIR:=$TMP}
: ${TMPDIR:=/tmp}
```
변경 셋팅
```bash
# set temporary file name
TEMPDIR="c:/ffmpegtmp"
TMP="c:/ffmpegtmp"
: ${TMPDIR:=$TEMPDIR}
: ${TMPDIR:=$TMP}
: ${TMPDIR:="c:/ffmpegtmp"}
```
#### build script 수정
경로 셋팅
이와 같이 수정하지 않으면 파일이 그 위치에 있음에도 불구하고 에러가 발생함
```bash
CUR=`cygpath -m $(pwd)`
TEMPDIR=`cygpath -m /tmp`
TMP=`cygpath -m /tmp`
TMPDIR=`cygpath -m /tmp`
CUR=`cygpath -m $(pwd)`
CPU=arm
PREFIX=$CUR/android/$CPU
```
#### 확인
성공적으로 빌드가 완료되면  ffmpeg-3.0\android\arm\lib와 ffmpeg-3.0\android\arm\include 디렉토리가 생성된다.


### 예제실행 (AndroidFFmpeg)
downloading source code
```bash
git clone https://github.com/appunite/AndroidFFmpeg.git AndroidFFmpeg
cd AndroidFFmpeg
git submodule init
git submodule sync #if you are updating source code
git submodule update
cd library-jni
cd jni
```
download libyuv and configure libs (※ 명령을 실행하기 위해선 cygwin 이나 mingw 필요)
```bash
./fetch.sh
```
build external libraries Download r8e ndk 
[window](https://dl.google.com/android/ndk/android-ndk-r8e-windows-x86_64.zip)
[mac](https://dl.google.com/android/ndk/android-ndk-r8e-darwin-x86_64.tar.bz2)
```bash
export ANDROID_NDK_HOME=/your/path/to/android-ndk
./build_android.sh
```

### 버그
#### AndroidFFmpeg 예제 실행 시 발생 에러 (build_android.sh)
msys 에서 빌드
##### 현상
configure:3411: error: in `/home/noco/AndroidFFmpeg/library-jni/jni/vo-amrwbenc':
configure:3413: error: C compiler cannot create executables
##### 해결
- [github issue](https://github.com/appunite/AndroidFFmpeg/issues/1) 답변을 보면 질문한 사람의 경우 자기 ndk에 i686-android-linux로 시작하는 toolchains이 없어서 에러가 발생했고 build_android.sh 에서 i686-android-linux를 x86으로 수정해서 해결됨
- 내 경우는 더 추가된게 os가 윈도우라서 ndk를 윈도우 ndk를 받아서 해야할 거 같음.. (github에 올린 사람의 경우 mac 사용자임) 윈도우 ndk를 받으니 됨..  

##### 현상
libtool: link: more than one -exported-symbols argument is not allowed
Makefile:409: recipe for target `libfribidi.la' failed
make[3]: *** [libfribidi.la] Error 1
make[3]: Leaving directory `/home/noco/AndroidFFmpeg/library-jni/jni/fribidi/lib'
Makefile:603: recipe for target `install' failed
make[2]: *** [install] Error 2
make[2]: Leaving directory `/home/noco/AndroidFFmpeg/library-jni/jni/fribidi/lib'
Makefile:412: recipe for target `install-recursive' failed
make[1]: *** [install-recursive] Error 1
make[1]: Leaving directory `/home/noco/AndroidFFmpeg/library-jni/jni/fribidi'
Makefile:738: recipe for target `install' failed
make: *** [install] Error 2
##### 해결
- mingw 문제일 수도..

#### FFmpeg 빌드 시 발생 에러 (build_android.sh)
##### 현상
빌드 중 , 다음과 같은 에러 발생
```
LD      libswscale/libswscale.so.4
d:/developer/android/ndk/android-ndk-r10e/toolchains/arm-linux-androideabi-4.9/prebuilt/windows-x86_64/bin/../lib/gcc/arm-linux-androideabi/4.9/../../../../arm-linux-androideabi/bin/ld.exe: error: libavutil/libavutil.so:1:1: syntax error, unexpected '!', expecting $end
d:/developer/android/ndk/android-ndk-r10e/toolchains/arm-linux-androideabi-4.9/prebuilt/windows-x86_64/bin/../lib/gcc/arm-linux-androideabi/4.9/../../../../arm-linux-androideabi/bin/ld.exe: error: libavutil/libavutil.so: not an object or archive
collect2.exe: error: ld returned 1 exit status
```
##### 해결
심볼릭 관련 에러임
configure 수정
```bash
ln_s="ln -s -f"
# 부분을 찾아서 
ln_s="cp -f 변경
```
빌드 스크립트 수정, 템프 경로 변경
```bash
CUR=`cygpath -m $(pwd)`
TEMPDIR=`cygpath -m /tmp`
TMP=`cygpath -m /tmp`
TMPDIR=`cygpath -m /tmp`
PREFIX=$CUR/android/$CPU
```
#### FFmpeg 빌드 시 발생 에러 (컴파일 강좌 참고)
make-standalone-toolchain.sh 실행 권한을 못줌.
##### 해결
스크립트 파일 상단에 !/bin/sh 추가

#### FFmpeg 빌드 시 발생 에러 (컴파일 강좌 참고)
cygwin에서 ndk-build 할 때 에러 발생
##### 현상
```
/Users/eladb/MyWorkspace/android-ndk-r8e/build/gmsl/__gmsl:512: *** non-numeric second argument to 'wordlist' function: ''.  Stop.
```
##### 해결
android-ndk-r8b/android-ndk-r8e 에서 발생하는 에러로
ndk 폴더/build/gmsl/__gmsl 에 파일을 수정 
int_encode = $(__gmsl_tr1) 시작하는 부분을 이렇게 수정..
```sh
int_encode = $(__gmsl_tr1)$(wordlist 1,$(words $1),$(__gmsl_input_int))
```





##### 현상



### 참고
- [AndroidFFmpeg](https://github.com/appunite/AndroidFFmpeg)
- [android-ndk-r8e](https://github.com/appunite/AndroidFFmpeg)
- [ffmpeg 안드로이드 포팅 및 동영상 테스트](http://blog.naver.com/PostView.nhn?blogId=just4u78&logNo=220628698165&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)
  - 빌드 시, 발생하는 에러에 대해 잘 정리가 되어있음
- [윈도우에서 AndroidStudio NDK 'ffmpeg' 빌드](http://devlibch.blogspot.kr/2015/12/androidstudio-ndk-ffmpeg.html)
  - 안드로이드 스튜디오에서 사용 예제