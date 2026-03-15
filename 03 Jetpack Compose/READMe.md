# 🚀 Introduction to Jetpack Compose

Jetpack Compose is Android’s **modern toolkit for building user interfaces**. Instead of creating layouts using XML files and connecting them to Kotlin code, Jetpack Compose allows developers to build the entire interface **directly in Kotlin code**.

In traditional Android development, we separate UI and logic:

- **XML** → defines what the screen looks like  
- **Kotlin** → controls how the app behaves  

Jetpack Compose changes this approach. With Compose:

- The **UI is written using Kotlin functions**
- The screen updates automatically when data changes
- Less boilerplate code is needed
- UI development becomes faster and more flexible

Google introduced Jetpack Compose to simplify Android UI development and make it more similar to modern frameworks like **React, Flutter, and SwiftUI**.

Think of Compose like building the screen using **Lego blocks in Kotlin** 🧱.

---

# 📚 Official Learning Resource

Before starting the activity, complete the official Jetpack Compose tutorial provided by Google:

👉 https://developer.android.com/compose

This resource introduces:

- What Jetpack Compose is
- How composables work
- Layouts and UI elements
- Handling state
- Building your first Compose screen

Students should **work through the tutorial step-by-step** to understand the basic concepts before continuing with further Compose activities.

---

# 🧠 Key Concept: Composable Functions

The main building block of Jetpack Compose is the **Composable function**.

A composable is a Kotlin function that describes part of the UI.

Example:

```kotlin
@Composable
fun Greeting() {
    Text("Hello Android!")
}
```

The `@Composable` annotation tells Android that this function creates UI elements.

---

# 🛠️ Creating a Simple Jetpack Compose App

We will now create a simple app that displays a message and updates it when a button is pressed.

---

# Step 1: Create a New Compose Project 📱

1. Open **Android Studio**
2. Click **New Project**
3. Select **Empty Activity (Jetpack Compose)**
4. Configure the project:

| Setting     | Value            |
| ----------- | ---------------- |
| Name        | ComposeDemo      |
| Language    | Kotlin           |
| Minimum SDK | API 24 or higher |

Click **Finish**.

Android Studio will create a Compose-ready project with the required dependencies.

---

# Step 2: Understand the Main File 🧩

The main UI will be created inside:

```
MainActivity.kt
```

Compose apps do **not use XML layout files**.

Instead, the UI is defined using composable functions.

---

# Step 3: Create a Simple Compose UI 🎨

Replace the contents of `MainActivity.kt` with the following example.

```kotlin
package com.example.composedemo

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Button
import androidx.compose.material3.Text
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.compose.ui.Alignment

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // set the compose ui content for this activity
        setContent {

            // call the composable function that creates our screen
            SimpleComposeApp()

        }
    }
}

@Composable
fun SimpleComposeApp() {

    // state variable that stores the message displayed on the screen
    var message by remember { mutableStateOf("Welcome to Jetpack Compose!") }

    // column arranges elements vertically
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(24.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {

        // display text on the screen
        Text(text = message)

        Spacer(modifier = Modifier.height(20.dp))

        // button that changes the text when clicked
        Button(onClick = {

            // update the message
            message = "Button clicked!"

        }) {

            Text("Click Me")

        }

    }
}
```

---

# 🔎 What This Code Does

### `setContent {}`

This tells Android that the UI will be built using **Jetpack Compose**.

---

### `@Composable`

Marks a function as a **UI component**.

---

### `Column`

A layout that arranges UI elements **vertically**.

---

### `remember` and `mutableStateOf`

These create **state variables** that allow the UI to update automatically when values change.

---

### `Button`

Runs code when the user presses it.

---

### `Text`

Displays text on the screen.

---

# ▶️ Running the App

1. Click the **Run ▶ button** in Android Studio.
2. Choose an emulator or a physical device.
3. Wait for the app to launch.

You should see:

* A message on the screen
* A button below the message
* When the button is pressed, the text changes

---

# 🎯 Why Jetpack Compose Matters

Jetpack Compose simplifies Android UI development because:

* No XML layouts are required
* UI and logic live in the same place
* The UI automatically updates when state changes
* Less code is needed compared to traditional Android views

It is now the **recommended way to build new Android UIs**.

---

# 📌 Next Step

Before continuing with more Compose projects, complete the official tutorial:

👉 [https://developer.android.com/compose](https://developer.android.com/compose)

Work through the examples and practise building composables so that you become comfortable with how Compose structures UI.

Once these basics are clear, you can start building more complex apps using **state management, navigation, animations, and lists** in Jetpack Compose.

