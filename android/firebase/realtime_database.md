
# Firebase Realtime Database
NoSQL 클라우드 데이터베이스로 실시간으로 데이터가 동기화되고
오프라인일 때도 데이터를 사용할 수 있습니다. (온라인 상태로 변경되면 서버에 호출함)
데이터는 JSON 으로 저장되고 연결된 모든 클라이언트에 실시간으로 동기화됩니다.

## 주요 기능
실시간, 오프라인, 클라이언트 기기에서 액세스 가능

## 오프라인으로 데이터 쓰기
클라이언트의 네트워크 연결이 끊겨도 앱은 계속 정상적으로 작동합니다.

Firebase 데이터베이스에 연결된 모든 클라이언트는 자체적으로 활성 데이터의 내부 버전을 유지합니다. 데이터를 쓰면 우선 로컬 버전에 기록됩니다. 그런 다음 Firebase 클라이언트가 해당 데이터를 원격 데이터베이스 서버 및 다른 클라이언트와 '최선을 다해' 동기화합니다.

## Write
다음과 같은 형태로 사용자 정보를 데이터베이스에 입력합니다.
```java
DatabaseReference.child("users").child(userId).setValue(user);
```
기본 데이터 베이스에 `users` 하위 항목을 만들고
그 아래에 `userId` 하위 항목을 만들고 사용자 정보를 기록하는 명령입니다.

## Read
데이터를 읽기 위해서 addValueEventListener() 또는 addListenerForSingleValueEvent() 메소드를 사용합니다.
다음과 같은 형태로 `users` 항목에 userId 항목의 사용자 정보를 가져옵니다.
```java
DatabaseReference.child("users").child(userId).addListenerForSingleValueEvent(
                new ValueEventListener() {
                    @Override
                    public void onDataChange(DataSnapshot dataSnapshot) {
                        //
                        User user = dataSnapshot.getValue(User.class);
                    }
                  });
```
addListenerForSingleValueEvent 는 addValueEventListener 와 다르게 데이터를 한 번만 읽을 때 사용합니다.

## updateChildren
다른 하위 노드를 덮어쓰지 않고 특정 하위 노드에 동시에 쓰려면 updateChildren() 메소드를 사용합니다.
updateChildren()을 호출할 때 키의 경로를 지정하여 하위 수준 값을 업데이트할 수 있습니다.
```java
// Create new post at /user-posts/$userid/$postid and at
// /posts/$postid simultaneously
String key = DatabaseReference.child("posts").push().getKey();
Post post = new Post(userId, username, title, body);
Map<String, Object> postValues = post.toMap();

Map<String, Object> childUpdates = new HashMap<>();
childUpdates.put("/posts/" + key, postValues);
childUpdates.put("/user-posts/" + userId + "/" + key, postValues);

DatabaseReference.updateChildren(childUpdates);
```
Post 값을 /posts/ 와 /user-posts/ 에 동시에 업데이트 할 수 있습니다.

## FirebaseRecyclerAdapter
Firebase 와 RecyclerView를 연동할 수 있다.

## 데이터 정렬 및 필터링
시간 데이터베이스의 Query 클래스를 사용하여 키, 값 또는 하위 값으로 정렬된 데이터를 검색할 수 있습니다.
데이터 정렬 후, 검색을 할 수 있다.


## 참고
[Firebase 실시간 데이터베이스](https://firebase.google.com/docs/database/?hl=ko)
