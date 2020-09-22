---
title: "안드로이드 Room 데이터베이스"
tags:
  - android
---

[Room 공식 문서](https://developer.android.com/training/data-storage/room)를 보면 상당히 잘 정리되어 있어서,
그대로 따라하면 웬만하면 사용하는데 문제는 없을 것이다.
유사하지만, 자그만 팁들과 함께 짧게 여기에도 정리해본다.

## 1. Entity 정의하기

```kotlin
@Entity(tableName = TABLE_NAME)
data class User(
  @PrimaryKey val id: Int,
  @ColumnInfo(name = COLUMN_FIRST_NAME) val firstName: String?
) {
  companion object {
    const val TABLE_NAME = "users"
    const val COLUMN_FIRST_NAME = "first_name"
  }
}
```

## 2. DAO 정의하기

```kotlin
@Dao
interface UserDao {
  @Query("SELECT * FROM ${User.TABLE_NAME}")
  fun getAll(): List<User>

  @Query("SELECT * FROM ${User.TABLE_NAME} WHERE id IN (:userIds)")
  fun getAllByIds(userIds: IntArray): List<User>

  @Insert
  fun insertAll(vararg users: User)

  @Delete
  fun delete(user: User)
}
```

## 3. Database 정의하기

```kotlin
@Database(entities = arrayOf(User::class), version = DATABASE_VERSION)
abstract class UserDatabase : RoomDatabase() {
  companion object {
    const val DATABASE_NAME = "db_user"
    const val DATABASE_VERSION = 1
  }
  
  abstract fun userDao(): UserDao
}
```

## 4. Database 사용하기

**`UserDatabase`의 구현체는 Room에서 생성**해준다. 이를 사용하기 위해서는:

```kotlin
val dao = Room.databaseBuilder(context, UserDatabase::class.java, UserDatabase.DATABASE_NAME)
  .build()
  .userDao()
  
// 반환된 DAO를 사용하여 데이터베이스를 이용한다.
dao.delete(...)
```

공식 문서에는 중간중간 다른 설명들이 있어서 언뜻 보면 복잡해 보일수도 있지만,
**보다시피 아주 간단한 4단계로 사용이 가능**하다!

마지막으로, 강조하고 싶은 주의 사항들이 있다.

## 주의 사항

1. 모든 Room의 Entity에는 **적어도 1개 이상의 PrimaryKey가 필수적**이다. 공식 문서에도 나와있지만, 충분히 강조되어 있지 않아 놓치기 쉽다.
2. DAO의 함수들은 **기본적으로 synchronous**하게 동작하지만, Single, LiveData, suspend 등의 키워드를 사용할 시에는 **asynchronous**하게 동작한다.
3. **suspend와 LiveData를 같이 사용하면 빌드가 안된다** (Single이나 Flow 등도 마찬가지일것이다). ([참고](https://stackoverflow.com/questions/54566663/room-dao-livedata-as-return-type-causing-compile-time-error/55913412#55913412))
4. @PrimaryKey(autoIncrement=true)를 사용할 시, 해당 파라미터는 **0으로 넣어야 자동 생성**이 된다. 1이나 2 같은 명시적 값을 주면 자동 생성이 아닌 해당 값으로 들어간다.
5. DAO는 interface나 abstract class로 선언이 가능하므로, **구현이 들어간 함수가 필요할 경우에는 abstract class**로 정의해서 사용하면 편하다.
6. Flow 혹은 Observable 같은 리턴 타입을 사용할 시, **쿼리문에서 참조하는 모든 테이들에 대한 옵저버가 등록되어, 해당 테이블 중 아무 테이블에나 변화가 생길 때 마다** 새로운 값을 가져온다.
따라서, 앱 로직에서 "정말 데이터가 바뀌었을 때"만 확인하고 싶다면 Flow나 Observable에서 제공하는 `distinctUntilChanged()` 함수를 사용하면 된다.