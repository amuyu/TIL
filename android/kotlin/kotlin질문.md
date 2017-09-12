# context casting 문제
kotlin에서는 casting을 어떻게 해야하는가..
```java
((Activity)context).finish()
```
이런 식으로 하면 되는데 맞는 방법인가
```kotlin
(context as Activity).finish()
```
맞다

# object 와 class 차이

# 뭘 쓸래도 설정이 필요하네
자바에서 코틀린으로 변환할 때, 필요한 것
realm, @.@
dagger 충돌 -
getter/setter
constoructor?
rxandroid 는 어떻게 써야 하나
annocation lifecycle충돌 - 안됨
adapter 의 viewholder 사용
Context 사용 그냥 바로 context
sigleton 사용


## realm 사용
## java.lang.IllegalArgumentException: Api is not part of the schema for this Realm 는 plugin 호출 순서
copyToRealmOrUpdate??
```groovy
apply plugin: 'kotlin-android'
apply plugin: 'realm-android'

dexOptions {
        incremental false
    }
```
객체 생성 createObject
[issue](https://github.com/realm/realm-java/issues/3139)

## list > arraylist 변환

## build error
Error:Execution failed for task ':app:transformClassesWithRealmTransformerForDebug'.
javassist.NotFoundException: com.lazycouple.restapiclient.db.model.Api
