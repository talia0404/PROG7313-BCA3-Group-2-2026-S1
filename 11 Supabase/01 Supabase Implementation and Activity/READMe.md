# Activity: Campus Event Hub with Supabase Storage 🎓

---

# Scenario

Your university wants a simple mobile app where student societies can upload posters for campus events.

Examples:

* Hackathons
* Sports events
* Music nights
* Career fairs
* Gaming tournaments
* Cultural events

Instead of storing posters directly on the device, the app will upload them to **Supabase Storage**.

---

# Real-world context 🌍

Apps like:

* LinkedIn
* Instagram
* Uber
* Airbnb
* Eventbrite

all upload files to cloud storage systems instead of storing files directly on users’ devices.

In this activity:

```text
Android App -> Supabase Storage Bucket -> Cloud File Storage
```

You will simulate a real campus event management system.

---

# App concept

## App name

```text
Campus Event Hub
```

## Main feature

The user can:

* choose an event poster image
* upload it to Supabase Storage
* view upload success messages

Example poster uploads:

```text
HackathonPoster.jpg
BasketballTournament.png
CulturalNightPoster.jpg
```

---

# Part 1: Create the Supabase project

## Step 1: Create a Supabase account

1. Go to Supabase.
2. Sign up or log in.
3. Click:

```text
New Project
```

4. Enter:

   * Project name:

```text
CampusEventHub
```

* Database password:
  create a secure password

* Region:
  choose the closest available region

5. Click:

```text
Create new project
```

---

# Step 2: Get your API details

Go to:

```text
Project Settings -> API
```

Copy:

```text
Project URL
anon public key
```

You will use these inside Android Studio.

Example:

```kotlin
const val SUPABASE_URL = "https://your-project.supabase.co"
const val SUPABASE_ANON_KEY = "your-anon-key"
```

---

# Step 3: Create a storage bucket 🪣

Go to:

```text
Storage
```

Click:

```text
New Bucket
```

Create:

```text
event-posters
```

Set the bucket to:

```text
Public
```

This allows you to test uploads easily.

---

# Why use buckets?

Buckets organise files in cloud storage.

Example buckets in real apps:

| Bucket         | Purpose               |
| -------------- | --------------------- |
| profile-images | user profile pictures |
| receipts       | uploaded receipts     |
| event-posters  | event posters         |
| pet-images     | pet adoption photos   |
| documents      | uploaded PDFs         |

---

# Part 2: Create the Android app

## Step 1: Create a new Android Studio project


Add:

```kotlin
dependencies {

    implementation("io.github.jan-tennert.supabase:storage-kt:3.1.4")
    implementation("io.github.jan-tennert.supabase:auth-kt:3.1.4")
    implementation("io.github.jan-tennert.supabase:postgrest-kt:3.1.4")

    implementation("io.ktor:ktor-client-android:3.0.3")

    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.8.7")
}
```

---

# Part 4: Add internet permission 🌐

Open:

```text
AndroidManifest.xml
```

Add:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

above the `<application>` tag.

Without this permission, the app cannot communicate with Supabase.

---

# Part 5: Create the Supabase client

Create a Kotlin file:

```text
SupabaseClientProvider.kt
```

You must replace:

```text
PASTE_YOUR_SUPABASE_URL
PASTE_YOUR_SUPABASE_ANON_KEY
```

with their actual project details.

---

# Part 6: Create the ViewModel

Create:

```text
EventPosterViewModel.kt
```

# Part 7: Build the Compose UI

Open:

```text
MainActivity.kt
```

# Part 8: Run the app 🚀

7. Verify the image uploaded successfully.

---

# Expected output

Before upload:

```text
Campus Event Hub 🎓
No poster uploaded yet.
```

After upload:

```text
Poster uploaded successfully 🎉
```

---

# Extra activity functions: 🔥

You can later add:

| Feature              | Supabase service    |
| -------------------- | ------------------- |
| User login           | Auth                |
| Save event details   | PostgreSQL database |
| Like/bookmark events | Database            |
| Live updates         | Realtime            |
| Admin uploads        | Auth + Storage      |
| Event comments       | Database            |

---

# Real-world explanation

This activity demonstrates how modern mobile apps communicate with backend cloud services.

Instead of manually building:

* servers
* APIs
* authentication systems
* file storage systems

Supabase provides these services as a **Backend-as-a-Service (BaaS)** platform.

You are therefore building a mobile app connected to a real cloud backend environment used in professional software development.
