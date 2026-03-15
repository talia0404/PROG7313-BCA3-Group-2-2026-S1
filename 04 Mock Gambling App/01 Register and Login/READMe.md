# 🎯 What you should achieve in this step

By the end of this task, your app should allow someone to:

• open the app
• see the **login screen first**
• navigate to the **register screen**
• enter details to create an account
• return to the login screen
• log in using the created credentials (demo logic for now)

At this stage the app is **not yet connected to a database**. That will happen later when you learn **RoomDB and Firebase**.

For now, you are building the **structure and validation logic**.

---

# 🧠 Understand how the app switches screens

Your app uses a modern **Jetpack Compose pattern** where screens are switched using state instead of launching new activities.

This happens in the composable called:

`BetWiseAuthApp()`

Inside this function a variable stores which screen should be displayed.

The possible values are defined in the enum:

```
AuthScreen.LOGIN
AuthScreen.REGISTER
```

When the value changes, Compose **recomposes the UI** and displays a different screen.

This means the app does **not open another Activity**.
Instead, it simply **changes which composable is visible**.

This is one of the key ideas behind **Jetpack Compose UI architecture**.

---

# ▶️ Step 1 - Run the app and test navigation

Before writing any logic, run the app.

Check that the following behaviour works:

1️⃣ The app opens on the **Login screen**

2️⃣ When you tap:

**“Don’t have an account? Register”**

The screen changes to the **Register screen**

3️⃣ When you tap:

**“Already have an account? Log In”**

The screen returns to **Login**

If this navigation does not work, fix it before continuing.

Never build more logic on top of a broken navigation flow.

---

# 🧾 Step 2 - Review the fields in the Register screen

Your register screen should collect the following information:

• Full Name
• Email
• Username
• Password
• Confirm Password

Each field should be clear and easy to understand.

Make sure every field includes:

• a clear **label**
• a helpful **placeholder**
• correct **keyboard input type** where necessary
• logical spacing between fields

A good form feels like a natural flow.

Example logical order:

1️⃣ Full name
2️⃣ Email
3️⃣ Username
4️⃣ Password
5️⃣ Confirm password

This makes the form easier to complete.

---

# 🔐 Step 3 - Add registration validation

When the **Register button** is pressed, the app must validate the input.

Your app should check the following rules:

### All fields must be completed

If any field is empty, show a message telling the user to complete all fields.

---

### Email must look like a real email

A basic check should confirm that the email contains:

• `@`
• `.`

This is not perfect email validation, but it is acceptable for a beginner project.

---

### Password length requirement

Your password should be at least **6 characters long**.

This prevents extremely weak passwords.

---

### Passwords must match

The password and confirm password fields must contain **exactly the same value**.

If they do not match, display a clear error message.

---

### Successful registration

If everything is valid, display a success message such as:

“Registration successful”

In a real application this is where the app would **save the user to a database**, but that will happen later.

---

# 🔑 Step 4 - Implement login validation

The login screen should contain two fields:

• Username
• Password

When the **Log In button** is pressed, the app must check:

1️⃣ Both fields are filled in
2️⃣ Display an error if either field is empty

For now, you can simulate a successful login if both fields contain values.

Later in the project, login will be connected to:

• **RoomDB**
or
• **Firebase Authentication**

so the app can verify real stored users.

---

# 👁 Step 5 - Password visibility toggle

Your password fields include a **Show / Hide toggle**.

This improves usability by allowing the user to check their password if they typed it incorrectly.

Make sure that:

• password text is hidden by default
• tapping **Show** reveals the password
• tapping **Hide** hides it again

This behaviour should work on:

• Login password field
• Register password field
• Confirm password field

---

# 🧱 Step 6 - Keep the UI reusable

Your app uses reusable UI components such as:

• `AuthCardContainer()`
• `AppTextField()`
• `PrimaryAppButton()`

These reusable components make your UI:

• cleaner
• easier to maintain
• consistent across screens

Avoid duplicating UI code when a reusable component already exists.

Professional apps reuse UI components heavily.

---

# 🧪 Step 7 - Test your flow carefully

Test the following scenarios:

### Register tests

Try registering with:

• empty fields
• invalid email
• password shorter than 6 characters
• mismatched passwords
• valid details

Confirm that the correct message appears each time.

---

### Login tests

Try logging in with:

• empty username
• empty password
• both fields filled

Make sure appropriate messages appear.

---

# 🧭 What comes next

Once register and login are working properly, the next step in the project will be:

### 🗂 Connecting authentication to storage

You will learn how to store users using:

• **RoomDB** (local database)

and optionally later

• **Firebase Authentication**

After that, you will begin building the core BetWise features such as:

• casino categories
• session tracking
• chip goals
• dashboards and graphs
• gamification badges

But none of that matters if the **authentication flow is broken**, so this step is extremely important.

---

# 📚 Helpful Resources

Jetpack Compose documentation
[https://developer.android.com/compose](https://developer.android.com/compose)

Compose navigation basics
[https://developer.android.com/jetpack/compose/navigation](https://developer.android.com/jetpack/compose/navigation)



