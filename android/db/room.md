
entity > dao > database

# Room Persistence Library
## gradle setup
room 을 사용하기 위해 다음의 dependencies 추가한다
```groovy
// For Room, add:
compile "android.arch.persistence.room:runtime:1.0.0-alpha9"
annotationProcessor "android.arch.persistence.room:compiler:1.0.0-alpha9"
// For testing Room migrations, add:
testCompile "android.arch.persistence.room:testing:1.0.0-alpha9"
// For Room RxJava support, add:
compile "android.arch.persistence.room:rxjava2:1.0.0-alpha9"
```
## Database
Roomdatabase 를 상속받아 사용하고, @database annotation 을 사용해서 entities와 version 정의한다.
예를 들어 User,Book,Load을 사용하는 데이터베이스 클래스는 다음과 같이 작성할 수 있다.
```java
@Database(entities = {User.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract UserDao userDao();
}
```
database 클래스에 dao 클래스들을 정의한다
```java
public abstract UserDao userModel();
public abstract BookDao bookModel();
public abstract LoanDao loanModel();
```
database 생성 호출
```java
Room.inMemoryDatabaseBuilder(context.getApplicationContext(), AppDatabase.class)
                    // To simplify the codelab, allow queries on the main thread.
                    // Don't do this on a real app! See PersistenceBasicSample for an example.
                    .allowMainThreadQueries()
                    .build();
```
### builder
- inMemoryDatabaseBuilder
- databaseBuilder

## Entity
This component represents a class that holds a database row. (Table)
```java
@Entity
public class User {
    @PrimaryKey
    private int uid;

    @ColumnInfo(name = "first_name")
    private String firstName;

    @ColumnInfo(name = "last_name")
    private String lastName;

    // Getters and setters are ignored for brevity,
    // but they're required for Room to work.
}
```
## DAO
data 에 접근하기 위한 객체, query를 정의한다 interface 클래스로 database 클래스의 멤버변수로 호출해서 실행한다
간단한 select구문을 사용하는 query는 다음과 같이 작성할 수 있다.
```java
@Dao
public interface UserDao {
    @Query("SELECT * FROM user")
    List<User> getAll();

    @Query("SELECT * FROM user WHERE uid IN (:userIds)")
    List<User> loadAllByIds(int[] userIds);

    @Query("SELECT * FROM user WHERE first_name LIKE :first AND "
           + "last_name LIKE :last LIMIT 1")
    User findByName(String first, String last);

    @Insert
    void insertAll(User... users);

    @Delete
    void delete(User user);
}
```

## DatabaseInitializer
정의한 DAO 클래스를 실행하는 클래스

# Codelab
## Observe LiveData from a ViewModel
@Query 의 return type 에 LiveData를 wrapping 해서 쉽게 database observer를 사용할 수 있다.
```java
@Dao
public interface BookDao {
      @Query("SELECT * FROM Book " +
              "INNER JOIN Loan ON Loan.book_id = Book.id " +
              "INNER JOIN User on User.id = Loan.user_id " +
              "WHERE User.name LIKE :userName"
      )
      public LiveData<List<Book>> findBooksBorrowedByName(String userName);  
}
```
ViewModel 에서 위의 쿼리를 호출한다
```java
public class BooksBorrowedByUserViewModel extends AndroidViewModel {
  public final LiveData<List<Book>> books;
  private AppDatabase mDb;
  public BooksBorrowedByUserViewModel(Application application) {
        super(application);
        ...
        books = mDb.bookModel().findBooksBorrowedByName("Mike");
        ...
    }
}
```
가져온 LiveData 의 observe 함수를 정의한다.
```java
private void subscribeUiBooks() {
         mViewModel.books.observe(this, new Observer<List<Book>>() {
             @Override
             public void onChanged(@Nullable List<Book> books) {
                 showBooksInUi(books, mBooksTextView);
             }
         });
    }
```
Database의 변화가 생길 때마다 view가 갱신된다.

## Using Custom Type Converters
date 클래스의 경우, data > long, long > date 변환이 필요한 듯,
java에서 쓸 때는 date로, sql에서는 long으로 사용
```java
public class DateConverter {
    @TypeConverter
    public static Date toDate(Long timestamp) {
        return timestamp == null ? null : new Date(timestamp);
    }

    @TypeConverter
    public static Long toTimestamp(Date date) {
        return date == null ? null : date.getTime();
    }
}
```

## Add Custom Query Result Objects
Database에서 정의된 Entity 객체말고 쿼리에서 별도의 결과를 객체에 리턴받을 수 있다.
다음과 같은 custom query를 작성했다고 할 때,
```java
// custom query
public interface LoanDao {
    //...
    @Query("SELECT Loan.id, Book.title, User.name, Loan.startTime, Loan.endTime From Loan " +
        "INNER JOIN Book ON Loan.book_id = Book.id " +
        "INNER JOIN User ON Loan.user_id = User.id// ")
    LiveData<List<LoanWithUserAndBook>> findAllWithUserAndBook();
    //...
}
```
위의 결과를 받기위해 아래의 객체를 정의해서 사용할 수 있다.  
```java
// custom query 의 결과를 받을 객체
public class LoanWithUserAndBook {
    public String id;
    @ColumnInfo(name="title")
    public String bookTitle;
    @ColumnInfo(name="name")
    public String userName;
    @TypeConverters(DateConverter.class)
    public Date startTime;
    @TypeConverters(DateConverter.class)
    public Date endTime;
}
```
Transformations.map 을 사용해서 출력되는 형태를 변형할 수 있다.
```java
mLoansResult = Transformations.map(loans,
               new Function<List<LoanWithUserAndBook>, String>() {
                   @Override
                   public String apply(List<LoanWithUserAndBook> loansWithUserAndBook) {
                       StringBuilder sb = new StringBuilder();
                       SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm", Locale.US);

                       for (LoanWithUserAndBook loan : loansWithUserAndBook) {
                           sb.append(String.format("%s\n  (Returned: %s)\n",
                                   loan.bookTitle,
                                   simpleDateFormat.format(loan.endTime)));
                       }
                       return sb.toString();
                }
           });
```  


## 참고
[room-codelabs](https://codelabs.developers.google.com/codelabs/android-persistence/#0)
[reference](https://developer.android.com/topic/libraries/architecture/room.html)
[Room — Entity Annotations](https://medium.com/@tonyowen/room-entity-annotations-379150e1ca82)
