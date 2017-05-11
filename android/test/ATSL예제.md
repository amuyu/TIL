# Android Testing Codelab 정리
## Unit Testing with JUnit4 and Mockito
### Source sets define product flavors
Tests are stored in the test and androidTest directories in your module's src folder.
#### androidTest
Location of Android tests (tests that are run on a device or emulator)
#### Test
Location of local tests
### Adding Gradle dependencies for JUnit 4
```gradle
// Dependencies for local unit tests
testCompile "junit:junit:$rootProject.ext.junitVersion"
testCompile "org.mockito:mockito-all:$rootProject.ext.mockitoVersion"
testCompile "org.hamcrest:hamcrest-all:$rootProject.ext.hamcrestVersion"
testCompile "org.powermock:powermock-module-junit4:$rootProject.ext.powerMockito"
testCompile "org.powermock:powermock-api-mockito:$rootProject.ext.powerMockito"
```
### Mocking dependencies with Mockito
Instead of relying on actual implementations of these dependencies, we can use mock objects to test our class under test in isolation.
A mock object is a dummy class for which you can define the output and behavior of its method calls based on their input.
## Unit Test 1 : Opening the ‘Add Notes' screen
### clickOnFab_ShowsAddsNoteUi
presenter 의 addNewNote 는 view.showAddNote를 호출한다.
presenter 코드
```java
@Override
public void addNewNote() {
    mNotesView.showAddNote();
}
```
test 코드
```java
@Test
public void clickOnFab_ShowsAddsNoteUi() {
   // When adding a new note
   mNotesPresenter.addNewNote();
   // Then add note UI is shown
   verify(mNotesView).showAddNote();
}
```
## Unit Test 2: Loading notes from presenter
### loadNotesFromRepositoryAndLoadIntoView
repository 에 load data 를 호출하고 data 를 callback 통해 return 받아 view에 전달한다.
presenter 코드
```java
@Override
public void loadNotes(boolean forceUpdate) {
    mNotesView.setProgressIndicator(true);
    if (forceUpdate) {
        mNotesRepository.refreshData();
    }

    // The network request might be handled in a different thread so make sure Espresso knows
    // that the app is busy until the response is handled.
    EspressoIdlingResource.increment(); // App is busy until further notice

    mNotesRepository.getNotes(new NotesRepository.LoadNotesCallback() {
        @Override
        public void onNotesLoaded(List<Note> notes) {
            EspressoIdlingResource.decrement(); // Set app as idle.
            mNotesView.setProgressIndicator(false);
            mNotesView.showNotes(notes);
        }
    });
}
```
test 코드
```java
@Test
public void loadNotesFromRepositoryAndLoadIntoView() {
    // Given an initialized NotesPresenter with initialized notes
    // When loading of Notes is requested
    mNotesPresenter.loadNotes(true);

    // Callback is captured and invoked with stubbed notes
    verify(mNotesRepository).getNotes(mLoadNotesCallbackCaptor.capture());
    mLoadNotesCallbackCaptor.getValue().onNotesLoaded(NOTES);

    // Then progress indicator is hidden and notes are shown in UI
    verify(mNotesView).setProgressIndicator(false);
    verify(mNotesView).showNotes(NOTES);
}
```

## UI Testing on Android with Espresso
The Android Testing Support Library contains the Espresso testing framework that provides APIs to simulate user interactions.
Espresso has a very nice fluent, concise and readable Api to write functional UI tests and was built to make UI testing as frictionless as possible. This means that you can focus on writing tests without having to deal with unreliable and flaky tests.
### Set up Espresso and AndroidJUnitRunner
```gradle
// Android Testing Support Library's runner and rules
androidTestCompile "com.android.support.test:runner:$rootProject.ext.runnerVersion"
androidTestCompile "com.android.support.test:rules:$rootProject.ext.rulesVersion"

// Espresso UI Testing dependencies.
androidTestCompile "com.android.support.test.espresso:espresso-core:$rootProject.ext.espressoVersion"
androidTestCompile "com.android.support.test.espresso:espresso-contrib:$rootProject.ext.espressoVersion"
```
### Set up Android Studio for Android instrumentation tests
Unlike local unit tests, instrumentation tests must be placed in the androidTest source set

## Espresso Test 1: Opening the ‘Add Note' screen
add note button 을 클릭해서 add note screen 이 호출되는지 확인
```java
@Test
public void clickAddNoteButton_opensAddNoteUi() throws Exception {
   // Click on the add note button
   onView(withId(R.id.fab_add_notes)).perform(click());

   // Check if the add note screen is displayed
   onView(withId(R.id.add_note_title)).check(matches(isDisplayed()));
}
```
## Espresso Test 2: Adding a note
```java
@Test
    public void addNoteToNotesList() throws Exception {
        String newNoteTitle = "Espresso";
        String newNoteDescription = "UI testing for Android";

        // Click on the add note button
        onView(withId(R.id.fab_add_notes)).perform(click());

        // Add note title and description
        // Type new note title
        onView(withId(R.id.add_note_title)).perform(typeText(newNoteTitle), closeSoftKeyboard());
        onView(withId(R.id.add_note_description)).perform(typeText(newNoteDescription),
                closeSoftKeyboard()); // Type new note description and close the keyboard

        // Save the note
        onView(withId(R.id.fab_add_notes)).perform(click());

        // Scroll notes list to added note, by finding its description
        onView(withId(R.id.notes_list)).perform(
                scrollTo(hasDescendant(withText(newNoteDescription))));

        // Verify note is displayed on screen
        onView(withItemText(newNoteDescription)).check(matches(isDisplayed()));
    }
```
## Espresso test 3: Clicking a note


# 참고
[Android Testing Codelab](https://codelabs.developers.google.com/codelabs/android-testing/#5)
[Mockito-features 번역](https://github.com/mockito/mockito/wiki/Mockito-features-in-Korean)
