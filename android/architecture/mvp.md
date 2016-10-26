## Android와 개발 패턴
많은 개발자들이 안드로이드에서는 MVC 자체의 모습보다는 MVVM이나 MVP가 가장 적합하다는 말을 많이 하고 있다. Activity가 View를 포함한 클래스이기 때문에 나타나는 현상이라고 볼 수 있다.

### 일반 코드
초창기 안드로이드 개발 코드의 모습, Activity 내에 이벤트 핸들링하는 처리나 뷰에 접근하는 코드들이 모두 존재함, 이러한 코드는 코드가 커지면 커질수록 가독성도 떨어지며 유지보수가 힘들어지는 코드로 가기 쉬움
```java
public class MainAcivity extends Activity {

	@Override
	public void onCreate(Bundle saveInstance) {
	   super.onCreate(saveInstance);
	   setContent(R.layout.main);

	   TextView textView = (TextView) findViewById(R.id.btn_confirm);
	   textView.setText("Non-Clicked");


	   findViewById(R.id.btn_confirm).setOnClickListener(new View.OnClickListener() {

	       @Override
	       public void onClick(View view) {
	           TextView textView = (TextView) findViewById(R.id.btn_confirm);
	           textView.setText(getClickedText());
	       }
	   });
	}

	String getClickedText() {
	   return "Clicked!!!";
	}

}
```

### MVC 모델
View에 상관없는 로직을 MainModel로 분리를 해서 Activity는 VIew와 Click Event를 처리하는 모습으로 변화함. 하지만 Activity는 View와 Controller 특성을 모두 가지고 있어 여전히 mvc 모델에 어긋나는 형태를 보여준다
외부의 모든 이벤트를 Controller(Activity)에서 처리하되 VIew에 관여는 하지 않는 것이 원칙입니다. 하지만 ACtivity의 특성상 View에 관여하지 않을 수 없습니다. 결국 Android에서는 MVC 구조를 계속적으로 유지하기에는 무리가 있습니다.
```java
public class MainAcivity extends Activity {
	private MainModel mainModel;

	@Override
	public void onCreate(Bundle saveInstance) {
		super.onCreate(saveInstance);
		setContent(R.layout.main);
		
		mainModel = new MainModel();
		
		TextView textView = (TextView) findViewById(R.id.btn_confirm);
		textView.setText("Non-Clicked");
		
		
		findViewById(R.id.btn_confirm)
			.setOnClickListener(new View.OnClickListener(){
			
				@Override
				public void onClick(View view) {
					String text = mainModel.getClickedText();
					TextView textView = (TextView) findViewById(R.id.btn_confirm);
					textView.setText(mainModel.getClickedText());
				}
			});
	}
}
```

### MVVM 패턴
ViewModel로 View에 대한 처리가 분리됨, View에 대한 처리가 복잡해질수록 ViewModel이 거대해지게 되고 상대적으로 Activity는 아무런 역할도 하지 않는 혀태의 클래스로 변모하게 됨
Controller의 성격을 지닌 Activity가 실질적으로 아무런 역할을 하지 못하고 ViewModel에 치중된 모습을 보여줌으로써 다른 형태의 Activity 클래스가 되어버림
ViewModel이 뷰와의 상호작용 및 제어에 집중한다. ViewModel에서 직접 뷰의 이벤트를 처리하기 때문에 Controller의 역할도 함께 수행하게 되어 점점 코드가 집중되는 구조가 될 수 있다. 또한 초기화와 외부 이벤트를 제외하고는 Activity의 역할이 모호해진다.
```java
public class MainAcivity extends Activity {

	private MainViewModel mainViewModel;

	@Override
	public void onCreate(Bundle saveInstance) {
		super.onCreate(saveInstance);
		setContent(R.layout.main);
		
		mainViewModel = new MainViewModel(MainActivity.this);
		
	}

}
```
```java
public class MainModel {

	public String getClickedText() {
		return "Clicked!!!";
	}

}
```
```java
public class MainViewModel {

	private Activity activity;
	private MainModel mainModel;
	private TextView textView;
	
	public MainViewModel(Activity activity) {
		this.activity = activity;
		this.mainModel = new MainModel();
		initView(activity);
	}

	private void initView(Activity activity) {
	
		textView = (TextView) activity.findViewById(R.id.btn_confirm);
		textView.setText("Non-Clicked");
	
		activity.findViewById(R.id.btn_confirm)
			.setOnClickListener(new View.OnClickListener(){
			
				@Override
				public void onClick(View view) {
					String text = mainModel.getClickedText();
					textView.setText(text);
				}
			});
	}
	
}
```
### MVP 모델
- View는 실제 View에 대한 직접적인 접근을 담당한다.
- view에서 발생하는 이벤트는 직접 핸들링하나 Presenter에 위임하도록 한다.
- Presenter는 실질적인 기능을 제어하는 곳으로써 ViewController로써 이해하면 쉽다.
- Model은 비즈니스 로직을 실질적으로 수행한다.

Presenter : View 는 1:1로 매칭하며 View Presenter가 주요 기능을 관장하되 실제 view에서 발생하는 이벤트는 Presenter (이벤트:View -> Presenter)로 전달하여 처리하도록 하고 다시 처리된 결과는 Presenter가 View에 전달하도록 하여 처리한다.

View에 대한 직접적인 접근은 Activity가 하도록 하고 이에 대한 제어는 Presenter가 하도록 하고 있다.

```java
public interface MainPresenter {

	void setView(MainPresenter.View view);

	void onConfirm();

	public interface View {
		void setConfirmText(String text);
	}
	
}
```
```java
public class MainAcivity extends Activity implements MainPresenter.View {

	private MainPresenter mainPresenter;
	
	private Button confirmButton;

	@Override
	public void onCreate(Bundle saveInstance) {
		super.onCreate(saveInstance);
		setContent(R.layout.main);
		
		mainPresenter = new MainPresenterImpl(MainActivity.this);
		mainPresenter.setView(this);
		
		confirmButton = (Button) findViewById(R.id.btn_confirm);
		confirmButton.setOnClickListener(new View.OnClick() {
			@Override
			public void onClick(View view) {
				mainPresenter.onConfirm();
			}
		});
	}
	
	@Override
	public void setButtonText(String text) {
		confirmButton.setText(text);
	}
}
```
```java
public class MainModel {

	public String getClickedText() {
		return "Clicked!!!";
	}

}
```
```java
public class MainPresenterImpl implements MainPresenter {

    private Activity activity;
    private MainPresenter.View view;

    public MainPresenterImpl(Activity activity) {
        this.activity = activity;
        this.mainModel = new MainModel();
    }
    
    @Override
    public void setView(MainPresenter.View view) {
    	this.view = view;
    }
    
    @Override
    public void onConfirm() {
    	if (view != null) {
    		view.setConfirmText(mainModel.getClickedText());
    	}
    }
    

}
```

### Test
MVP에서 테스트는 Model Layer, View Layer, Presenter Layer 3개의 레이어를 따로 테스트한다. 
Model은 전통적인 JUnit, 
Presenter는 구글에서 제공하는 Android Test Support Library(ATSL)을 사용한다.. 
View는 ATSL에 더하여 Espresso를 사용해서 테스트 한다.
종합예시는 아래 참고에 **GDG-ATSL-ON-MVP**를 참고한다.


### 참고
- [Android에 테스트 도입하기](http://blog.dramancompany.com/2016/08/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%97%90-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EB%8F%84%EC%9E%85%ED%95%98%EA%B8%B0/)
- [Android와 개발 패턴](http://tosslab.github.io/android/2015/03/01/01.Android-mvc-mvvm-mvp.html)
- [Android Testing Support Library](https://google.github.io/android-testing-support-library/)
- [GDG-ATSL-ON-MVP](https://github.com/ZeroBrain/GDG-ATSL-ON-MVP)
- [Android Testing Codelab](https://codelabs.developers.google.com/codelabs/android-testing/#1)
- [androidTest-JUnit4, Espresso를 이용한 테스트 코드 작성](http://thdev.tech/androiddev/2016/05/04/Android-Test-Example.html)

## MVP 좀더 자세히
### 구조
![MVP구조](https://codelabs.developers.google.com/codelabs/android-testing/img/f910e640272817e9.png)
#### Model
The model provides and stores the internal data.
ex) in notes app, the notes are represented in the model
#### View
The view handles the display of data.
ex) in notes app,
- A view that provides an interface for displaying a list of notes (NotesContract.View) that is implemented in its own fragment (NotesFragment).
- A view that displays details of a single note (NoteDetailContract.View) and corresponding fragment (NoteDetailFragment).
- A view where the user enters data for a new note (user action) (AddNoteContract.View) and corresponding fragment (AddNoteFragment).  

#### Presenter
The presenter sits between the model and view.
ex) in notes app,
- NotesPresenter that loads a list of notes from the API.
- NoteDetailPresenter that loads a single note.
- AddNotePresenter that saves a new note.

### MVP를 적용하는 이유?
- TDD(Test-driven development)의 가능성
- View와 Model 간의 구분이 가능
- View와 Model의 사용법이 분리되면서 Clean code가 가능하다

### 구글 예제 (TODO-MVP)
TODO-MVP는 다음과 같은 구조를 가지고 있다.
![이미지](https://raw.githubusercontent.com/wiki/googlesamples/android-architecture/images/mvp.png)

### Android Testing Codelab 예제
#### Notes app structure: Feature separation by packages
Instead of using a package by layer approach, We have structured the application by package per feature. This greatly improves readability and modularizes the app in a way that parts of it can be changed independently from each other. Let's take a quick look  

Package: com.example.android.testing.notes  

|pacakge|feature|
|---|---|
|.addnote|Adding a new note|
|.data|Storage of notes|
|.notedetail|Showing details for a single note|
|.statistics|The statistics screen|
|.util|Utility classes used in various parts of the app|

#### Explore the "Notes" Features
##### Showing a list of notes
We'll be looking at these classes:  

|class|description|
|---|---|
|NotesActivity|The entry point into our app. Hosts the NotesFragment|
|NotesContract.View|Defines the View layer for this feature|
|NotesContract.UserActionsListener|Defines the interaction between the View and Presenter layers|
|NotesFragment|Concrete implementation of the View layer|
|NotePresenter|The Presenter layer, implements the NotesContract.UserActionsListener to listen for user actions. It has a reference to the View to update it when the model changes|
 
###### View layer
###### Presenter layer





### 참고
- [simple-mvp](https://github.com/tinmegali/simple-mvp)
- [adapter 너는 누구냐?](https://medium.com/@jsuch2362/adapter-%EB%88%84%EA%B5%AC%EB%83%90-%EB%84%8C-data-view-2db7eff11c20#.xe3ygwcr4)
- [Android TODO MVP 어떻게 적용할까?](http://thdev.tech/androiddev/2016/06/14/Android-TODO-MVP-Example.html)
- [Google Sample Basic MVP](https://github.com/googlesamples/android-architecture/tree/todo-mvp/)