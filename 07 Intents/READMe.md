# 🚀 What is an Intent?

An **Intent** in Android is basically a **message object** that lets one part of your app talk to another.

Think of it like:

> “I want to go to this screen / do this action.”

---

# 🧠 Why we use Intents

You use Intents to:

* Navigate between screens (Activities)
* Send data between screens
* Open external apps (camera, browser, maps, etc.)

---

# ⚡ Types of Intents

## 1. Explicit Intent (most common for your apps)

You **tell Android exactly where to go**.

> Example: Move from `MainActivity` -> `SecondActivity`

---

## 💻 Example: Basic Navigation

###  In `MainActivity.kt`

```kotlin
val intent = Intent(this, SecondActivity::class.java)
startActivity(intent)
```

What’s happening:

* `this` = current screen
* `SecondActivity::class.java` = where you want to go

---

## 2. Implicit Intent

You **don’t specify the exact app**, just the action.

> Example: Open a website

---

## 💻 Example: Open a Website

```kotlin
val intent = Intent(Intent.ACTION_VIEW)
intent.data = Uri.parse("https://www.google.com")
startActivity(intent)
```

Android decides:

* Chrome? Edge? Firefox? Your problem now.

---

## Sending Data

```kotlin
val intent = Intent(this, SecondActivity::class.java)
intent.putExtra("username", "Talia")
startActivity(intent)
```

---

## Receiving Data (in `SecondActivity.kt`)

```kotlin
val username = intent.getStringExtra("username")
```

---

## Start Activity for Result

### Step 1: Register launcher

```kotlin
val launcher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
    if (result.resultCode == RESULT_OK) {
        val data = result.data?.getStringExtra("result")
    }
}
```

### Step 2: Launch

```kotlin
val intent = Intent(this, SecondActivity::class.java)
launcher.launch(intent)
```

---

