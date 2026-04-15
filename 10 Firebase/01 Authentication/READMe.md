# Implementing Firebase Auth with Jetpack Compose

## 1. What you are building

Your app will do three main things:

* show a login and registration interface in **Jetpack Compose**
* send authentication requests to **Firebase Authentication**
* update the screen based on whether the user is logged in or not

In this version, `MainActivity` will host the Compose UI, and `AuthViewModel` will handle the authentication logic and screen state. Android guidance recommends using a `ViewModel` as the screen-level state holder for Compose, with the activity mainly hosting composables rather than carrying the business logic itself. ([Android Developers][1])

---

# 2. Create and configure your Firebase project

Before Android Studio can talk to Firebase, you need to connect your app to a Firebase project.

### What to do

Go to the Firebase Console and:

* create a new Firebase project
* add an **Android app** to that project
* enter your app’s **package name** exactly as it appears in Android Studio
* download the `google-services.json` file
* place that file inside your app-level module folder, usually the `app` folder

Firebase’s Android setup requires registering the Android app in the console and adding the `google-services.json` file to your app module so the project is configured correctly. ([Firebase][2])

### Why this matters

That JSON file connects your Android app to the correct Firebase project. Without it, your app will basically be trying to send the wrong number to Firebase.

---

# 3. Add Firebase to your Android project

Next, set up your Gradle files so the project knows how to use Firebase.

### What needs to happen

You need to:

* add the **Google Services plugin**
* add the **Firebase BoM**
* add the **Firebase Authentication dependency**

Firebase recommends using the **Firebase BoM** so that Firebase library versions stay aligned with each other. ([Firebase][2])

### Why this matters

If dependencies are added incorrectly, the project may sync badly, authentication classes may not resolve, or the app may build but fail later. The BoM helps prevent version mismatches.

---

# 4. Enable the sign-in method in Firebase Console

Adding the SDK is not enough. Firebase Auth also needs to know which type of login you want to allow.

### For email and password auth

In Firebase Console:

* open **Authentication**
* go to **Sign-in method**
* enable **Email/Password**
* save the change

Firebase’s Android auth setup explicitly requires enabling the provider in the Firebase Authentication section before those sign-in methods will work. ([Firebase][3])

### Why this matters

If you skip this step, your app can call Firebase Auth methods correctly, but sign-in or registration will fail because that provider is disabled in the project.

---

# 5. Decide the responsibility of each file

Since you only want to use `MainActivity` and `AuthViewModel`, be very clear about what each one is responsible for.

## `MainActivity`

This file should:

* start the app
* load the Compose UI
* display different UI states based on the data from the ViewModel
* send user actions to the ViewModel

It should **not** contain the real Firebase authentication logic.

## `AuthViewModel`

This file should:

* hold the auth screen state
* store email and password input values
* handle loading state
* handle error messages
* call Firebase Authentication to register, log in, sign out, and check current user status

This matches Android’s architecture guidance: the ViewModel should expose UI state and receive events from the UI, while the activity mainly hosts the UI. ([Android Developers][1])

---

# 6. Plan your screen states before building the UI

Before designing the screen, decide what the UI needs to know at all times.

Your auth screen usually needs these pieces of state:

* current email text
* current password text
* whether the app is loading
* whether the user is logged in
* any error message to show
* whether the user is on the login mode or register mode

### Why this matters

Compose works best when the UI simply reads state and redraws itself when that state changes. Compose documentation recommends hoisting state and exposing immutable state to the UI, especially when business logic is involved. ([Android Developers][4])

So instead of making the screen “figure things out” on its own, the ViewModel should provide the truth, and the UI should reflect it.

---

# 7. Create the UI state inside `AuthViewModel`

Inside the ViewModel, define a single source of truth for the screen.

### Your state should represent:

* what the user typed
* what screen mode they are in
* whether an auth request is currently running
* whether the request succeeded
* whether an error happened

### Important mindset

Do not think of Compose as “telling widgets what to do” like old-school XML + Activity logic.

Think of it like this:

* the ViewModel updates state
* Compose reads that state
* the screen changes automatically

That state-driven flow is central to how Compose is designed to work. ([Android Developers][4])

---

# 8. Initialise Firebase Auth inside the ViewModel

Since you are not using a repository in this simplified version, the ViewModel will talk directly to Firebase Authentication.

### What to do conceptually

Inside `AuthViewModel`, create access to the Firebase Auth instance and use it whenever you need to:

* register a new user
* log in an existing user
* sign out
* check if a user is already authenticated

Firebase Authentication on Android supports creating a user with email/password and signing in with email/password directly from the client SDK. ([Firebase][3])

### Why this matters

This is the actual bridge between your app and Firebase. Everything in the UI is just the front desk. Firebase Auth is the person behind the office door doing the actual paperwork.

---

# 9. Add the authentication actions to the ViewModel

Your ViewModel should expose clear functions for the UI to call.

You should create actions conceptually like:

* change email
* change password
* switch between login and register mode
* register
* login
* logout
* clear error

### What each auth action should do

## Register action

When the user taps register:

1. mark the screen as loading
2. clear any old error
3. read the current email and password from state
4. send them to Firebase Auth
5. if successful, update the state to logged in
6. if failed, stop loading and show the error

## Login action

Very similar process:

1. set loading
2. clear old error
3. send credentials to Firebase Auth
4. if successful, mark user as logged in
5. if failed, show an error message

## Logout action

When the user taps logout:

1. call Firebase sign-out
2. update state so the app returns to the logged-out screen

Firebase provides methods for creating accounts, signing users in, and managing the signed-in user session. ([Firebase][3])

---

# 10. Decide how `MainActivity` should display the screen

Since you only want `MainActivity`, this file will both host your UI and decide what to show based on ViewModel state.

### Basic screen flow

`MainActivity` should:

* get access to the `AuthViewModel`
* observe the ViewModel’s UI state
* show one of two broad UI experiences:

  * logged-out auth form
  * logged-in success/home state

### Logged-out state

Show:

* email input
* password input
* button for login
* button for register or a toggle between login/register mode
* error message if one exists
* loading indicator when needed

### Logged-in state

Show:

* confirmation that the user is signed in
* optional display of the user email
* logout button

This follows Compose’s recommended pattern of displaying UI conditionally from state rather than manually manipulating view visibility like older Android approaches. ([Android Developers][4])

---

# 11. Handle input the Compose way

Your text fields in Compose should not permanently own the important screen data.

### Instead

When the user types:

* the UI sends the new value to the ViewModel
* the ViewModel updates state
* the UI redraws with the latest state

This is called **state hoisting**, and it is a core Compose best practice when business logic or screen-level state is involved. ([Android Developers][5])

### Why this matters

If the screen owns the input but the ViewModel also owns part of it, things get messy fast. Then your login form starts acting possessed.

---

# 12. Validate before sending to Firebase

Before the ViewModel attempts registration or login, it should do basic checks.

### Validate things like:

* email is not blank
* password is not blank
* email looks like an email
* password meets your chosen minimum length

### Why this matters

Firebase can still reject bad input, but client-side validation gives users faster and cleaner feedback before sending the request.

---

# 13. Check whether the user is already logged in when the app opens

Firebase Authentication keeps track of the currently signed-in user.

So when the ViewModel is created, it should immediately check:

* is there already a current user?

If yes:

* update the state to logged in

If no:

* keep the user on the login/register screen

Firebase Auth exposes the current signed-in user so apps can restore authenticated sessions without forcing the user to log in every time they reopen the app. ([Firebase][2])

### Why this matters

This makes the app feel normal. Users should not have to sign in every single time like they are reapplying for citizenship.

---

# 14. Think carefully about loading and error states

A lot of beginner auth screens fail here.

### Your screen should clearly handle:

## Loading

While Firebase is processing:

* disable repeated taps if possible
* show a progress indicator
* avoid sending duplicate login or register requests

## Error

If login fails:

* update the state with a user-friendly message
* show it clearly on screen
* do not leave the loading indicator running forever

### Why this matters

Auth is asynchronous. Firebase does not answer instantly in the same line of thinking. Your UI must be ready for “waiting,” “success,” and “failure.” Firebase’s Android auth APIs are asynchronous, so handling the state transitions correctly is part of the implementation. ([Firebase][3])

---

# 15. Keep the activity thin

Even though you are using only two files, still keep `MainActivity` as light as possible.

### `MainActivity` should mainly do this:

* set content
* get ViewModel
* collect state
* pass user events back to ViewModel
* decide what composable sections appear

### It should not do this:

* direct Firebase login calls
* input validation rules
* auth decision-making
* manual business logic

Android’s architecture guidance for Compose emphasizes that ViewModels expose screen UI state and activities host composables rather than taking on that business logic role. ([Android Developers][1])

---

# 16. Test the full flow in this order

Once implemented, test each stage carefully.

## Stage 1: Firebase connection

Check that:

* project sync succeeds
* app builds
* no Firebase setup errors appear

## Stage 2: Registration

Try:

* valid email + valid password
* invalid email
* weak password
* blank inputs

## Stage 3: Login

Try:

* correct credentials
* incorrect password
* non-existent account

## Stage 4: Session persistence

Try:

* log in
* close the app
* reopen it
* confirm user stays logged in if expected

## Stage 5: Logout

Try:

* sign out
* confirm UI returns to logged-out state

---

# 17. Know the most common mistakes

These are the classics.

## `google-services.json` is in the wrong place

It must be in the app module folder. Firebase’s setup instructions require that config file in the Android app module. ([Firebase][2])

## Email/Password is not enabled in Firebase Console

Then registration/login will fail even if your code is fine. ([Firebase][3])

## Activity is doing too much

If you put all auth logic in `MainActivity`, the app becomes harder to read and maintain.

## UI does not react to state properly

If you are not observing ViewModel state correctly, the screen may not update after login or logout.

## No loading or error handling

Then users tap buttons repeatedly and your auth flow turns into chaos with extra seasoning.

---


[1]: https://developer.android.com/topic/libraries/architecture/viewmodel?utm_source=chatgpt.com "ViewModel overview | App architecture - Android Developers"
[2]: https://firebase.google.com/docs/auth/android/start?utm_source=chatgpt.com "Get Started with Firebase Authentication on Android"
[3]: https://firebase.google.com/docs/auth/android/password-auth?utm_source=chatgpt.com "Authenticate with Firebase using Password-Based Accounts ..."
[4]: https://developer.android.com/develop/ui/compose/state?utm_source=chatgpt.com "State and Jetpack Compose | Android Developers"
[5]: https://developer.android.com/develop/ui/compose/state-hoisting?utm_source=chatgpt.com "Where to hoist state | Jetpack Compose - Android Developers"
