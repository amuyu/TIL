
# kotlin
## 좋은 점
생산성이 좋아진다~

## Setup
### app/build.gradle
```groovy
apply plugin: 'kotlin-android'
```
### project/build.gradle
```groovy
buildscript {
    ext.kotlin_version = '1.1.2-3'
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.1'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

## grammar
### 상속
extends 나 implements 대신 ':' 으로 상속을 표시함
### function
#### 정의
```kotlin
fun double(x: Int): Int {
}
fun 함수명(변수: 변수Type): 리턴타입 {
}
```
#### 사용
```kotlin
val result = double(20)
Sample().foo()
```
#### Extension Function
To declare an extension function, we need to prefix its name with a receiver type, i.e. the type being extended.
The following adds a swap function to MutableList<Int>:
```kotlin
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // 'this' corresponds to the list
    this[index1] = this[index2]
    this[index2] = tmp
}
val l = mutableListOf(1, 2, 3)
l.swap(0, 2) // 'this' inside 'swap()' will hold the value of 'l'
```
#### Infix
```kotlin
// Define extension to Int
infix fun Int.shl(x: Int): Int {
...
}

// call extension function using infix notation

1 shl 2

// is the same as

1.shl(2)
```
#### default argument
```kotlin
fun read(b: Array<Byte>, off: Int = 0, len: Int = b.size()) {
...
}
```
#### named argument
```kotlin
fun reformat(str: String,
             normalizeCase: Boolean = true,
             upperCaseFirstLetter: Boolean = true,
             divideByCamelHumps: Boolean = false,
             wordSeparator: Char = ' ') {
...
}
```
위와 같이 정의된 함수에서 wordSeparator의 값을 전달하고 싶을 때, 다음과 같이 호출한다.
```kotlin
reformat(str,
    normalizeCase = true,
    upperCaseFirstLetter = true,
    divideByCamelHumps = false,
    wordSeparator = '_'
)
```
하지만 kotlin에서는 argument에 이름을 줄 수 있어 아래와 같이 호출 할 수 있다.
```kotlin
reformat(str, wordSeparator = '_')
```
#### Unit return
아무것도 리턴하지 않을 때 리턴 타입을 Unit으로 한다.
```kotlin
fun printHello(name: String?): Unit {
    if (name != null)
        println("Hello ${name}")
    else
        println("Hi there!")
    // `return Unit` or `return` is optional
}
```
Unit은 선언하지 않아도 된다
```kotlin
fun printHello(name: String?) {
    ...
}
```
#### Single-Expression
함수에서 단일문을 사용한다면 중괄호를 생략할 수 있다
```kotlin
fun double(x: Int): Int = x * 2
```
리턴 타입이 유추가 가능하면 리턴 타입 생략 가능하다.
```kotlin
fun double(x: Int): Int = x * 2
```
#### Expicit return type
block(중괄호)을 정의된 함수는 block 안에 리턴 타입을 명시해야 한다. Unit 타입 빼고
#### higher-order function 고차함수
전달인자로 함수를 받고 함수를 리턴할 수 있다
```kotlin
fun <T> lock(lock: Lock, body: () -> T): T {
    lock.lock()
    try {
        return body()
    }
    finally {
        lock.unlock()
    }
}

fun toBeSynchronized() = sharedResource.operation()
val result = lock(lock, ::toBeSynchronized)
```
lambda 표현식은 다음과 같다
```kotlin
fun <T, R> List<T>.map(transform: (T) -> R): List<R> {
    val result = arrayListOf<R>()
    for (item in this)
        result.add(transform(item))
    return result
}

val doubled = ints.map { value -> value * 2 }
```




### Generic
```kotlin
interface Generic<in T> {
  fun setItem(item: T)
}
```

### findViewId
```java
// java 형식
변수Type 변수명 = (변수 Type 형변환) findViewById()
// Kotlin 형식
val 변수명: 변수Type = findViewById() as 변수Type
val 변수명 = findViewById() as 변수Type
```
### lambda
kotlin은 기본으로 lambda를 지원함 (자바의 경우 dependency 설정이 필요함)

# Kotlin Extension
layout/view 에 접근하기 쉽게 한다 (findViewId 등의 코드가 필요없음)
## Setup
### app/build.gradle
```groovy
apply plugin: 'kotlin-android-extensions'
```

# Android kotlin
## Activity 생성
Activity 생성 코드 비교
```java
public class MainActivity extends Activity {
  @Override
  protected void onCreate(@Nullable Bundle savedInstanceState) {
  }
}
```
```kotlin
class MainActivity : Activity() {
    override fun onCreate(savedInstanceState: Bundle?) {
    }
}
```
Java에서는 onCreate 에서 '@Nullable'로 사용하던 표현이 kotlin에선 간단하게 '?'로 사용할 수 있다.
## getter/setter
Java에서는 getter/setter를 구현해서 사용했지만
```java
// TextView.java
public float getAlpha();
public void setAlpha(float alpha);

TextView tv = //
tv.getAlpha();
tv.setAlpha(0f);
```
kotlin에서는 자동으로 변환해준다.
```kotlin
val tv = //
tv.alpha
tv.alpha = 0f;
```
## for문, higher-order function use
리스트에서 for 문을 돌며 아이템에서 어떤 작업을 할 때, java는 다음과 같이 작성한다
```java
MainActivity.java

LinearLayout views = //...
for (int i=0; i<views.getChildCount(); i++) {
  View view = views.getChildAt(i);
  // TODO
}
```
kotlin 언어로 변경하면
```kotlin
MainActivity.kt

val view = //...
for (index in 0 until views.childCount) {
  val view = views.getChildAt(index)
  // TODO
}
```
kotlin에서는 higher-order function 을 지원하는데 이를 이용하면
```kotlin
val view = //...
view.forEach { view ->
  // TODO
}

fun ViewGroup.forEach(action: (View) -> Unit) {
  for (index in 0 until views.childCount) {
    action(getChildAt(index))
  }
}
```
## object expression and declaration
for 문 반복에서 주로 다음과 같은 lamda 식을 사용합니다.
```java
List<Int> lists = ..
for (int val : lists) {
  // TODO
}
```
lists를 iterator 하는 방식으로 동작을 하게 되는데 kotlin 에서는
리스트의 클래스 중에서 iterator 메소드를 수정해서 호출 할 수 있습니다.
이렇게 클래스의 일부를 수정하기 위해서 object 선언이 필요합니다.
```kotlin
val lists = ..
for (int val in lists.children()) {
  // TODO
}

fun IntArray.children() = object : Iterable<Int> {
        override fun iterator() = object : Iterator<Int> {
            var index = 0
            override fun hasNext() = (index+2) < size
            override fun next() : Int {
                index = index +2
                return get(index)
            }
        }
    }
```
## inline
inline 함수 사용
```java
SQLiteDatabase db = //..
db.beginTransaction();
try {
  db.delete("users", "first_name = ? ", new String[] { "jake"});
  db.setTransactionSuccessful();
} finally {
  db.endTransaction():
}
```
위와 같은 코드를 inline함수를 사용하면 다음과 같이 작성할 수 있다.
```kotlin
val db = // ...
db.transaction {
  db.delete("users", "first_name = ? ", arrayOf("jake"))
}

inline fun SQLiteDatabase.transaction(body: () -> Unit) {
  beginTransaction()
  try {
    body()
    setTransactionSuccessful()
  } finally {
    endTransation()
  }
}
```
## Properties and Fields
### get/set
property 선언의 full syntax
```
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```
initializer 와 getter, setter 는 선태적으로 입력할 수 있다.
```kotlin
var allByDefault: Int? // error: explicit initializer required, default getter and setter implied
var init = 1 // has type int, default getter and setter

// custom getter
val isEmpty: Boolean
    get() = this.size == 0
// custom getter and setter
var stringRepresentation: String
    get() = this.toString()
    set(value) {
        setDataFromString(value) // parses the string and assigns values to other properties
    }    
```
### backing field

```kotlin
var counter = 0 // the initializer value is written directly to the backing field
    set(value) {
        if (value >= 0) field = value
    }
```


### lateinit/lazy
늦은 초기화 변수 선언
lateinit : var 변수 초기화
lazy : val 변수 초기화
#### lateinit
```kotlin
fun main() {
	val test = Test()
	test.subject = "제목 초기화"
	println("subject ${test.subject}")
}

class Test {
	lateinit var subject: String
}
```
- var(mutable)만 사용 가능
- null을 사용해서 초기화 불필요
- 늦은 초기화이므로 초기화 전에 사용하면 오류 발생
- lateinit property subject has not been initialized
- 변수에 대한 setter/getter 사용할 수 없음
#### lazy
변수 사용 시점에 초기화 진행
```kotlin
fun main(args: Array<String>) {
    val test = Test()
    test.test()
}

class Test {
    init {
        println("init")
    }

    val subject by lazy {
        println("lazy initialized")
        "제목 초기화"
	}

    fun test() {
        println("not initialized")
        println("subject one : $subject")
        println("subject two : $subject")
        println("subject three : $subject")
    }
}
```
출력 결과
```
init
not initialized
lazy initialized
subject one : 제목 초기화
subject two : 제목 초기화
subject three : 제목 초기화
```
### Delegates
getter와 setter 구현을 다른 객체에 넘기는 것이 가능
#### Delegates
Delegate 사용 예제
```kotlin
class Example {
    var p: String by Delegate()
}

class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }

    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) {
        println("$value has been assigned to '${property.name} in $thisRef.'")
    }
}
```
```kotlin
val e = Example()
println(e.p)

// result
Example@33a17727, thank you for delegating ‘p’ to me!
```

#### Delegates.observable (lazy)
```kotlin
private var name by Delegates.observable("jane") {
            prop,old, new -> println("name changed from $old to $new")
        }

fun main() {
  print("name:$name")
  name="kane"
  print("name:$name")
}        
```
출력 결과
```
name:jane
name changed from jane to kane
name:kane
```
#### Delegates.notNull
설정되지 않은 상태에서 getter가 호출될 경우 크래시 발생
#### bindView
'kotterknife' kotlin 버전의 butterknife
## 코틀린의 유용한 함수들
### let()
let()은 함수를 호출하는 객체를 이어지는 블록의 인자로 넘기고, 블록의 결과값을 반환합니다.
```kotlin
fun <T, R> T.let(block: (T) -> R): R
```
사용 예, 일반적으로 padding 값을 설정하는 코드
```
val padding = TypedValue.applyDimension(
        TypedValue.COMPLEX_UNIT_DIP, 16f, resources.displayMetrics).toInt()
// 왼쪽, 오른쪽 padding 설정
setPadding(padding, 0, padding, 0)
```
let을 사용하면 불필요한 선언을 방지할 수 있음
```
TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 16f,
        resources.displayMetrics).toInt().let { padding ->
    // 계산된 값을 padding 이라는 이름의 인자로 받음
    setPadding(padding, 0, padding, 0)
}
```
let 을 '?'과 사용하면 'if(null != obj)' 를 대체 할 수 있음
```
// null일 수 있는 변수 obj
var obj: String?

// ...작업 수행...

// obj가 null이 아닐 경우 작업 수행 (기존 방식)
if (null != obj) {
    Toast.makeText(applicationContext, obj, Toast.LENGTH_LONG).show()
}

// obj가 null이 아닐 경우 작업 수행 (Safe calls + let 사용)
obj?.let {
    Toast.makeText(applicationContext, it, Toast.LENGTH_LONG).show()
}
```
### apply()
함수를 호출하는 객체를 이어지는 블록의 리시버 로 전달하고, 객체 자체를 반환합니다.
리시버란, 바로 이어지는 블록 내에서 메서드 및 속성에 바로 접근할 수 있도록 할 객체를 의미합니다.
```
fun <T> T.apply(block: T.() -> Unit): T
```
사용 예, LayoutParams 객체를 생성하고 속성을 지정하는 코드
```
val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT)
param.gravity = Gravity.CENTER_HORIZONTAL
param.weight = 1f
param.topMargin = 100
param.bottomMargin = 100
```
apply를 사용하면
```
val param = LinearLayout.LayoutParams(0, LinearLayout.LayoutParams.WRAP_CONTENT).apply {
    gravity = Gravity.CENTER_HORIZONTAL
    weight = 1f
    topMargin = 100
    bottomMargin = 100
}
```
### run()
run() 함수는 인자가 없는 익명 함수처럼 동작하는 형태와 객체에서 호출하는 형태 총 두 가지가 있습니다.
인자가 없는 경우, 인자 없는 익명 함수처럼 사용할 수 있습니다.
```
fun <R> run(block: () -> R): R
```
객체에서 호출하는 경우,
```
fun <T, R> T.run(block: T.() -> R): R
```
객체를 리시버로 전달받으므로, 특정 객체의 메서드나 필드를 연속적으로 호출하거나 값을 할당할 때 사용합니다.
apply()와 비슷하지만 apply()는 새로운 객체를 생성함과 동시에 연속된 작업이 필요할 때 사용하고
run()은 이미 생성된 객체에 연속된 작업이 필요할 때 사용한다는 점이 조금 다릅니다.

### let apply run with 비교
let 도 apply 처럼 사용이 가능하다
let은 객체를 사용하여 다른 메서드를 실행하거나 연산을 수행할 때 사용하기 편하고
apply는 객체의 메서드 및 속성에 접근해서 값을 변경하는데 쓰기 편하다
run 은 let과 apply 은 혼합형처럼 보인다.
apply()는 새로운 객체를 생성하고 객체에 대한 연속된 작업 수행 후, 해당 객체를 사용할 때 사용하고
run()은 해당 객체를 사용할 필요가 없거나 연속된 작업에 대한 결과가 필요할 때 사용하면 될 듯 하다.
╔══════════╦═════════════════╦═══════════════╦═══════════════╗
║ Function ║ Receiver (this) ║ Argument (it) ║    Result     ║
╠══════════╬═════════════════╬═══════════════╬═══════════════╣
║ let      ║ this@MyClass    ║ String("...") ║ Int(42)       ║
║ run      ║ String("...")   ║ N\A           ║ Int(42)       ║
║ run*     ║ this@MyClass    ║ N\A           ║ Int(42)       ║
║ with*    ║ String("...")   ║ N\A           ║ Int(42)       ║
║ apply    ║ String("...")   ║ N\A           ║ String("...") ║
║ also     ║ this@MyClass    ║ String("...") ║ String("...") ║
╚══════════╩═════════════════╩═══════════════╩═══════════════╝

## Static Fields
Companion object 를 사용해서 static field 로 만들어준다
다음과 같이 companion object 키워드를 사용한다.
```kotlin
class Key(val value: Int) {
    companion object {
        val COMPARATOR: Comparator<Key> = compareBy<Key> { it.value }
        fun bar() {}
    }
}
```
위와 같이 작성하면 아래와 같은 형태로 호출할 수 있다.
```kotlin
Key.COMPARATOR
Key.bar()
```
### java 에서 호출하는 경우
java에서 코틀린 static field를 호출하기 위해서는 @JvmField, @JvmStatic annotation 을 사용한다.
다음과 같이 field 키워드 앞에 annotation 을 붙여준다.
```kotlin
class Key(val value: Int) {
    companion object {
        @JvmField val COMPARATOR: Comparator<Key> = compareBy<Key> { it.value }
        @JvmStatic fun bar() {}
    }
}
```
위와 같이 작성하면 java에서 아래와 같은 형태로 호출할 수 있다.
```java
Key.COMPARATOR;
Key.bar();
```

### databinding 사용하기
```groovy
apply plugin: 'kotlin-kapt'
...
dependencies {
    kapt "com.android.databinding:compiler:2.3.2"
}
```


### dagger 사용하기


### TypeDef
kotlin에서 TypeDef 사용하기
1
```kotlin
iimport android.support.annotation.IntDef
import kotlin.annotation.AnnotationRetention
class MyClass {
companion object {
const val ITEM_SERVICES = 0L
const val ITEM_PORTFOLIO = 1L
  }
  @IntDef(ITEM_SERVICES, ITEM_PORTFOLIO)
  @Retention(AnnotationRetention.SOURCE)
annotation class ITEM
}
```
2
```kotlin
@Retention(AnnotationRetention.SOURCE)
@StringDef(ListAnnotation.DIALOG, ListAnnotation.TOSTS, ListAnnotation.ALERTS, ListAnnotation.SELECTORS, ListAnnotation.PROGRESS_DIALOG)
annotation class ListName


object ListAnnotation {
    const val DIALOG = "Dialogs"
    const val TOSTS = "Toasts"
    const val ALERTS = "Alerts"
    const val SELECTORS = "Selectors"
    const val PROGRESS_DIALOG = "ProgressDialog"
}
```
[Typedef annotations example in kotlin (IntDef)](http://makingiants.github.io/blog/typedef-annotations-example-in-kotlin-intdef/)
[Again: Supporting Android Typedefs in Kotlin](http://www.tonicartos.nz/2015/11/again-supporting-android-typedefs-in.html)
[android-movie-mvp](https://github.com/KotlinID/android-movie-mvp)

# show Kotlin Bytecode 로 중간 체크 가능 

# 참고
[Android Kotlin 시작하기](http://thdev.tech/androiddev/2017/07/09/Kotlin-Android-Start.html)
[Android 개발을 수주해서 Kotlin을 제대로 써봤더니 최고였다](https://gist.github.com/Hazealign/1bbc586ded1649a8f08f)
[Getting started with Android and Kotlin](https://kotlinlang.org/docs/tutorials/kotlin-android.html)
[코틀린의 유용한 함수들 - let, apply, run, with](http://kunny.github.io/lecture/kotlin/2016/07/06/kotlin_let_apply_run_with/)
[The difference between Kotlin’s functions: ‘let’, ‘apply’, ‘with’, ‘run’ and ‘also’](https://medium.com/@tpolansk/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8)
[What's Good About Kotlin in Android Development](http://pluu.github.io/blog/android/2017/01/15/whats_good_about_kotlin_in_android_development/)
[kotlin 예제](https://github.com/taehwandev/AndroidBase)
[제이크옹 kotlin 발표 자료](https://speakerdeck.com/jakewharton/life-is-great-and-everything-will-be-ok-kotlin-is-here-google-io-2017)
[kortlin-android-mvp](https://github.com/general-mobile/kotlin-android-mvp-starter?utm_source=android-arsenal.com&utm_medium=referral&utm_campaign=5791)
[kotlin 코딩 팁](https://cchcc.github.io/blog/Kotlin-코딩-팁/)
[android-kotlin-demo](https://github.com/yodle/android-kotlin-demo)
[코틀린에는 static이 없다? - companion object](http://kunny.github.io/lecture/kotlin/2016/07/10/kotlin_companion_object/)
[Calling Kotlin from Java](https://kotlinlang.org/docs/reference/java-to-kotlin-interop.html)
[Singleton in kotlin](https://medium.com/@adinugroho/singleton-in-kotlin-502f80fd8a63)
[똑똑, 프로젝트에 코틀린을 도입하려고 합니다.](http://woowabros.github.io/experience/2017/07/18/introduction-to-kotlin-in-baeminfresh.html)
[클래스 위임은 어떻게 동작하는가](https://medium.com/@cwdoh/kotlin의-클래스-위임은-어떻게-동작하는가-c14dcbbb08ad)

## 참고소스
[kotlin 으로 짠 DI](https://github.com/SalomonBrys/Kodein)
