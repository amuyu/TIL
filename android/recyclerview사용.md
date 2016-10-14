# RecyclerView 사용
AndroidStudio 에서 RecyclerView 사용하기
## Gradle 설정
```gradle
dependencies {
    compile 'com.android.support:recyclerview-v7:21.+'
}
```
## Xml 코드 작성
```xml
<android.support.v7.widget.RecyclerView
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
</android.support.v7.widget.RecyclerView>
```

## Adpater 생성
RecyclerView.Adapter 를 상속받는 Adapter 를 생성한다
```java
public class RecyclerViewAdapter extends RecyclerView.Adapter {
    private List items;
    RecyclerViewAdapter(List items) {        
        this.items = items;
    }

    @Override
    public ListItemViewHolder onCreateViewHolder(
            ViewGroup viewGroup, int viewType) {
        View itemView = LayoutInflater.
                from(viewGroup.getContext()).
                inflate(R.layout.layout, viewGroup, false);
        return new ListItemViewHolder(itemView, viewType);
    }

    @Override
    public void onBindViewHolder(
            ListItemViewHolder viewHolder, int position) {
        String item = items.get(position);
        viewHolder.label.setText(item);        
    }

    @Override
    public int getItemCount() {
        return items.size();
    }

    public final static class ListItemViewHolder 
           extends RecyclerView.ViewHolder {
        // ViewHolder
    }
}
```

## Activity 코드 작성
화면 UI 에서 resycleview 셋팅
```java
recyclerView.setAdapter(new RecyclerViewAdapter(items));
recyclerView.setLayoutManager(new LinearLayoutManager(getApplicationContext()));
recyclerView.setItemAnimator(new DefaultItemAnimator());
```




## 참고
[http://www.kmshack.kr/2014/10/android-recyclerview/](http://www.kmshack.kr/android-recyclerview/)
[Android support v7 widget RecyclerView 사용하기](http://iw90.tistory.com/214)