# 📱 Navigation Bars in Jetpack Compose

There are **2 main ways** to implement a navigation bar in a Jetpack Compose app:

---

# 🥇 Method 1: Single Activity + Navigation Compose

> This is the **recommended approach** today.

## 🧠 Characteristics

* You have **ONE activity**
* You switch between **screens (composables)**
* The nav bar controls which screen is shown

---

## How it works

```
NavBar click → NavController → Switch Composable Screen
```

---

## 💻 Example

### NavHost setup

```kotlin
val navController = rememberNavController()

NavHost(
    navController = navController,
    startDestination = "home"
) {
    composable("home") { HomeScreen() }
    composable("profile") { ProfileScreen() }
}
```

---

### Bottom Navigation Bar

```kotlin
NavigationBar {

    NavigationBarItem(
        selected = true,
        onClick = { navController.navigate("home") },
        icon = { Icon(Icons.Default.Home, contentDescription = "Home") },
        label = { Text("Home") }
    )

    NavigationBarItem(
        selected = false,
        onClick = { navController.navigate("profile") },
        icon = { Icon(Icons.Default.Person, contentDescription = "Profile") },
        label = { Text("Profile") }
    )
}
```

---

## Is this better?

- Modern Android architecture
- Better performance
- Easier state management
- Used in real apps

---

# 🥈 Method 2: Multiple Activities + Intents

> This is used for multiple activities.

## 🧠 Characteristics

* Each screen = **different Activity**
* Nav bar uses **Intents** to switch screens

---

## How it works

```
NavBar click → Intent → Open Activity
```

---

## 💻 Example

```kotlin
val context = LocalContext.current

NavigationBarItem(
    selected = false,
    onClick = {
        val intent = Intent(context, SecondActivity::class.java)
        context.startActivity(intent)
    },
    icon = { Icon(Icons.Default.Photo, contentDescription = "Photo") },
    label = { Text("Photo") }
)
```

---

## Important

Use flags to avoid duplicate screens:

```kotlin
intent.flags = Intent.FLAG_ACTIVITY_CLEAR_TOP or Intent.FLAG_ACTIVITY_SINGLE_TOP
```

---

## Downsides

* Can create messy back stack
* Harder to manage state
* Not typical for modern Compose apps


---


