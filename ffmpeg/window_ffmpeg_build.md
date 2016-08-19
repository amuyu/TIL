## FFmpeg 윈도우 빌드
### MSYS 1.0 설치
1. [MinGW-builds](https://sourceforge.net/projects/mingwbuilds/files/external-binary-packages/) 에서 가장 최근 버전 다운로드
2. 적당한 경로에 압축 해제 - c:\msys
3. MinGW-x64 설치 - [MinGW-w64](https://sourceforge.net/projects/mingw-w64/)
4. 2번에서 압축을 해제한 경로에 3번에서 설치한 mingw64 폴더를 복사
5. 2번 경로로 이동해서 msys.bat 실행
6. 터미널에서 다음 3줄 실행
   ```bash
   echo 'export PATH=.:/local/bin:/bin:/mingw64/bin' > .profile
   echo 'git config --global core.autocrlf false' >> .profile
   source .profile
   ```

### FFmpeg 빌드
1. [FFmpeg](https://www.ffmpeg.org/download.html) 사이트로 이동하여 소스를 다운받는다. 다운 받은 후, msys 적당한 경로에 압축을 푼다.
   최신 소스를 받으려면 git으로 받고, Release 버전을 받으려면 하단 Release 부분에 있는 파일을 받아서 사용한다.     
2. ffmpeg 경로로 이동해서 configure를 수행한다.
   ```bash
   ./configure --arch=x86_64 --enable-stripping --disable-w32threads --enable-memalign-hack --disable-static --enable-shared --enable-version3 --disable-doc
   ```
   노트북이라서 그런지 한참 걸림
3. make 실행 코어수에 따라
   ```bash
   make -j2
   ```
4. make install 실행
   ```bash
   make install
   ```
5. 설치 확인
   ```bash
   ffmpeg -v
   ```


### 버그
#### yasm not found
ffmpeg configure 시, 발생하는 에러
##### 해결
[yasm](http://yasm.tortall.net/Download.html)사이트로 이동해서 환경에 맞는 (Win63.exe) 파일을 다운받아서, yasm으로 이름을 바꾼뒤에 `C:\msys\mingw64\bin` 에 복사한 후, configure 다시 실행한다.


### 참고
- [ffmpeg 64bit windows build](http://ttsun.tistory.com/80)
- [MinGW Windows 64 bit 에 설치하기](http://kkikkodev.tistory.com/41)
- [yasm/nasm not found or too old](https://ffmpeg.org/pipermail/ffmpeg-user/2014-May/021270.html)
- [ffmpeg 컴파일하기](http://egloos.zum.com/sallykim/v/2399353)
  - 설치하면서 발생하는 버그 등 첨부되어 있음
- [Windows 기반의 FFmpeg 다운로드 및 간단한 동영상 변환](http://logg.tistory.com/64)