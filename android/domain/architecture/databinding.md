# 데이터 바인딩 라이브러리
데이터 바인딩을 사용하면 view와 model 이 자연스럽게 분리된다.
view-data 간 이벤트 처리가 쉬워진다.
boilerPlate 코드들을 줄일 수 있다.
## 환경
Android 2.1~ 사용 가능
Gralde 1.5.0-alpha1 이상 필요
## gradle.build
dataBinding 요소 추가
```gradle
android {
    ....
    dataBinding {
        enabled = true
    }
}
```
## layout
### 데이터 바인딩 식 작성
layout 루트 태그로 시작, data 요소와 view 루트 요소가 나옴
```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"/>
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.lastName}"/>
   </LinearLayout>
</layout>
```
#### data
data 내 변수는 레이아웃 내에서 사용할 수 있는 속성 설명
```xml
<variable name="user" type="com.example.User"/>
```
 "@{}" 구문을 사용해서 특성 속성에 기록  
 ```xml
 <TextView android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:text="@{user.firstName}"/>
 ```
### data 바인딩
Binding 클래스는 레이아웃 파일의 이름 기준으로 생성
main_activity.xml -> MainActivityBinding
#### 기본적인 생성 방법
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
   super.onCreate(savedInstanceState);
   MainActivityBinding binding = DataBindingUtil.setContentView(this, R.layout.main_activity);
   User user = new User("Test", "User");
   binding.setUser(user);   // layout 에서 작성한 variable에 셋팅
}
```
#### 그 외 생성 방법
inflate를 사용 뷰를 얻는 방법
```java
MainActivityBinding binding = MainActivityBinding.inflate(getLayoutInflater());
```
ListView 어댑터나 RecyclerView 어댑터 내에서 데이터 바인딩 항목을 사용
```java
ListItemBinding binding = ListItemBinding.inflate(layoutInflater, viewGroup, false);
//or
ListItemBinding binding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false);
```
### 이벤트 처리
#### 메서드 참조
데이터 바인딩될 때 리스너 구현이 생성된다.
이벤트 바인딩 식을 작성하고,
```java
public class MyHandlers {
    public void onClickFriend(View view) { ... }
}
```
View에 리스너 할당
```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="handlers" type="com.example.Handlers"/>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"
           android:onClick="@{handlers::onClickFriend}"/>
   </LinearLayout>
</layout>
```
#### 리스너 바인딩
이벤트 발생 시 실행되는 바인딩 식
```java
public class Presenter {
    public void onSaveClick(Task task){}
}
```
click이벤트 바인딩
```xml
<?xml version="1.0" encoding="utf-8"?>
  <layout xmlns:android="http://schemas.android.com/apk/res/android">
      <data>
          <variable name="task" type="com.android.example.Task" />
          <variable name="presenter" type="com.android.example.Presenter" />
      </data>
      <LinearLayout android:layout_width="match_parent" android:layout_height="match_parent">
          <Button android:layout_width="wrap_content" android:layout_height="wrap_content"
          android:onClick="@{() -> presenter.onSaveClick(task)}" />
      </LinearLayout>
  </layout>
```
#### 복잡한 리스너 방지
|클래스 |	리스너 Setter |	특성 |
|---|---|---|
|SearchView |	setOnSearchClickListener(View.OnClickListener) |	android:onSearchClick |
|ZoomControls |	setOnZoomInClickListener(View.OnClickListener) |	android:onZoomIn |
|ZoomControls |	setOnZoomOutClickListener(View.OnClickListener) |	android:onZoomOut |

### 레이아웃 세부정보
#### 가져오기
import 요소가 전혀 사용되지 않거나 한 개 이상 사용될 수 있다
```xml
<data>
    <import type="android.view.View"/>
</data>
```
바인딩 식 내에 View를 사용할 수 있다
```xml
<TextView
   android:text="@{user.lastName}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
```
클래스 이름 간에 충돌이 발생할 때 해당 클래스 중 하나의 이름을 별칭으로 바꿀 수 있다
```xml
<import type="android.view.View"/>
<import type="com.example.real.estate.View"
        alias="Vista"/>
```

### Data Objects
데이터가 변경될 때 이를 알려주는 기능을 데이터 객체에 부여할 수 있다.
Observable 객체, Observable 필드, Observable 컬렉션이라는 세 가지 변경 알림 메커니즘이 있다.
#### Observable 객체
바인딩된 객체에 단일 리스너를 연결하여 그 객체에 대한 모든 속성의 변경 사항을 수신한다.
getter에 Bindable 주석을 할당하고 setter에서 이를 알림으로써 속성 변경을 알릴 수 있다.
```java
private static class User extends BaseObservable {
   private String firstName;
   private String lastName;
   @Bindable
   public String getFirstName() {
       return this.firstName;
   }
   @Bindable
   public String getLastName() {
       return this.lastName;
   }
   public void setFirstName(String firstName) {
       this.firstName = firstName;
       notifyPropertyChanged(BR.firstName);
   }
   public void setLastName(String lastName) {
       this.lastName = lastName;
       notifyPropertyChanged(BR.lastName);
   }
}
```
Bindable 주석은 컴파일 중에 BR 클래스 파일에 항목을 생성한다.

## 식 언어
### null 병합 연산자
?? 는 왼쪽 피연산자가 null이 아니면 왼쪽 피연산자를 선택하고, null이면 오른쪽 피연산자를 선택합니다.

## ListView databinding
### listview 에 데이터를 추가하는 방법 - ListBiding 사용
viewModel 의 데이터 갱신 후, Adapter의 데이터를 set하기 위해서 아래와 같은 형태의 Binding클래스를 생성한다.
```java
public class TasksListBindings {

    @SuppressWarnings("unchecked")
    @BindingAdapter("app:items")
    public static void setItems(ListView listView, List<Task> items) {
        TasksFragment.TasksAdapter adapter = (TasksFragment.TasksAdapter) listView.getAdapter();
        if (adapter != null)
        {
            adapter.replaceData(items);
        }
    }
}
```
위 클래스 생성 후, 아래처럼 xml 의 listview에서 app:items 태그 사용이 가능해진다
```xml
<ListView
      android:id="@+id/tasks_list"
      app:items="@{viewmodel.items}"
      android:layout_width="match_parent"
      android:layout_height="wrap_content" />
```
호출 순서를 정리해보면 ViewModel의 items에서 데이터 갱신이 이루어지면
TasksListBindings 의 setItems 가 호출이 되고 adapter의 데이터 갱신 함수를 호출함으로써
adpater의 데이터 까지 갱싱이 이루어지게 된다.
### ViewModel의 데이터와 Adapter 의 데이터를 분리하면서 얻게 되는 이점?
viewModel의 데이터를 직접 사용하면 viewModel의 데이터가 빠르게 변경되는 경우
adapter에서 view를 구성하기 위해 viewModel 데이터 호출 시점과 실제 데이터 호출의 싱크가 맞지 않는 경우가 발생한다.
이를 막기위해서 binding 클래스에서 view에서 호출되는 값들의 valid 체크를 해줘야하는 코드가 발생하게 된다
예를 들어 viewModel의 데이터가 100개에서 90개로 줄어들었을 때, adapter에서는 100개의 데이터를 보여주려고 하는 경우



## 실제로 해보자
레이아웃 작성,
View에 대한 model 필요,
View 이벤트 필요 -> presenter
Binding 호출




## 참고
[2-way Data Binding on Android!](https://halfthought.wordpress.com/2016/03/23/2-way-data-binding-on-android/)
