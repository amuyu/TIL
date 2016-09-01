# TIL
# 안드로이드
## Doze 모드
안드로이드 6.0 마쉬멜로우에 새롭게 추가된 절전모드
일정시간동안 움직이지 않는 경우, 디바이스는 Doze 모드에 진입하고 앱들은 배터리 소모가 많은 기능, 네트워크 연결, GPS 스캔 등을 활용할 수 없음..
### 참고
- [Doze와 Foreground Service](https://brunch.co.kr/@huewu/3)

## google-services.json 관련 설명
google-services plugin은 google-services.json 파일에서 서비스 이용에 필요한 프로젝트 정보, 클라이언트 정보를 읽어서 처리한다
### 참고
- [The Google Services Gradle Plugin](https://developers.google.com/android/guides/google-services-plugin)

----
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


---
## Android와 Annotation
안드로이드에서는 기본적으로 Java 1.6에서 사용되는 Annotation을 지원한다. 대표적으로 @Override가 있다.


## Dipendency Injection
요즘 안드로이드 개발에서 Dependency Injection 방법을 이용한 개발 방법이 이슈이다.
### 안드로이드에서 @Inject와 @Test 사용의 좋은 점
#### DI의 이점
DI의 이점은 크게 세 가지를 들 수 있다.
- 객체의 생성 주기를 제어한다.
  DI 스타일이 유행하기 전까지는 한 번 생성해서 계속 재활용할 객체의 sigleton 패턴을 직접 구현했다.
  객체 생성 지점을 통제하기 위해서, getInstance() 메서드를 추가하는 등 각 클래스마다 코드를 추가해야 했다.
- 객체에 부가 기능을 추가한다.
  AOP(Aspect Oriented Programming)을 이용하면, 의존하는 객체에 같은 규약을 유지한 채로 로깅, 보안, 트랜잭션 처리, Exception 처리 등의 부가 기능을 추가할 수 있어 반복되는 코드를 줄이고 유연하게 각종 운영 정책을 변경할 수 있다.
- 실행 환경에 따라 구현 객체를 바꿔치기한다.  


#### Andorid에서도 @Inject가 의미 있을까
- 용량에 부담이 된다.
  모바일 기기의 한정된 자원 때문
- 성능에 부담이 된다.
  프레임워크를 사용하면 실행 시에 콜스택이 늘어날 수 있다. 의존 관계를 해석하기 위해 앱 초기 로딩에 더 많은 일을 해야 할 가능성도 크다.
- Dalvik에서는 cglib과 같은 런타임 바이트 코드 생성 라이브러리를 사용할 수 없다.
- 앱 규모가 작아서 DI의 유연성으로 얻는 이득이 크지 않다.
- 안드로이드 기본 프레임워크와 DI 프레임워크의 조화를 고려해야 한다.



### Dagger2
Square에서 만든 Dagger를 Google이 Fork하여 개선한 DI(Dependency injection) 프레임워크
의존성에 따른 객체생성을 추상화하여 보일러플레이트 코드를 줄이고, 변화에 유연하게 만든다.
Dagger2를 이용해 여러 클래스에 의존하는 MVP 모델의 Presenter와 Model객체를 깔끔하게 구현할 수 있다.
#### 중요 Annotation
- @Module : Module로 구성되고, 내부에서 주입할 객체들을 provide해주는 것
- @Provides : 객체 주입에 필요한 내용을 리턴해주는 것
- @Component : Module과 Inject간의 Bridge 같은 역할
- @Inject : 객체의 주입.

#### 사용 방법 정리
##### gradle 설정 추가
```gradle
compile 'com.google.dagger:dagger:2.0'
```
##### 모델 정의
Motor.java
```java
public class Motor { 
    private int speed;
 
    public Motor() {
        this.speed = 0;
    }
 
    public int getSpeed() {
        return speed;
    }
 
    public void accelerate(int value) {
        speed = speed + value;
    }
 
    public void setSpeed(int value) {
        speed = value;
    }
 
    public void brake() {
        speed = 0;
    }
}

```
Bike.java
```java
public class Bike {
 
    private Motor motor;
 
    public Bike(@NonNull Motor motor) {
        this.motor = motor;
    }
 
    public void increaseSpped() {
        this.motor.setSpeed(this.motor.getSpeed() + 1);
    }
 
    public void decreaseSpeed() {
        if (this.motor.getSpeed() > 0) {
            this.motor.setSpeed(this.motor.getSpeed() - 1);
        }
    }
 
    public int getSpeed() {
        return this.motor.getSpeed();
    }
}
```
##### Module 정의
MotorModule.java
```java
@Module
public class MotorModule {
 
    @Provides
    Motor provideMotor() {
        return new Motor();
    }
}
```
BikeModule.java
```java
@Module(
        includes = MotorModule.class
)
public class BikeModule {
 
    @Provides
    Bike provideBike(Motor motor) {
        return new Bike(motor);
    }
}
```
#### Component 정의
MainActivityComponent.java
```java
@Component(
        ...
        modules = {
                ...
                BikeModule.class
        }
)
public interface MainActivityComponent extends ActivityComponent {
    void inject(@NonNull MainActivity mainActivity);
}
```
이렇게 Component를 정의하고 컴파일 하면 "DaggerMainActivityComponent"가 만들어 진다. 이걸 가지고 실질적으로 객체 주입을 진행할 수 있다.
```java
aggerMainActivityComponent.builder()
        ...
        .bikeModule(new BikeModule())
        .build();
```
### Inject 정의
```java
@Inject
Bike bike;
```

#### 참고
- [Android깨알팁4-Dagger2](https://medium.com/@jsuch2362/android-%EA%B9%A8%EC%95%8C%ED%8C%81-4-dagger2-7f38cd9cb11b#.owet5o6uu)
- [Android 개발에서 Dagger2 이용해보기](http://drcarter.tistory.com/169)

### 참고
- [Android에서 @Inject, @Test](http://d2.naver.com/helloworld/342818)

---
