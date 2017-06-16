
# Room Persistence Library


## Database
Roomdatabase 를 상속받아 사용하고, @database annotation 을 사용해서 entities와 version 정의한다.
```java
@Database(entities = {User.class, Book.class, Loan.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
}
```
database 클래스에 dao 클래스들을 정의한다
```java
public abstract UserDao userModel();
public abstract BookDao bookModel();
public abstract LoanDao loanModel();
```
## Entity
table
## DAO
query를 정의한다 interface 클래스로 database 클래스의 멤버변수로 호출해서 실행한다
- @Query
- @Insert
- @Delete
- @Update  
## DatabaseInitializer
정의한 DAO 클래스를 실행하는 클래스

## 참고
[room-codelabs](https://codelabs.developers.google.com/codelabs/android-persistence/#0)
