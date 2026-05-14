# Activity: Campus Event Hub with Supabase Storage 🎓

---

# Scenario

Your university wants a simple Android app where student societies can upload and display campus event posters.

Examples of events:

* Hackathons
* Sports tournaments
* Music nights
* Career fairs
* Gaming competitions
* Cultural festivals

Instead of saving images directly on the phone, the app will store posters in **Supabase Storage**.

---

# Real-world context 🌍

Modern apps store files in cloud storage systems.

Examples:

| App        | Stored Files                 |
| ---------- | ---------------------------- |
| Instagram  | photos and reels             |
| LinkedIn   | profile photos and documents |
| Airbnb     | property images              |
| Uber       | driver profile images        |
| Eventbrite | event posters                |

In this activity:

```text
Android App -> Supabase Storage -> Cloud Storage Bucket
```

You will simulate a real mobile app connected to a cloud backend.

---

# App Concept 📱

## App Name

```text
Campus Event Hub
```

## Main Features

The user can:

* choose an event poster image using the Android Photo Picker
* upload the image to Supabase Storage
* retrieve the uploaded image
* display the uploaded poster back to the user
* view upload success messages

Example uploads:

```text
HackathonPoster.jpg
MusicNight.png
CareerFairPoster.jpg
```

---

# Part 1: Create the Supabase Project ☁️

---

# Step 1: Create a Supabase account

1. Go to Supabase.
2. Sign up or log in.
3. Click:

```text
New Project
```

4. Enter:

```text
Project Name:
CampusEventHub
```

```text
Database Password:
Create a secure password
```

```text
Region:
Choose the closest available region
```

5. Click:

```text
Create new project
```

---

# Step 2: Get your API details 🔑

Go to:

```text
Project Settings -> API
```

Copy:

```text
Project URL
anon public key
```

These will be used inside Android Studio.

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

Create a bucket called:

```text
event-posters
```

Set the bucket visibility to:

```text
Public
```

This allows uploaded images to be displayed easily inside the app during testing.

---

# Why use buckets?

Buckets organise files inside cloud storage systems.

Examples:

| Bucket Name    | Purpose               |
| -------------- | --------------------- |
| profile-images | user profile pictures |
| receipts       | uploaded receipts     |
| event-posters  | campus event posters  |
| pet-images     | pet adoption photos   |
| documents      | uploaded PDFs         |

---

# Part 2: Create the Android App 📱

---

# Step 1: Create a new Android Studio project

Create:

```text
Empty Activity
```

App name:

```text
CampusEventHub
```

Language:

```text
Kotlin
```

---

# Step 2: Add dependencies ⚙️

Open:

```text
build.gradle.kts (Module: app)
```

Add:

```kotlin
dependencies {

    implementation("io.github.jan-tennert.supabase:storage-kt:3.1.4")

    implementation("io.ktor:ktor-client-android:3.0.3")

    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.8.7")

    implementation("io.coil-kt:coil-compose:2.7.0")
}
```

---

# Why are these dependencies needed? 🤔

| Dependency                  | Purpose                            |
| --------------------------- | ---------------------------------- |
| storage-kt                  | communicates with Supabase Storage |
| ktor-client-android         | handles internet requests          |
| lifecycle-viewmodel-compose | manages app state                  |
| coil-compose                | displays uploaded images           |

---

# Part 3: Add Internet Permission 🌐

Open:

```text
AndroidManifest.xml
```

Add this above the `<application>` tag:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

Without internet permission, the app cannot connect to Supabase.

---

# Part 4: Create the Supabase Client 🔌

Create a Kotlin file:

```text
SupabaseClientProvider.kt
```

This file will:

* connect the app to Supabase
* store the project URL
* store the anon public key

You must replace:

```text
PASTE_YOUR_SUPABASE_URL
PASTE_YOUR_SUPABASE_ANON_KEY
```

with your own Supabase project details.

---

# Part 5: Create the ViewModel 🧠

Create:

```text
EventPosterViewModel.kt
```

The ViewModel will:

* upload images to Supabase Storage
* retrieve image URLs
* manage upload states
* store success/error messages

---

# Part 6: Build the Compose UI 🎨

Open:

```text
MainActivity.kt
```

The app UI should include:

| Component     | Purpose                    |
| ------------- | -------------------------- |
| Button        | opens Android Photo Picker |
| Image         | displays uploaded poster   |
| Text          | shows upload status        |
| Upload button | uploads selected image     |

---

# Android Photo Picker 📸

The Android Photo Picker allows users to safely choose images from their device without requesting full storage permissions.

This is the modern recommended Android approach.

---

# App Workflow 🔥

```text
1. User selects a poster image
        ->
2. App uploads image to Supabase Storage
        ->
3. Supabase returns the image URL
        ->
4. App displays uploaded image
        ->
5. Success message shown to user
```

---

# Expected Output ✅

Before upload:

```text
Campus Event Hub 🎓
No poster uploaded yet.
```

After upload:

```text
Poster uploaded successfully 🎉
```

The selected image should now appear inside the app.

---

# Testing the App 🧪

Test the following:

| Test          | Expected Result         |
| ------------- | ----------------------- |
| Select image  | Photo Picker opens      |
| Upload image  | Upload succeeds         |
| Display image | Uploaded image appears  |
| Internet off  | Error message displayed |

---

# Real-world explanation 🌍

This activity demonstrates how mobile apps connect to cloud backend systems.

Instead of building your own:

* file server
* storage system
* image hosting service

Supabase provides these services for developers.

This is known as:

```text
Backend-as-a-Service (BaaS)
```

You are therefore building a real cloud-connected Android application similar to systems used in professional software development.
