# 📦 What is JSON & Why Do We Use It?

## Purpose of JSON Files

**JSON (JavaScript Object Notation)** is a lightweight format used to **store and exchange data**.

It's like a **structured text file** that looks clean and is easy for both people and programs to understand.

### 💡 Why JSON is used:

* Store app data (settings, preferences, offline data)
* Send data between apps and servers (APIs)
* Save structured data in a readable format

### Example JSON file:

```json
{
  "name": "Talia",
  "age": 23,
  "isStudent": true
}
```

---

# 📖 Using JSON Objects to **Read Data**

## What does “reading JSON” mean?

It means:
 Taking JSON data and converting it into something your app can use (objects/variables).

---

## Example in Kotlin (Reading JSON)

### Step 1: JSON string (could come from API or file)

```kotlin
val jsonString = """
{
    "name": "Talia",
    "age": 23
}
"""
```

### Step 2: Convert JSON → Object

Using `JSONObject`:

```kotlin
import org.json.JSONObject

val jsonObject = JSONObject(jsonString)

// extract values
val name = jsonObject.getString("name")
val age = jsonObject.getInt("age")
```

### What’s happening:

* `JSONObject(jsonString)` → converts text into a JSON object
* `getString()` / `getInt()` → pulls out values

---

# ✍️ Using JSON Objects to **Write Data**

## What does “writing JSON” mean?

It means:
 Creating JSON data from your app’s variables.

---

## Example in Kotlin (Writing JSON)

```kotlin
import org.json.JSONObject

val jsonObject = JSONObject()

jsonObject.put("name", "Talia")
jsonObject.put("age", 23)

// convert to string
val jsonString = jsonObject.toString()
```

### Output:

```json
{"name":"Talia","age":23}
```

---

## What’s happening:

* `JSONObject()` → creates empty JSON object
* `put()` → adds key-value pairs
* `toString()` → converts it into JSON format

---

# Quick Summary 

| Concept    | Meaning                          |
| ---------- | -------------------------------- |
| JSON file  | Stores structured data           |
| Read JSON  | Convert JSON → variables         |
| Write JSON | Convert variables → JSON         |
| JSONObject | Tool to work with JSON in Kotlin |

---

# 🚀 Real Use-case Example

In your apps (like SavvySaver 👀), JSON is used for:

* API responses (e.g., fetching expenses)
* Saving user data locally
* Sending data to backend servers

