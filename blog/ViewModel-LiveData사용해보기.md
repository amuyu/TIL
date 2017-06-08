
Android lifecycle-aware components ViewModel과 LiveData
구글 새로운 component인 ViewModel과 LiveData를 사용해보자

## ViewModel
[ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel.html)
은 UI 관련된 data를 저장하고 관리하기 하는 클래스이다.
보통 Activity나 Fragment가 다시 recreate 되는 상황에(screen rotation 같은) 갖고 있던
data들은 clear가 된다. 이러한 상황에서 data를 잃지 않고 view를 구성하기 위해서
onSaveInstanceState 나 create 될 때, data를 다시 읽는 다던지하는 구현이 필요하다.

ViewModel을 사용하면 이런 구현이 필요 없어진다. ViewModel은 Activity/fragment lifecycle을 따라
동작하는데 생성된 시점에서 Activity/fragment가 finish() 되기 전까지 데이터를 유지하는 기능을 가지고 있다.
ViewModel 사용 방법을 알아보자

### 사용방법
#### download
viewmodel 을 사용하기 위해서 dependencies 를 추가한다.
*project/build.gradle*
```groovy
allprojects {
    repositories {
        jcenter()
        maven { url 'https://maven.google.com' }
    }
}
```
*app/build.gradle*
```groovy
compile "android.arch.lifecycle:runtime:1.0.0-alpha1"
compile "android.arch.lifecycle:extensions:1.0.0-alpha1"
annotationProcessor "android.arch.lifecycle:compiler:1.0.0-alpha1"
```
#### create
'ViewModelProviders'를 사용해서 ViewModel 객체를 생성한다.
```java
// this = Fragment / Activity
MyViewModel myviewmodel = ViewModelProviders.of(this).get(ChronometerViewModel.class)
```

#### 예제1
예제를 통해 사용방법을 알아보자. 전체 소스는 [codelab](https://codelabs.developers.google.com/codelabs/android-lifecycles/#0)을 참고하면 된다.
*step2/ChronoActivity2*
Activity가 onCreate될 때,
'ChronometerViewModel'을 생성하고 startdate를 가져와 Chronometer에 셋팅한다
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    // The ViewModelStore provides a new ViewModel or one previously created.
    ChronometerViewModel chronometerViewModel
            = ViewModelProviders.of(this).get(ChronometerViewModel.class);

    // Get the chronometer reference
    Chronometer chronometer = (Chronometer) findViewById(R.id.chronometer);

    if (chronometerViewModel.getStartDate() == null) {
        // If the start date is not defined, it's a new ViewModel so set it.
        long startTime = SystemClock.elapsedRealtime();
        chronometerViewModel.setStartDate(startTime);
        chronometer.setBase(startTime);
    } else {
        // Otherwise the ViewModel has been retained, set the chronometer's base to the original
        // starting time.
        chronometer.setBase(chronometerViewModel.getStartDate());
    }

    chronometer.start();
}
```
*step2/ChronometerViewModel*
Chronometer 에 셋팅할 값이 startDate 가 있다.
```java
public class ChronometerViewModel extends ViewModel {

    @Nullable
    private Long startDate;

    @Nullable
    public Long getStartDate() {
        return startDate;
    }

    public void setStartDate(final long startDate) {
        this.startDate = startDate;
    }
}
```
앱을 실행한 후, 스마트폰을 좌우로 회전시켜보면 view는 새로 create되지만 Chronometer는 초기 값을 잃어버리지
않고 표시되는 것을 확인할 수 있다. 홈버튼으로 나갔다 다시 앱으로 돌아와도 값은 유지된다.
백버튼으로 나갔다 앱을 실행하면 값이 초기화 되는 것을 볼 수 있다.
이렇게 ViewModel은 Activity/Fragment 가 finish 되서 destroy 되기 전까지는 값을 유지한다.

#### 예제2
LiveData를 사용법을 알아보기 전에 'Step3' 을 조금 수정해서 ViewModel 만 사용한 예제를 살펴보자.
참고로 이 코드는 codelab 프로젝트에 없다.
*step3/ChronoActivity3rev*
ViewModle을 생성하고 ViewModel 로부터 Update 이벤트를 받아 view를 갱신한다.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    setContentView(R.layout.chrono_activity_3);

    viewModel = ViewModelProviders.of(this).get(ChronometerViewModelrev.class);
    viewModel.setMyListener(new ChronometerViewModelrev.MyListener() {
        @Override
        public void onUpdate(long aLong) {
            String newText = ChronoActivity3rev.this.getResources().getString(
                    R.string.seconds, aLong);
            ((TextView) findViewById(R.id.timer_textview)).setText(newText);
        }
    });
}
```
*step3/ChronometerViewModelrev*
'Timer'를 사용해서 1초마다 이벤트를 Activity로 전달한다.
```java
public ChronometerViewModelrev() {
    mInitialTime = SystemClock.elapsedRealtime();
    // Update the elapsed time every second.
    timer.scheduleAtFixedRate(new TimerTask() {
        @Override
        public void run() {
            final long newValue = (SystemClock.elapsedRealtime() - mInitialTime) / 1000;
            // setValue() cannot be called from a background thread so post to main thread.
            new Handler(Looper.getMainLooper()).post(new Runnable() {
                @Override
                public void run() {
                    if(myListener != null) myListener.onUpdate(newValue);

                }
            });
        }
    }, ONE_SECOND, ONE_SECOND);
}
```
앱을 실행해보면 예제1과 마찬가지로 스마트폰을 좌우로 회전하거나 홈화면으로 나갔다 돌아와도
값이 유지되는 것을 확인할 수 있다. 그런데 Activity의 onUpdate 리스너에 로그를 남겨보면
홈화면에 나가있는 상태에서도 이벤트를 받아 view를 갱신하는 것을 확인할 수 있다.

view가 보이지 않는 상태에서는 데이터는 유지되더라도 view를 갱신할 필요는 없다. 다시 view가 보일 때,
갱신된 값만 보여주면 불필요한 리소스 낭비를 막을 수 있을 것 같다.

LiveData를 사용하면 이런 불필요한 갱신을 막을 수 있다.

## LiveData
[LiveData](https://developer.android.com/topic/libraries/architecture/livedata.html) 는
acitivity/fragment lifecyle에 따라 동작하며 STARTED/RESUMED state일 때만 데이터 변화 이벤트를
observer 에게 전달한다.
### 사용방법
#### download
ViewModel과 설정이 동일하다
#### observe
LiveData는 observe 호출을 통해서 데이터 갱신 이벤트를 받을 수 있다.
```java
LiveData.get(getActivity()).observe(this, location -> {
                   // update UI
                });
```

#### 예제3
LiveData를 사용한 예제를 살펴보자.
*step3/ChronoActivity3*
'LiveDataTimerViewModel'에서 LiveData를 호출해 observe 하는 것을 볼 수 있다.
elapsedTime 에 값이 변화가 생기면 onChanged가 호출된다.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    setContentView(R.layout.chrono_activity_3);
    mLiveDataTimerViewModel = ViewModelProviders.of(this).get(LiveDataTimerViewModel.class);
    subscribe();
}

private void subscribe() {
    final Observer<Long> elapsedTimeObserver = new Observer<Long>() {
        @Override
        public void onChanged(@Nullable final Long aLong) {
            String newText = ChronoActivity3.this.getResources().getString(
                    R.string.seconds, aLong);
            ((TextView) findViewById(R.id.timer_textview)).setText(newText);
            Log.d("ChronoActivity3", "Updating timer");
        }
    };
    mLiveDataTimerViewModel.getElapsedTime().observe(this, elapsedTimeObserver);
}
```
*step3/LiveDataTimerViewModel*
Timer를 사용해 1초마다 LiveData인 mElapsedTime 에 값을 변경한다.
```java
public LiveDataTimerViewModel() {
    mInitialTime = SystemClock.elapsedRealtime();
    // Update the elapsed time every second.
    timer.scheduleAtFixedRate(new TimerTask() {
        @Override
        public void run() {
            final long newValue = (SystemClock.elapsedRealtime() - mInitialTime) / 1000;
            // setValue() cannot be called from a background thread so post to main thread.
            new Handler(Looper.getMainLooper()).post(new Runnable() {
                @Override
                public void run() {
                    mElapsedTime.setValue(newValue);
                }
            });
        }
    }, ONE_SECOND, ONE_SECOND);
}
```
앱을 실행해보면 예제2와 마찬가지로 스마트폰을 좌우로 회전하거나 홈화면으로 나갔다 돌아와도
값이 유지되는 것을 확인할 수 있다. 그런데 예제2와는 다르게 홈화면에 나가있는 상태에서는
Activity의 onChanged 리스너의 로그가 발생하지 않는 것을 확인할 수 있다.

LiveData는 lifecycle이 STARTED/RESUMED state일 때만 dispatch하기 때문에
view가 보일 때만 view를 갱신할 수 있다.


## 결론
Lifecycle-aware components인 LiveData, ViewModel을 사용하면 exception이 발생하지 않도록
lifecycle을 신경써서 구현해야 했던 작업들을 줄일 수 있고 앱의 성능을 높이는데 도움이 될것 같다.

# 참고
[android-lifecycles codelab](https://codelabs.developers.google.com/codelabs/android-lifecycles/#1)
[Android Architecture Components](https://developer.android.com/topic/libraries/architecture/index.html)
