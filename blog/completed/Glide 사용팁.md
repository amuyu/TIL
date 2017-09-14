# Glide 에서 발생하는 Out Of memory 대처
## Glide.with()
Glide with()는 여러 메소드 가 있다
```
public static RequestManager with(Context context)
public static RequestManager with(Activity activity)
public static RequestManager with(FragmentActivity activity)
public static RequestManager with(Fragment fragment)
```
이유는 각각의 LifeCycle에 맞게 동작하도록 구현되어 있기 때문이다.
예를 들어, Fragment에서 Glide를 사용해 3MB 정도의 이미지를 로드한 후, 백버튼을 눌러 Fragment를 제거하거나 Activity Close한다고 가정해보자
이 때,
with(context)를 사용해서 로드했다면 Glide는 변화가 없다.
with(fragment)를 사용했다면 Glide는 Fragment 의 Lifecycle 이벤트에 맞춰 동작을 멈추고 clear한다
with(activity)를 사용했다면 Activity 의 Lifecycle 이벤트에 맞춰 동작을 멈추고 clear한다 (Activity 가 stop, destory 되어야만 한다)

Glide에서 사용하는 메모리를 관리하기 위해서 with() 호출 시, context 보다는 fragment/activity 호출이 좋을 듯 하다.
```
Glide.with(fragment)
Glide.with(activity)
```

## with(context) 메모리 관리
with 호출 시, fragment/activity 가 아닌 application context를 사용한다면
아래 코드를 추가해서 메모리가 부족할 때, glide 에서 사용하는 메모리를 clear 해줄 수 있다.
```java
public class App extends Application {
    @Override public void onLowMemory() {
        super.onLowMemory();
        Glide.get(this).clearMemory();
    }
    @Override public void onTrimMemory(int level) {
        super.onTrimMemory(level);
        Glide.get(this).trimMemory(level);
    }
}
```

## Adapter에서 Glide 사용하기
Glide를 사용하는 경우에 이미지 호출이 필요할 때마다 다음과 같이 호출해서 사용한다.
```java
Glide.with(this)
  .load(url)
  .centerCrop()
  .placeholder(R.drawable.loading_spinner)
  .into(myImageView);
```
리스트의 경우 Adapter에서 위와 같이 사용하면 리스트 개수만큼 Glide.with를 호출하게 된다.
이러한 호출을 줄이기 위해서 Adpater 클래스에 RequestManager를 변수로 가지도록 구현하고
RequestManager를 통해 load 하도록 구현할 수 있다.
Adapter
```java
class MyAdapter extends WhichEveryOneYouUse {
    private final RequestManager glide;
    MyAdapter(RequestManager glide, ...) {
        this.glide = glide;
        ...
    }
    void getView/onBindViewHolder(... int position) {
        // ... holder magic, and get current item for position
        glide.load... or even loadImage(glide, item.url, holder.image);
    }
}
```
Activity/Fragment 호출 시,
```java
list.setAdapter(new MyAdapter(Glide.with(this), data));
```




# 참고
[1] https://stackoverflow.com/questions/31964737/glide-image-loading-with-application-context/32887693#32887693
[2] https://github.com/bumptech/glide/issues/1135)
[3] http://sub-dev.tistory.com/1
