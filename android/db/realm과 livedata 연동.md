
LiveData 는 Activity 가 ViewModel 의 변경을 폴링할 필요 없이 스트림 데이터를 ViewModel 에서 Activity로 보내준다.
모든 데이터는 LiveData 로 나타낼 수 있으며 RealmResults 와 RealmObject 와 사용할 수 있다.


```java
public class LiveRealmData<T extends RealmModel> extends LiveData<RealmResults<T>> {

    private RealmResults<T> results;
    private final RealmChangeListener<RealmResults<T>> listener = 
        new RealmChangeListener<RealmResults<T>>() {
            @Override
            public void onChange(RealmResults<T> results) { setValue(results);}
    };

    public LiveRealmData(RealmResults<T> realmResults) {
        results = realmResults;
    }

    @Override
    protected void onActive() {
        results.addChangeListener(listener);
    }

    @Override
    protected void onInactive() {
        results.removeChangeListener(listener);
    }
}
```


# 참고
[Realm과 함께 하는 안드로이드 아키텍처 컴포넌트](https://academy.realm.io/kr/posts/android-architecture-components-and-realm/)