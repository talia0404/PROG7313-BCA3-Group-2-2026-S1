## Step 2 - Adding RoomDB (Local Database)

Now that your **Register and Login screens work**, the next step is to give your app a **memory**.

Right now, when someone registers, nothing is actually saved anywhere. If the app closes, the account disappears into the void like a poker chip falling off a table.

To fix this, you will add **RoomDB**, which is Android’s official library for storing structured data locally on the device.

Room acts as a **layer on top of SQLite**. Instead of writing raw SQL everywhere, you work with **Kotlin objects**, and Room translates them into database operations.

In this step you will connect your authentication system to a real database so that:

- registration saves a user
- login checks the saved users
- the app can remember accounts

---

# 🧠 What RoomDB Actually Is

Room is made of **three main pieces**. Think of them as parts of a small database machine.

### 🧾 Entity

An **Entity** represents a **table** in the database.

For example:

User table

| id | fullName | email | username | password |

Each row in the table represents one registered user.

---

### 📬 DAO (Data Access Object)

The DAO contains the **queries and database operations**.

Examples of operations:

- insert a user
- find a user by username
- check login credentials

The DAO is basically the **bridge between your app and the database**.

---

### 🗄 Database

The Database class connects everything together.

It:

- defines which entities exist
- provides access to the DAOs
- builds the database instance

---

# 🎯 By the end of this step your app will:

- store registered users locally
- prevent duplicate usernames
- verify login credentials against the database
- display login success or failure based on real stored data

This is the first time your app will behave like a **real application instead of a demo UI**.

---

# 🪜 Step 1 - Add the Room library

Before you can use Room, your project must include the **Room dependencies**.

Open your **app-level Gradle file**.

You need to add the Room libraries that allow:

- database runtime
- Kotlin extensions
- the Room compiler

The compiler is extremely important. It generates the database code automatically.

Without it, Room cannot build your database classes.

After adding the dependencies, **sync your project**.

Android Studio will download the required libraries.

---

# 🪜 Step 2 - Create a data package

To keep your project organised, create a new package called:

```
data
```

Inside this package you will store all database-related files.

Clean architecture matters, even in student projects. Chaos multiplies quickly in mobile apps.

Your package structure should begin to look something like this:

```
com.example.betwise
│
├── data
├── viewmodel
├── ui
├── navigation
```

The **data package** will hold the database components.

---

# 🪜 Step 3 - Create the User Entity

Inside the **data package**, create a class that represents the **User table**.

This class describes what a registered user looks like.

The entity should contain the following properties:

- id
- full name
- email
- username
- password

The **id field** should be the **primary key** and should automatically generate values.

This means every user will receive a unique ID when inserted into the database.

The **username should be unique**, because two accounts should not share the same username.

This prevents duplicate accounts.

Once this entity exists, Room understands what the **users table** should look like.

---

# 🪜 Step 4 - Create the User DAO

Next, create a **DAO interface** inside the data package.

The DAO defines how the app interacts with the database.

Your DAO should include operations for:

### - Inserting a new user

Used when someone registers.

---

### - Finding a user by username

Used to check if the username already exists.

This prevents duplicate registrations.

---

### - Checking login credentials

Used during login to verify that:

- the username exists
- the password matches

If both match, login is successful.

If not, login should fail.

---

# 🪜 Step 5 - Create the Room Database class

Next, create the main **database class**.

This class represents the **entire database** used by the app.

It should:

- declare the entity classes used by the database
- provide access to the DAO
- define the database version

The database class should also include a **singleton instance**.

A singleton ensures that:

- only one database instance exists
- the app does not accidentally create multiple database connections

Multiple database instances can cause crashes or corrupted data.

---

# 🪜 Step 6 - Connect the database to a ViewModel

You should **not access the database directly from the UI**.

Instead, create a **ViewModel** that handles authentication logic.

Create a new package:

```
viewmodel
```

Inside this package create an **authentication ViewModel**.

This ViewModel will:

- communicate with the DAO
- perform database queries
- expose messages to the UI
- manage login and registration logic

This keeps your architecture clean.

The UI should only display data, not perform database operations.

---

# 🪜 Step 7 - Implement registration logic

Inside your ViewModel, implement the registration process.

The process should follow this order:

1️⃣ Check that all fields are completed
2️⃣ Validate the email format
3️⃣ Ensure the password length is sufficient
4️⃣ Confirm that passwords match
5️⃣ Check if the username already exists in the database

If the username already exists, display a message telling the user to choose a different one.

If everything is valid, create a **new user object** and insert it into the database.

Once the user is saved, display a message confirming successful registration.

---

# 🪜 Step 8 - Implement login logic

Next, implement login validation in the ViewModel.

When the login button is pressed:

1️⃣ Check that username and password fields are not empty
2️⃣ Query the database for a matching username and password

If a matching user exists:

- login is successful

If no match exists:

- show an error message such as “Invalid username or password”.

This ensures the login screen checks **real stored data instead of fake validation**.

---

# 🪜 Step 9 - Connect the ViewModel to the UI

Your Login and Register screens must now communicate with the ViewModel.

Instead of performing validation directly inside the composables, the UI should call ViewModel functions.

The ViewModel will then:

- perform validation
- access the database
- return messages to display on the screen

This pattern is known as **MVVM (Model View ViewModel)** and is widely used in Android development.

It keeps business logic separate from the UI.

---

# 🧪 Step 10 - Test everything

Once the database is connected, test several scenarios.

### Registration tests

Try registering with:

- empty fields
- invalid email
- short password
- mismatched passwords
- a username that already exists

Confirm that appropriate messages appear.

---

### Login tests

Try logging in with:

- an incorrect username
- an incorrect password
- correct credentials

Make sure login only succeeds when the credentials match a stored user.

---

# 📚 Helpful resources

Room database guide
[https://developer.android.com/training/data-storage/room](https://developer.android.com/training/data-storage/room)

Room entities
[https://developer.android.com/reference/androidx/room/Entity](https://developer.android.com/reference/androidx/room/Entity)

Room DAO guide
[https://developer.android.com/reference/androidx/room/Dao](https://developer.android.com/reference/androidx/room/Dao)


---

