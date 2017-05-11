#Espresso

## Espresso setUp
### build.gradle 설정
dependency
```gradle
androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.2'
androidTestCompile 'com.android.support.test:runner:0.5'
```
instrumnetaion runner
```gradle
testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
```
### Example JUnit4 test using Rules
```java
@RunWith(AndroidJUnit4.class)
@LargeTest
public class HelloWorldEspressoTest {

    @Rule
    public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule(MainActivity.class);

    @Test
    public void listGoesOverTheFold() {
        onView(withText("Hello world!")).check(matches(isDisplayed()));
    }
}
```


## Espresso basics
You can still safely operate on views by implementing your own `ViewAction`s and `ViewAssertion`s.

Here's an overview of the main components of Espresso
- Espresso - Entry point to interactions with views(via onView and onData)
- ViewMatchers - onView
- ViewActions - ViewInteraction.perform()
- ViewAssertions - ViewInteraction.check()

Example:
```java
onView(withId(R.id.my_view))      // withId(R.id.my_view) is a ViewMatcher
  .perform(click())               // click() is a ViewAction
  .check(matches(isDisplayed())); // matches(isDisplayed()) is a ViewAssertion
```  

## Finding a view with onView
Finding a view by its R.id is as simple as:
```java
onView(withId(R.id.my_view));
onView(allOf(withId(R.id.my_view), withText("Hello!")));
onView(allOf(withId(R.id.my_view), not(withText("Unwanted"))));
```
Note: If the target view is inside an AdapterView (such as ListView, GridView, Spinner) the onView method might not work and it is recommended to use the onData method instead.

## Performing an action on a view
For example, to click on the view:
```java
onView(...).perform(click());
onView(...).perform(typeText("Hello"), click());
onView(...).perform(scrollTo(), click());
```

## Checking if a view fulfills an assertion
```java
onView(...).check(matches(withText("Hello!")));
onView(allOf(withId(...), withText("Hello!"))).check(matches(isDisplayed()));
```

## Fragment Test
```java
//fevi-regacy
MainActivity activity = activityTestRule.getActivity();
        FragmentManager fragmentManager = activity.getSupportFragmentManager();
        movieListFragment = (MovieListFragment) fragmentManager.findFragmentById(R.id.content_frame);
```



## viewMatcher
[espresso 가 제공하는 viewmatcher](https://android.googlesource.com/platform/frameworks/testing/+/android-support-test/espresso/core/src/main/java/android/support/test/espresso/matcher/ViewMatchers.java)

## 참고
[Android Testing Support Library](https://google.github.io/android-testing-support-library/docs/espresso/basics/)
[번역](http://eyeahs.github.io/blog/2016/05/03/espresso-2-espresso/)
