# 안드로이드 NDK 프로그래밍
## JNI를 이용한 자바와 C/C++인터페이스
안드로이드 애플리케이션 프레임워크는 대부분 자바로 작성돼 있거나 얕은 자바 계층으로 감싸여 있다. 자바와 C/C++을 함께 연결할 수 없었다면 안드로이드에서 안드로이드 네이티브 C/C++코드는 무용지물이 됐을 것이다. JNI 덕분에 누구나 자바에서 C/C++ 함수를 호출할 수 있다.
### 자바 기본 데이터 타입
#### 네이티브 키/값 저장소 빌드
안드로이드 UI에서 사용자가 int/String 값을 key 와 함께 네이티브 메모리에 저장하고 key를 입력해서 네이티브 메모리에서 값을 호출하는 예제
##### 주요 API
###### GetStringUTFChars
java string에서 c 문자열 변환

```jni
const char* lKeyTmp = (*pEnv)->GetStringUTFChars(pEnv,pKey,NULL);
```
임시 버퍼를 생성하므로 사용 후, 반드시 ReleaseStringUTFChars를 해줘야한다.

###### NewStringUTF
C문자열에서 java string 변환

```jni
(*pEnv)->NewStringUTF(pEnv, lEntry->mValue.mString);
```
###### jint
C int 타입은 jint로 형 변환 가능



## 참고
1. 안드로이드 NDK 프로그래밍(book)