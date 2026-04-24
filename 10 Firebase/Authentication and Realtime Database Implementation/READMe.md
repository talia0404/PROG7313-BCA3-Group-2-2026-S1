# 🔥 Firebase Auth and Realtime Database

## 🔐 Part 1: Firebase Authentication

Firebase Authentication handles:

* registering a user
* logging in a user
* checking if a user is already logged in
* logging out a user

---

### 📄 `AuthUiState.kt`

```kotlin
package com.talia.firebasedemogr2_2026

/*
this data class stores all the information needed by the auth screen.

compose reads this state and automatically updates the screen whenever the state changes.
*/
data class AuthUiState(
    val email: String = "",
    val password: String = "",
    val isLoading: Boolean = false,
    val isLoggedIn: Boolean = false,
    val message: String = ""
)
```

---

### 📄 `AuthViewModel.kt`

```kotlin
package com.talia.firebasedemogr2_2026

import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.google.firebase.auth.FirebaseAuth
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.update
import kotlinx.coroutines.launch
import kotlinx.coroutines.tasks.await

/*
this viewmodel controls the firebase authentication logic.

it handles:
registering a user,
logging in a user,
logging out a user,
updating the email and password fields,
and sending messages back to the ui.
*/
class AuthViewModel : ViewModel() {

    /*
    this creates an instance of firebase authentication.

    firebaseauth is the main class used to communicate with firebase authentication.
    */
    private val auth: FirebaseAuth = FirebaseAuth.getInstance()

    /*
    this is the private editable version of the ui state.

    only the viewmodel should change this state.
    */
    private val _uiState = MutableStateFlow(
        AuthUiState(
            /*
            this checks if firebase already has a logged-in user.

            if the user previously logged in and did not log out,
            firebase may still remember their session.
            */
            isLoggedIn = auth.currentUser != null
        )
    )

    /*
    this is the public read-only version of the ui state.

    the ui can read it, but it cannot directly change it.
    */
    val uiState: StateFlow<AuthUiState> = _uiState

    /*
    this function runs every time the user types in the email text field.

    it updates only the email value and keeps the rest of the state the same.
    */
    fun updateEmail(newEmail: String) {
        _uiState.update {
            it.copy(email = newEmail)
        }
    }

    /*
    this function runs every time the user types in the password text field.

    it updates only the password value and keeps the rest of the state the same.
    */
    fun updatePassword(newPassword: String) {
        _uiState.update {
            it.copy(password = newPassword)
        }
    }

    /*
    this function creates a new user account using firebase authentication.
    */
    fun registerUser() {

        /*
        trim removes extra spaces from the beginning and end of the email.
        this prevents login problems caused by accidental spaces.
        */
        val email = _uiState.value.email.trim()

        /*
        the password is kept as typed because spaces may technically be part of a password.
        */
        val password = _uiState.value.password

        /*
        this prevents firebase from being called if the user has not entered both fields.
        */
        if (email.isBlank() || password.isBlank()) {
            _uiState.update {
                it.copy(message = "Please enter email and password.")
            }
            return
        }

        /*
        firebase calls are asynchronous.

        views should not be frozen while waiting for firebase,
        so this work runs inside a coroutine.
        */
        viewModelScope.launch {

            /*
            this tells the ui to show a loading indicator while firebase is busy.
            */
            _uiState.update {
                it.copy(
                    isLoading = true,
                    message = ""
                )
            }

            try {
                /*
                this sends the email and password to firebase.

                await pauses this coroutine until firebase gives back a result.
                */
                auth.createUserWithEmailAndPassword(email, password).await()

                /*
                if registration succeeds, the user is now logged in.
                the screen will switch to the realtime database screen.
                */
                _uiState.update {
                    it.copy(
                        isLoading = false,
                        isLoggedIn = true,
                        message = "Registration successful."
                    )
                }

            } catch (e: Exception) {

                /*
                if firebase rejects the registration, the error message is shown to the user.
                */
                _uiState.update {
                    it.copy(
                        isLoading = false,
                        message = e.message ?: "Registration failed."
                    )
                }
            }
        }
    }

    /*
    this function logs in an existing firebase user.
    */
    fun loginUser() {
        val email = _uiState.value.email.trim()
        val password = _uiState.value.password

        /*
        this basic validation prevents empty login attempts.
        */
        if (email.isBlank() || password.isBlank()) {
            _uiState.update {
                it.copy(message = "Please enter email and password.")
            }
            return
        }

        viewModelScope.launch {

            /*
            show loading while firebase checks the login details.
            */
            _uiState.update {
                it.copy(
                    isLoading = true,
                    message = ""
                )
            }

            try {
                /*
                this attempts to sign the user in using firebase authentication.
                */
                auth.signInWithEmailAndPassword(email, password).await()

                /*
                if login succeeds, the app state changes to logged in.
                mainactivity will then display the notes screen.
                */
                _uiState.update {
                    it.copy(
                        isLoading = false,
                        isLoggedIn = true,
                        message = "Login successful."
                    )
                }

            } catch (e: Exception) {

                /*
                if login fails, show the firebase error message.
                */
                _uiState.update {
                    it.copy(
                        isLoading = false,
                        message = e.message ?: "Login failed."
                    )
                }
            }
        }
    }

    /*
    this function logs the user out of firebase.
    */
    fun logoutUser() {

        /*
        this clears the firebase authentication session.
        */
        auth.signOut()

        /*
        this resets the app back to the logged-out state.

        because isloggedin becomes false, mainactivity will show the auth screen again.
        */
        _uiState.update {
            it.copy(
                isLoggedIn = false,
                email = "",
                password = "",
                message = "Logged out."
            )
        }
    }
}
```

---

### 🖥️ Auth screen explanation

Your `AuthScreen` is responsible for showing:

* email field
* password field
* register button
* login button
* loading indicator
* message text

The screen does **not** talk directly to Firebase(good practice as explained in class)

Instead:

* the screen sends events to `AuthViewModel`
* the ViewModel talks to Firebase
* the ViewModel updates the state
* Compose redraws the screen

---

## 🔥 Part 2: Firebase Realtime Database

Realtime Database is used after the user logs in.

It stores notes under the logged-in user’s UID.

Your database path is:

```text
users / useruid / notes / noteid
```

This is a good structure because each user gets their own notes.

---

### 📄 `NoteItem.kt`

```kotlin
package com.talia.firebasedemogr2_2026

/*
this data class represents one note stored in firebase realtime database.

firebase needs default values so it can convert database snapshots back into kotlin objects.
*/
data class NoteItem(
    val id: String = "",
    val text: String = "",
    val timestamp: Long = 0L
)
```

---

### 📄 `NotesUiState.kt`

```kotlin
package com.talia.firebasedemogr2_2026

/*
this data class stores everything the notes screen needs.

it stores:
the text currently typed by the user,
the list of notes loaded from firebase,
the loading state,
and a message for success or error feedback.
*/
data class NotesUiState(
    val noteInput: String = "",
    val notes: List<NoteItem> = emptyList(),
    val isLoading: Boolean = false,
    val message: String = ""
)
```

---

### 📄 `NotesViewModel.kt`

```kotlin
package com.talia.firebasedemogr2_2026

import androidx.lifecycle.ViewModel
import com.google.firebase.auth.FirebaseAuth
import com.google.firebase.database.DataSnapshot
import com.google.firebase.database.DatabaseError
import com.google.firebase.database.DatabaseReference
import com.google.firebase.database.FirebaseDatabase
import com.google.firebase.database.ValueEventListener
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.flow.update

/*
this viewmodel controls all realtime database logic.

it handles:
getting the logged-in user,
creating the correct database path,
saving notes,
loading notes in real time,
and stopping the listener when needed.
*/
class NotesViewModel : ViewModel() {

    /*
    this gives access to the currently logged-in firebase user.
    */
    private val auth: FirebaseAuth = FirebaseAuth.getInstance()

    /*
    this gives access to firebase realtime database.
    */
    private val database: FirebaseDatabase = FirebaseDatabase.getInstance()

    /*
    this is the private editable state for the notes screen.
    */
    private val _uiState = MutableStateFlow(NotesUiState())

    /*
    this is the public read-only state for the notes screen.
    */
    val uiState: StateFlow<NotesUiState> = _uiState

    /*
    this stores the database location where the current user's notes are stored.
    */
    private var notesRef: DatabaseReference? = null

    /*
    this stores the realtime database listener.

    keeping a reference to it allows us to remove it later.
    */
    private var notesListener: ValueEventListener? = null

    /*
    this updates the note input field as the user types.
    */
    fun updateNoteInput(newValue: String) {
        _uiState.update {
            it.copy(noteInput = newValue)
        }
    }

    /*
    this function starts listening for notes from firebase realtime database.
    */
    fun startListeningForNotes() {

        /*
        get the currently logged-in firebase user.
        */
        val currentUser = auth.currentUser

        /*
        if there is no logged-in user, the database should not be accessed.
        */
        if (currentUser == null) {
            _uiState.update {
                it.copy(
                    isLoading = false,
                    message = "No authenticated user found."
                )
            }
            return
        }

        /*
        this removes any old listener before adding a new one.

        this prevents duplicate listeners from being attached.
        */
        stopListening()

        /*
        this tells the screen that data is loading.
        */
        _uiState.update {
            it.copy(
                isLoading = true,
                message = ""
            )
        }

        /*
        the uid uniquely identifies the logged-in user.

        each user's notes are stored under their own uid.
        */
        val uid = currentUser.uid

        /*
        this creates the path:
        users / uid / notes
        */
        notesRef = database
            .getReference("users")
            .child(uid)
            .child("notes")

        /*
        this listener runs whenever the notes data changes.

        it also runs once immediately when it is first attached.
        */
        notesListener = object : ValueEventListener {

            /*
            this function receives the latest data from firebase.
            */
            override fun onDataChange(snapshot: DataSnapshot) {

                /*
                this temporary list stores all notes loaded from firebase.
                */
                val loadedNotes = mutableListOf<NoteItem>()

                /*
                each child represents one note in the database.
                */
                for (child in snapshot.children) {

                    /*
                    firebase converts the snapshot into a noteitem object.
                    */
                    val note = child.getValue(NoteItem::class.java)

                    /*
                    only add the note if it was converted successfully.
                    */
                    if (note != null) {
                        loadedNotes.add(note)
                    }
                }

                /*
                the notes are sorted so the newest note appears first.
                */
                _uiState.update {
                    it.copy(
                        notes = loadedNotes.sortedByDescending { note -> note.timestamp },
                        isLoading = false,
                        message = ""
                    )
                }
            }

            /*
            this function runs if firebase cancels the read operation.
            */
            override fun onCancelled(error: DatabaseError) {
                _uiState.update {
                    it.copy(
                        isLoading = false,
                        message = error.message
                    )
                }
            }
        }

        /*
        this attaches the listener to firebase.

        after this line, the app starts receiving realtime updates.
        */
        notesRef?.addValueEventListener(notesListener as ValueEventListener)
    }

    /*
    this function saves a new note to firebase realtime database.
    */
    fun saveNote() {

        /*
        get the currently logged-in firebase user.
        */
        val currentUser = auth.currentUser

        /*
        get the text typed by the user and remove unnecessary spaces.
        */
        val noteText = _uiState.value.noteInput.trim()

        /*
        if there is no logged-in user, do not save anything.
        */
        if (currentUser == null) {
            _uiState.update {
                it.copy(message = "User is not logged in.")
            }
            return
        }

        /*
        prevent blank notes from being saved.
        */
        if (noteText.isBlank()) {
            _uiState.update {
                it.copy(message = "Please enter a note first.")
            }
            return
        }

        /*
        get the current user's uid.
        */
        val uid = currentUser.uid

        /*
        create the path where the user's notes are stored.
        */
        val userNotesRef = database
            .getReference("users")
            .child(uid)
            .child("notes")

        /*
        push creates a new unique location inside the notes node.
        */
        val newNoteRef = userNotesRef.push()

        /*
        get the unique key generated by firebase.
        */
        val noteId = newNoteRef.key ?: return

        /*
        create the note object that will be saved.
        */
        val note = NoteItem(
            id = noteId,
            text = noteText,
            timestamp = System.currentTimeMillis()
        )

        /*
        save the note object into firebase.
        */
        newNoteRef.setValue(note)
            .addOnSuccessListener {

                /*
                if the note saves successfully, clear the input field and show a message.
                */
                _uiState.update {
                    it.copy(
                        noteInput = "",
                        message = "Note saved successfully."
                    )
                }
            }
            .addOnFailureListener { exception ->

                /*
                if saving fails, show an error message.
                */
                _uiState.update {
                    it.copy(
                        message = exception.message ?: "Failed to save note."
                    )
                }
            }
    }

    /*
    this function removes the realtime database listener.
    */
    fun stopListening() {

        /*
        only remove the listener if both the database reference and listener exist.
        */
        if (notesRef != null && notesListener != null) {
            notesRef?.removeEventListener(notesListener!!)
        }

        /*
        clear both variables after removing the listener.
        */
        notesListener = null
        notesRef = null
    }

    /*
    this runs when the viewmodel is destroyed.

    it is important to stop listening so the app does not keep unnecessary database listeners alive.
    */
    override fun onCleared() {
        super.onCleared()
        stopListening()
    }
}
```

---

## 🧭 Part 3: MainActivity Screen Flow

`MainActivity` decides which screen to show.

The logic is simple:

* if the user is **not logged in**, show `AuthScreen`
* if the user **is logged in**, show `NotesScreen`

---

### 📄 `MainActivity.kt`

```kotlin
package com.talia.firebasedemogr2_2026

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.viewModels
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.Button
import androidx.compose.material3.CircularProgressIndicator
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.LaunchedEffect
import androidx.compose.runtime.collectAsState
import androidx.compose.runtime.getValue
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.unit.dp

/*
mainactivity is the entry point of the app.

it controls which screen is shown based on whether the user is logged in or not.
*/
class MainActivity : ComponentActivity() {

    /*
    this viewmodel handles firebase authentication.
    */
    private val authViewModel: AuthViewModel by viewModels()

    /*
    this viewmodel handles firebase realtime database notes.
    */
    private val notesViewModel: NotesViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContent {

            /*
            collectasstate allows compose to observe the auth state.

            when the state changes, the screen recomposes automatically.
            */
            val authUiState by authViewModel.uiState.collectAsState()

            /*
            this observes the realtime database screen state.
            */
            val notesUiState by notesViewModel.uiState.collectAsState()

            MaterialTheme {
                Surface(
                    modifier = Modifier.fillMaxSize()
                ) {

                    /*
                    if the user is not logged in, show the authentication screen.
                    */
                    if (!authUiState.isLoggedIn) {
                        AuthScreen(
                            uiState = authUiState,
                            onEmailChange = { authViewModel.updateEmail(it) },
                            onPasswordChange = { authViewModel.updatePassword(it) },
                            onRegisterClick = { authViewModel.registerUser() },
                            onLoginClick = { authViewModel.loginUser() }
                        )
                    } else {

                        /*
                        this runs once when the notes screen is shown.

                        it starts listening for notes from firebase realtime database.
                        */
                        LaunchedEffect(Unit) {
                            notesViewModel.startListeningForNotes()
                        }

                        /*
                        if the user is logged in, show the notes screen.
                        */
                        NotesScreen(
                            uiState = notesUiState,
                            onNoteTextChange = { notesViewModel.updateNoteInput(it) },
                            onSaveClick = { notesViewModel.saveNote() },
                            onLogoutClick = {
                                notesViewModel.stopListening()
                                authViewModel.logoutUser()
                            }
                        )
                    }
                }
            }
        }
    }
}
```

---

## 🔑 Part 4: AuthScreen Composable

```kotlin
@Composable
fun AuthScreen(
    uiState: AuthUiState,
    onEmailChange: (String) -> Unit,
    onPasswordChange: (String) -> Unit,
    onRegisterClick: () -> Unit,
    onLoginClick: () -> Unit
) {
    /*
    this column arranges all auth screen elements vertically.
    */
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(24.dp),
        verticalArrangement = Arrangement.Center
    ) {
        Text(
            text = "Firebase Auth Demo",
            style = MaterialTheme.typography.headlineMedium
        )

        Spacer(modifier = Modifier.height(16.dp))

        /*
        this text field allows the user to type their email address.
        */
        OutlinedTextField(
            value = uiState.email,
            onValueChange = onEmailChange,
            label = { Text("Email") },
            singleLine = true,
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(12.dp))

        /*
        this text field allows the user to type their password.

        passwordvisualtransformation hides the password characters.
        */
        OutlinedTextField(
            value = uiState.password,
            onValueChange = onPasswordChange,
            label = { Text("Password") },
            singleLine = true,
            visualTransformation = PasswordVisualTransformation(),
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(16.dp))

        /*
        if firebase is busy, show a loading spinner.
        */
        if (uiState.isLoading) {
            CircularProgressIndicator()
        } else {

            /*
            this button creates a new firebase auth account.
            */
            Button(onClick = onRegisterClick) {
                Text("Register")
            }

            Spacer(modifier = Modifier.height(8.dp))

            /*
            this button logs in an existing firebase auth user.
            */
            Button(onClick = onLoginClick) {
                Text("Login")
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        /*
        this shows success or error messages from the auth viewmodel.
        */
        if (uiState.message.isNotBlank()) {
            Text(
                text = uiState.message,
                color = MaterialTheme.colorScheme.primary
            )
        }
    }
}
```

---

## 📝 Part 5: NotesScreen Composable

```kotlin
@Composable
fun NotesScreen(
    uiState: NotesUiState,
    onNoteTextChange: (String) -> Unit,
    onSaveClick: () -> Unit,
    onLogoutClick: () -> Unit
) {
    /*
    this column arranges the notes screen vertically.
    */
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(24.dp)
    ) {
        Text(
            text = "My Notes",
            style = MaterialTheme.typography.headlineMedium
        )

        Spacer(modifier = Modifier.height(16.dp))

        /*
        this text field allows the user to type a note.
        */
        OutlinedTextField(
            value = uiState.noteInput,
            onValueChange = onNoteTextChange,
            label = { Text("Enter a note") },
            modifier = Modifier.fillMaxWidth(),
            singleLine = true
        )

        Spacer(modifier = Modifier.height(12.dp))

        /*
        this button saves the note to firebase realtime database.
        */
        Button(onClick = onSaveClick) {
            Text("Save Note")
        }

        Spacer(modifier = Modifier.height(12.dp))

        /*
        this button logs the user out and returns them to the auth screen.
        */
        Button(onClick = onLogoutClick) {
            Text("Logout")
        }

        Spacer(modifier = Modifier.height(16.dp))

        /*
        show a loading spinner while notes are being loaded.
        */
        if (uiState.isLoading) {
            CircularProgressIndicator()
            Spacer(modifier = Modifier.height(16.dp))
        }

        /*
        show success or error messages from the notes viewmodel.
        */
        if (uiState.message.isNotBlank()) {
            Text(
                text = uiState.message,
                color = MaterialTheme.colorScheme.primary
            )
            Spacer(modifier = Modifier.height(16.dp))
        }

        /*
        if there are no notes, show an empty state message.
        */
        if (uiState.notes.isEmpty()) {
            Text("No notes yet. Add your first one.")
        } else {

            /*
            lazycolumn efficiently displays a scrollable list of notes.
            */
            LazyColumn {
                items(uiState.notes) { note ->

                    /*
                    each note is displayed as text.
                    */
                    Text(
                        text = "• ${note.text}",
                        style = MaterialTheme.typography.bodyLarge,
                        modifier = Modifier.padding(vertical = 6.dp)
                    )
                }
            }
        }
    }
}
```

---

## 🔌 Part 6: Gradle Dependencies

Your Firebase dependencies are correct:

```kotlin
implementation(platform("com.google.firebase:firebase-bom:34.12.0"))
implementation("com.google.firebase:firebase-auth")
implementation("com.google.firebase:firebase-database")
```

These give you:

* Firebase Authentication
* Firebase Realtime Database
* Firebase version management through the BoM

Also keep:

```kotlin
id("com.google.gms.google-services")
```

That plugin connects your app to the `google-services.json` file.

---

## 🧠 How Everything Works Together

### 🔐 Authentication flow

1. User enters email and password.
2. User clicks Register or Login.
3. `AuthScreen` sends the click event to `AuthViewModel`.
4. `AuthViewModel` calls Firebase Authentication.
5. Firebase returns success or failure.
6. If successful, `isLoggedIn` becomes `true`.
7. `MainActivity` sees that the user is logged in.
8. The app switches to `NotesScreen`.

---

### 🔥 Realtime Database flow

1. `NotesScreen` opens after login.
2. `LaunchedEffect` runs.
3. `startListeningForNotes()` is called.
4. The app gets the current Firebase user.
5. The app gets the user’s UID.
6. The app listens to:

```text
users / uid / notes
```

7. Firebase sends the current notes.
8. The UI displays them.
9. When a new note is saved, Firebase updates the database.
10. The listener receives the new data automatically.
11. Compose updates the list on screen.

---

## 🛡️ Recommended Realtime Database Rules

Use this after testing:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "auth != null && auth.uid == $uid",
        ".write": "auth != null && auth.uid == $uid"
      }
    }
  }
}
```

This means:

* users must be logged in
* users can only read their own data
* users can only write to their own data

---
