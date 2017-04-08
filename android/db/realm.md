
## Realm 이란? 렘
Mobile Database

## 사용하는 이유
- 기존 데이터베이스보다 속도가 빠르다
- ORM과 같은 뛰어난 사용성
- RealmResults이 자동으로 DB의 현재 상태를 반영한다
- 오픈소스


## 사용방법
### Gradle 설정
```gradle
// app-gradle
compile 'io.realm:realm-android:0.87.4'

// proguard
-keep class io.realm.annotations.RealmModule
-keep @io.realm.annotations.RealmModule class *
-keep class io.realm.internal.Keep
-keep @io.realm.internal.Keep class * { *; }
-dontwarn javax.**
-dontwarn io.realm.**
```
### RealmConfiguration 생성
패키지의 "files" 디렉토리의 Realm파일을 저장하는 RealmConfiguration 생성하기.
```java
RealmConfiguration realmConfig = new RealmConfiguration.Builder(context).build();
Realm.setDefaultConfiguration(realmConfig);
```
### Realm 인스턴스 얻기
```
Realm realm = Realm.getDefaultInstance();
```
### 테이블 생성
Realm에서는 VO 객체를 통해서 테이블이 생성된다.
```java
public class UserVO extends RealmObject{

    @PrimaryKey
    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```
name을 primaryKey로 해서 테이블 생성
### 데이터 검색
```java
mRealm = Realm.getInstance(this);
private RealmResults<유저vo> getUserList(){
    return mRealm.where(UserVO.class).findAll();
}
```
### 데이터 추가
```java
mRealm = Realm.getInstance(this);
private void insertuserData(){
    mRealm.beginTransaction();
    UserVO user = mRealm.createObject(UserVO.class);
    user.setName("John");
    user.setAge(27);
    mRealm.commitTransaction();
}
```
### 데이터 삭제
```java
mRealm = Realm.getInstance(this);
private void deleteuserData(){
    mRealm.beginTransaction();
    RealmResults<UserVO> userList = mRealm.where(UserVO.class).findAll();
    userList.remove(0);
    mRealm.commitTransaction();
}
```
### 질의 예
2살 보다 어린 모든 개에 대한 Realm 질의
```java
final RealmResults<도그> puppies = realm.where(Dog.class).lessThan("age", 2).findAll();
puppies.size(); // => Realm에 아직 개가 추가되지 않았기 때문에 0
```
### 리스너
데이터가 변하면 리스너들에게 알림이 갑니다
```java
puppies.addChangeListener(new RealmChangeListener<RealmResults<Dog>>() {
    @Override
    public void onChange(RealmResults<Dog> results) {
        // 질의 결과는 실시간으로 갱신됩니다
        puppies.size(); // => 1
    }
});
```
### 백그라운드 갱신
비동기 갱신
```java
realm.executeTransactionAsync(new Realm.Transaction() {
    @Override
    public void execute(Realm bgRealm) {
        Dog dog = bgRealm.where(Dog.class).equalTo("age", 1).findFirst();
        dog.setAge(3);
    }
}, new Realm.Transaction.OnSuccess() {
    @Override
    public void onSuccess() {
    	// 원래 질의, Realm 객체는 자동으로 갱신된다.
    	puppies.size(); // => 0 2살 미만의 강아지가 더 이상 없기 때문에
    	managedDog.getAge(); // => 3 강아지의 나이가 업데이트 되었음
    }
});
```

## 드라마앤컴퍼니에서 말하는 Realm
리멤버에서 GreenDAO를 사용해왔음, 그래서 두 프레임워크를 비교함
> GreenDAO : Sqlite 기반 ORM 프레임워크  

### 벤치마크
두 프레임워크 성능 비교
![GreenDAO와 Realm insert 비교](http://i2.wp.com/developer.dramancompany.com/wp-content/uploads/2016/03/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA-2016-03-14-%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE-4.45.04.png?resize=684%2C474)
![GreenDAO와 Realm select 비교](http://i1.wp.com/developer.dramancompany.com/wp-content/uploads/2016/03/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA-2016-03-14-%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE-7.57.59.png?resize=657%2C429)

### 사용성
Sqlite는 객체를 그대로 DB에 저장할 수 없으므로 테이블 간의 관계를 고민하며 스키마를 구성해야 하고 조인 쿼리를 필수로 사용해야 한다. 하지만 Realm은 테이블 간의 관계를 has로 표현하여 설계와 쿼리를 덜 고민하도록 한다.

### 제약사항과 선택
RealObject를 상속받는 클래스의 customize가 허용되지 않는다. Realm으로부터 받아온 객체는 쓰레드 간 전달이 불가능하다.
- RealmObject 클래스의 customize는 static util 클래스를 만들어 대체
- 쓰레드 간 전달은 Realm이 제공하는 트랜잭션, Async Method와 Auto Refresh를 통해 해결할 수 있음

### Realm의 단점
#### BadVersion
findAllAsync(), findAllSortedAsync()에서 발생하는 문제   findAll, findAllSorted로 대체하면 발생하지 않음. 0.88.0 부터 해결되었다고 함
#### SharedGroup
Realm.getDefaultInstance()에서 발생하는 문제, 해결방법 없음
#### 다중 쓰레드에서의 Realm 객체 관리
다중 쓰레드 환경에서 시간차를 예방하는 것이 힘들다.
#### 쿼리가 부족하다
- Collate Localized ASC – Java 코드에서 결과값을 한번 더 Sorting 하였습니다.
- CASE WHEN – int 값을 저장하는 Language Order Column을 별도로 생성하였습니다.
- MATCH – 명함은 데이터가 많으므로 Contains로는 검색 속도가 나오지 않습니다(Table을 Full Scan 하기 때문입니다). Full Text Search를 활용해야 원하는 속도를 얻을 수 있습니다. 하지만 Full Text Search의 MATCH 쿼리는 Sqlite에서만 지원합니다. 그래서 Sqlite로 명함 Index를 저장, 검색하고 Realm에서 명함을 가져오는 방식으로 DB를 조합하였습니다.


## 참고
- [Android DB Realm 사용하기](http://iw90.tistory.com/261)
- [Realm 데이터베이스, 제대로 알고 안드로이드에서 사용하기](http://blog.dramancompany.com/2016/03/realm-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4/)
- [realm 사용 - android](https://devgooda.wordpress.com/2016/08/29/realm-%EC%82%AC%EC%9A%A9/)
- [realm ref site](https://realm.io/docs/java/latest/#getting-started)
