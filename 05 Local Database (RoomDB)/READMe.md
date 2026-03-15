
# 🗄️ Introduction to Room Database in Jetpack Compose

Most mobile apps need to **store data locally** on the device. For example:

- saving notes 📝  
- storing user preferences ⚙️  
- keeping track of expenses 💰  
- storing offline data 📶  

Android apps store structured data using **SQLite**, which is a built-in database system on Android devices.

However, writing raw SQLite queries and managing database connections can become complicated. To make this easier, Android provides **Room Database (RoomDB)**.

Room is part of **Android Jetpack** and acts as a **layer on top of SQLite** that makes working with databases safer, easier, and more structured.

Room works very well with **Jetpack Compose** because it integrates smoothly with Kotlin, coroutines, and modern Android architecture.

---

# 🧠 What RoomDB Does

Room provides a structured way to store and retrieve data by using three main components:

| Component | Purpose |
|---|---|
| **Entity** | Defines the database table |
| **DAO (Data Access Object)** | Defines database queries |
| **Database Class** | Connects the database to the app |

Think of it like this:

```

Entity → table structure
DAO → queries
Database → database connection

````

Room generates most of the database code automatically, which reduces bugs and improves performance.

---

# 📦 Step 1: Add Room Dependencies

To use Room, you must add the required dependencies to your project.

Example `build.gradle` dependencies:

```kotlin
implementation("androidx.room:room-runtime:2.6.1")
kapt("androidx.room:room-compiler:2.6.1")
implementation("androidx.room:room-ktx:2.6.1")
````

These libraries allow your project to use Room’s database features.

---

# 🧱 Step 2: Create an Entity (Database Table)

An **Entity** represents a table inside the database.

Example: A simple table that stores notes.

```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "notes")
data class Note(

    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,

    val title: String,

    val content: String

)
```

Explanation:

* `@Entity` tells Room this class represents a database table
* `tableName` sets the name of the table
* `@PrimaryKey` identifies the unique column
* `autoGenerate = true` automatically creates IDs

This creates a table similar to:

| id | title    | content           |
| -- | -------- | ----------------- |
| 1  | Shopping | Buy milk          |
| 2  | Homework | Finish assignment |

---

# 📂 Step 3: Create a DAO (Database Queries)

The **DAO (Data Access Object)** defines how the app interacts with the database.

Instead of writing SQL manually everywhere, we define queries inside a DAO.

Example:

```kotlin
import androidx.room.Dao
import androidx.room.Insert
import androidx.room.Query
import androidx.room.Delete

@Dao
interface NoteDao {

    @Insert
    suspend fun insertNote(note: Note)

    @Delete
    suspend fun deleteNote(note: Note)

    @Query("SELECT * FROM notes")
    suspend fun getAllNotes(): List<Note>

}
```

Explanation:

* `@Dao` marks this interface as a data access object
* `@Insert` inserts data into the table
* `@Delete` removes data
* `@Query` allows custom SQL queries

Room automatically generates the implementation behind the scenes.

---

# 🗄️ Step 4: Create the Database Class

The **database class** connects everything together.

```kotlin
import androidx.room.Database
import androidx.room.RoomDatabase

@Database(
    entities = [Note::class],
    version = 1
)
abstract class AppDatabase : RoomDatabase() {

    abstract fun noteDao(): NoteDao

}
```

Explanation:

* `@Database` tells Room which entities belong in the database
* `version` tracks database schema changes
* `RoomDatabase` is the base class for Room databases
* `noteDao()` gives access to database queries

---

# 🔌 Step 5: Creating the Database Instance

Before using the database, we must create an instance of it.

Example:

```kotlin
import androidx.room.Room

val database = Room.databaseBuilder(
    context,
    AppDatabase::class.java,
    "notes_database"
).build()

val noteDao = database.noteDao()
```

Explanation:

* `databaseBuilder()` creates the database
* `"notes_database"` is the database file name
* `noteDao()` gives access to DAO functions

---

# 🔄 Room + Jetpack Compose

Room works very well with Compose because it can provide **reactive data streams** using `Flow`.

Example DAO query:

```kotlin
@Query("SELECT * FROM notes")
fun getAllNotes(): Flow<List<Note>>
```

Compose can observe this data and automatically update the UI when the database changes.

This is powerful because:

* the database updates
* the UI refreshes automatically
* no manual refresh logic is required

Modern Android apps rely heavily on this **reactive data pattern**.

---

# 📊 Room Architecture Overview

Typical Room structure in a Compose project:

```
data/
   entity/
       Note.kt

   dao/
       NoteDao.kt

   database/
       AppDatabase.kt
```

This keeps database code organized and maintainable.

---

Room is widely used in Android development because it:

* simplifies working with SQLite
* reduces boilerplate code
* provides compile-time query checking
* integrates well with Kotlin and coroutines
* works smoothly with Jetpack Compose

In modern Android development, Room is commonly used together with:

* **ViewModel**
* **Repository pattern**
* **Flow / LiveData**
* **Jetpack Compose UI**

These tools form the foundation of scalable Android applications.

Room Database provides a clean way to manage local data in Android apps.

The three key parts are:

* **Entity** → defines tables
* **DAO** → defines queries
* **Database** → connects everything

When combined with **Jetpack Compose**, Room allows developers to build apps where **data changes automatically update the UI**, making apps more responsive and easier to maintain.
