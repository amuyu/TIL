# Android Testing Codelab 정리
## MVP 구조
### 앱 구조
package: com.example.android.testing.notes

|package|description|
|---|---|
|.addnote|note 추가|
|.data|Data|
|.notedetail|상세내용 보여주기|
|.notes|목록 보여주기|
|.statistics|화면 통계|
|.util|다양한 도구|
### Notes 특징
|class|description|
|---|---|
|NotesActivity|앱의 시작 지점|
|NotesContract.View|view layer 정의|
|NotesContract.UserActionsListener|view와 presenter interaction 정의|
|NotesFragment|Concrete implementation of the View layer|
|NotesPresenter|Presenter layer|
#### View Layer
NoteFragment 는 NotesContract.View 에 정의된 interface를 실행하는 클래스이다.
**NotesFragment.java**
**NotesContract.java**
#### Presenter Layer
**NotePresenter.java**
**NotesContract.java**
#### Connecting the View and Presenter layers
##### Open the NotesFragment
#### Creating the NotesPresenter

