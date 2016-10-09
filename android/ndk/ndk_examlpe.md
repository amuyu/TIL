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

### 네이티브 코드에서 자바 객체 참조
#### Store 내의 객체 레퍼런스 저장
##### 주요 API
###### GetNewGlobalRef
객체 레퍼런스를 유지하기 위해 전역 레퍼런스 생성  
```jni
jobject lColor = (*pEnv)->NewGlobalRef(pEnv, pColor);
```
###### DeleteGlobalRef
전역 레퍼런스 해제  
```jni
(*pEnv)->DeleteGlobalRef(pEnv, lColor);
```

### 네이티브 코드에서 예외 전달
#### Store에서 예외 발생시키기
##### 주요 API
###### FindClass
java class 객체 호출  
```jni
jclass lClass = (*pEnv)->FindClass(pEnv,
        "com/packtpub/exception/InvalidTypeException");
```
###### ThrowNew
java exception 던지기 호출  
```jni
(*pEnv)->ThrowNew(pEnv, lClass, "Type is invalid.");
```

### 자바 배열 처리
자바 배열로 네이티브와 통신하며, 전형적인 C 배열로 저장되게 설정한다
#### Store에서 객체 레퍼런스 저장
Guava 도우미 라이브러리 http://code.google.com/p/guava-libraries
##### 주요 API
###### NewIntArray
정수형 배열을 초기화  
```jni
jintArray lJavaArray = (*pEnv)->NewIntArray(pEnv, lEntry->mLength);
```
###### SetIntArrayRegion
네이티브 int 버퍼 내용 복사  
```jni
(*pEnv)->SetIntArrayRegion(pEnv, lJavaArray, 0,
            lEntry->mLength, lEntry->mValue.mIntegerArray);
```
###### GetIntArrayRegion
배열의 크기 측정  
```jni
jsize lLength = (*pEnv)->GetArrayLength(pEnv, pIntegerArray);
```

###### GetIntArrayRegion
자바 배열을 네이티브로 저장  
```jni
(*pEnv)->GetIntArrayRegion(pEnv, pIntegerArray, 0, lLength, lArray);
```

###### NewObjectArray
객체 배열을 초기화  
```jni
jobjectArray lJavaArray = (*pEnv)->NewObjectArray(
            pEnv, lEntry->mLength, lColorClass, NULL);
```  

###### SetObjectArrayElement
네이티브 객체 내용 복사  
```jni
(*pEnv)->SetObjectArrayElement(pEnv, lJavaArray, i,
                lEntry->mValue.mColorArray[i]);
```  

###### GetObjectArrayElement
배열 요소를 하나씩 저장  
```jni
jobject lLocalColor = (*pEnv)->GetObjectArrayElement(pEnv, pColorArray, i);
```

### 자바 배열 관련 API
#### 기본형 타입
jbooleanArray, jbyteArray, jcharArray, jdoubleArray, jfloatArray, jlongArray, jshortArray
#### API
##### ArrayRegion
C 자바 배열의 내용을 네이티브 배열 혹은 반대로 복사한다.  
Get<기본형>ArrayRegion, Set<기본형>ArrayRegion
##### ArrayElements
ArrayRegion과 유사한 기능 수행, 임시로 할당된 버퍼상에서 동작하거나 대상 배열을 직접 가리킨다
Get<기본형>ArrayElements, Set<기본형>ArrayElements, Release<기본형>ArrayElements
##### ArrayCritical
대상 배열에 직접 접근(복사 대신)을 제공한다. JNI 함수와 자바 콜백이 수행돼서는 안된다
Get<기본형>ArrayCritical(), Release<기본형>ArrayCritical()




## 참고
1. 안드로이드 NDK 프로그래밍(book)