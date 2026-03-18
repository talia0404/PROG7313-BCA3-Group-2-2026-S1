# 🗄️ Introduction to Room Database in Jetpack Compose

Most mobile apps need to **store data locally** on the device. For example:

* saving notes 📝
* storing user preferences ⚙️
* keeping track of expenses 💰
* storing offline data 📶

Android apps store structured data using **SQLite**, which is a built-in database system on Android devices.

However, writing raw SQLite queries and managing database connections can become complicated. To make this easier, Android provides **Room Database (RoomDB)**.

Room is part of **Android Jetpack** and acts as a **layer on top of SQLite** that makes working with databases safer, easier, and more structured.

Room works very well with **Jetpack Compose** because it integrates smoothly with Kotlin, coroutines, Flow, and modern Android architecture.

---

# 🧠 What RoomDB Does

Room provides a structured way to store and retrieve data by using three main components:

| Component                    | Purpose                          |
| ---------------------------- | -------------------------------- |
| **Entity**                   | Defines the database table       |
| **DAO (Data Access Object)** | Defines database queries         |
| **Database Class**           | Connects the database to the app |

Think of it like this:

```text
Entity → table structure
DAO → queries
Database → database connection
```

Room generates most of the database code automatically, which reduces bugs and improves performance.

---

# 📦 Step 1: Add Room Dependencies

To use Room in a **Jetpack Compose** project, you must add the required dependencies.

## In your app-level `build.gradle.kts`

```kotlin
plugins {
    id("com.google.devtools.ksp") version "2.3.4"
}
```

## Add these Room dependencies

```kotlin
implementation("androidx.room:room-runtime:2.6.1")
implementation("androidx.room:room-ktx:2.6.1")
ksp("androidx.room:room-compiler:2.6.1")
```

Explanation:

* `room-runtime` provides the core Room functionality
* `room-ktx` adds Kotlin coroutine and Flow support
* `room-compiler` generates Room’s database code automatically
* `ksp(...)` is used instead of `kapt(...)` in modern projects

If you are using **version catalogues** or a project-level plugin setup, the exact syntax may look slightly different, but the idea stays the same.

---

# 🧱 Step 2: Create an Entity (Database Table)

An **Entity** represents a table inside the database.

Example: a simple table that stores notes.

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

Instead of writing SQL manually throughout the app, we define queries inside a DAO.

Example:

```kotlin
import androidx.room.Dao
import androidx.room.Delete
import androidx.room.Insert
import androidx.room.Query
import kotlinx.coroutines.flow.Flow

@Dao
interface NoteDao {

    @Insert
    suspend fun insertNote(note: Note)

    @Delete
    suspend fun deleteNote(note: Note)

    @Query("SELECT * FROM notes ORDER BY id DESC")
    fun getAllNotes(): Flow<List<Note>>

}
```

Explanation:

* `@Dao` marks this interface as a data access object
* `@Insert` inserts data into the table
* `@Delete` removes data
* `@Query` allows custom SQL queries
* `Flow<List<Note>>` allows Compose to observe database changes automatically

Room generates the implementation behind the scenes, which is very neat and mildly suspicious the first time you see it.

---

# 🗄️ Step 4: Create the Database Class

The **database class** connects everything together.

```kotlin
import androidx.room.Database
import androidx.room.RoomDatabase

@Database(
    entities = [Note::class],
    version = 1,
    exportSchema = false
)
abstract class AppDatabase : RoomDatabase() {

    abstract fun noteDao(): NoteDao

}
```

Explanation:

* `@Database` tells Room which entities belong in the database
* `version` tracks database schema changes
* `exportSchema = false` disables schema export for simple projects
* `RoomDatabase` is the base class for Room databases
* `noteDao()` gives access to database queries

---

# 🔌 Step 5: Create the Database Instance

Before using the database, you must create an instance of it.

Example:

```kotlin
import android.content.Context
import androidx.room.Room

fun provideDatabase(context: Context): AppDatabase {
    return Room.databaseBuilder(
        context,
        AppDatabase::class.java,
        "notes_database"
    ).build()
}
```

Then you can access the DAO like this:

```kotlin
val database = provideDatabase(context)
val noteDao = database.noteDao()
```

Explanation:

* `databaseBuilder()` creates the database
* `"notes_database"` is the database file name
* `noteDao()` gives access to DAO functions

In a real Compose app, you would usually create this database once, not every five minutes like a goblin with a keyboard.

---

# 🔄 Room + Jetpack Compose

Room works very well with Compose because it can provide **reactive data streams** using `Flow`.

Example DAO query:

```kotlin
@Query("SELECT * FROM notes ORDER BY id DESC")
fun getAllNotes(): Flow<List<Note>>
```

Compose can observe this data and automatically update the UI when the database changes.

This is powerful because:

* the database updates
* the UI refreshes automatically
* no manual refresh logic is required

This reactive pattern is a big part of modern Android development.

---

# 🧩 Using Room Data in a Compose Screen

In Jetpack Compose, Room data is usually collected inside a **ViewModel**, then observed by the UI.

Example idea:

```kotlin
val notes by viewModel.allNotes.collectAsState(initial = emptyList())
```

This allows your composable screen to automatically redraw when the database content changes.

That means:

* add a note → UI updates
* delete a note → UI updates
* change database content → UI updates

Compose and Room work together very nicely because both support modern reactive programming patterns.

---

# 📊 Room Architecture Overview

A typical Room structure in a Compose project might look like this:

```text
data/
   local/
      Note.kt
      NoteDao.kt
      AppDatabase.kt
```

Or a more layered structure:

```text
data/
   entity/
      Note.kt
   dao/
      NoteDao.kt
   database/
      AppDatabase.kt
```

Both are fine. The goal is simply to keep your project organized and easy to maintain.

---

# ✅ Why Room Is Useful in Jetpack Compose

Room is widely used in Android development because it:

* simplifies working with SQLite
* reduces boilerplate code
* provides compile-time query checking
* integrates well with Kotlin coroutines
* supports Flow for reactive updates
* works smoothly with Jetpack Compose

In modern Android development, Room is commonly used together with:

* **ViewModel**
* **Repository pattern**
* **Flow**
* **Jetpack Compose UI**

These tools form the foundation of scalable Android applications.

---

# 🏁 Summary

Room Database provides a clean way to manage local data in Android apps.

The three key parts are:

* **Entity** → defines tables
* **DAO** → defines queries
* **Database** → connects everything

When combined with **Jetpack Compose**, Room allows developers to build apps where **data changes automatically update the UI**, making apps more responsive and easier to maintain.

For a modern project setup, make sure you are using:

* **Jetpack Compose**
* **Room**
* **Kotlin coroutines and Flow**
* **KSP instead of kapt**

That combo is the current Android dev starter pack. Tiny little toolbox of chaos, but in a good way.

---
