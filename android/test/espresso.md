#Espresso


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
## Performing an action on a view
## Checking if a view fulfills an assertion
## Fragment Test
```java
//fevi-regacy 
MainActivity activity = activityTestRule.getActivity();
        FragmentManager fragmentManager = activity.getSupportFragmentManager();
        movieListFragment = (MovieListFragment) fragmentManager.findFragmentById(R.id.content_frame);
```


## 참고
[Android Testing Support Library](https://google.github.io/android-testing-support-library/docs/espresso/basics/)