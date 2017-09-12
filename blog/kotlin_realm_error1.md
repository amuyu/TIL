# Android kotlin 변환 중 발생한 realm 관련 에러 - IllegalArgumentException
java 에서 kotlin 으로 변환 중, realm 관련 에러가 발생했다.
`java.lang.IllegalArgumentException: class is not part of the schema for this Realm..`

realm.createObject 호출 시, 발생한다.

발생하게 된 원인을 찾아보면 gradle 파일에 플러그인을 작성할 때, java에서 kotlin 으로 변환하다보니
아래와 같은 순서로 플러그인이 추가되게 되었다.
```groovy
apply plugin: 'realm-android'
apply plugin: 'kotlin-android'
```
플러그인을 호출 순서를 다음과 같이 `realm-android` 전에 `kotlin-andorid` 가 오도록 수정한다.
```groovy
apply plugin: 'kotlin-android'
apply plugin: 'realm-android'
```
그리고 다시 빌드해서 실행하면 에러가 사라지게 된다.

## 참고
[github issue](https://github.com/realm/realm-java/issues/3139)
