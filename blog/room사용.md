Room 사용 따라하기
# Entity 작성
tableName, columninfo, primarykey
```kotlin
@Entity(tableName = "users")
data class User(@PrimaryKey
                @ColumnInfo(name = "userId")
                val id:String = UUID.randomUUID().toString(),
                @ColumnInfo(name = "username")
                val userName:String)
```

# DAO 작성
query 작성
```kotlin
@Dao
interface UserDao {
    @Insert(onConflict = OnConflictStrategy.REPLACE)
    fun insertUser(user: User)

    @Query("SELECT * FROM users where userId = :id")
    fun getUserById(id: String): Flowable<User>

    @Query("DELETE FROM users")
    fun deleteAllUsers()
}
```

# Database 작성
entity, dao 정의
```kotlin
@Database(entities = arrayOf(User::class), version = 1)
abstract class UsersDatabase : RoomDatabase() {
    abstract fun userDao(): UserDao
}
```

# room init
```kotlin
Room.databaseBuilder(context.applicationContext,
                        UsersDatabase::class.java, "Sample.db")
                        .build()
```
