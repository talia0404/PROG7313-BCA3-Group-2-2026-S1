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

Create a compose project:

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

# Step 2: Add plugin and dependencies ⚙️

In:

```text
build.gradle.kts (Module: app)
```

Add:

```kotlin

plugins {
    kotlin("plugin.serialization") version "2.0.21"
}


dependencies {

    //this ensures that android picks up supabase
    implementation(platform("io.github.jan-tennert.supabase:bom:3.6.0"))

    //db/storage
    implementation("io.github.jan-tennert.supabase:postgrest-kt")
    implementation("io.github.jan-tennert.supabase:storage-kt")

    //supabase client
    implementation("io.ktor:ktor-client-android:3.4.3")

    //jetpack compose
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

Add this in the android manifest file:

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

## Important Supabase Storage Functions

### Writing data to Supabase (Uploading)

Uploading means sending files from the Android app to cloud storage.

Example:

```kotlin
bucket.upload(
    path = "posters/poster1.jpg",
    data = byteArray
)
```

This writes data into the Supabase Storage bucket.

---

### Reading data from Supabase (Retrieving)

Reading means retrieving files from Supabase so they can be displayed in the app.

Example:

```kotlin
val imageUrl = bucket.publicUrl("posters/poster1.jpg")
```

This retrieves the public URL of the uploaded image.

The URL can then be displayed using Coil in Jetpack Compose.

---

# Understanding the Flow 🔄

## Write Flow

```text
Android App
    ->
Select image
    ->
Upload image
    ->
Supabase Storage bucket
```

## Read Flow

```text
Supabase Storage bucket
    ->
Retrieve image URL
    ->
Display image in Android app
```

---

# Tip 💡

When testing Supabase Storage:

* ensure the bucket is public
* ensure internet permission is added
* ensure the correct bucket name is used
* ensure the Supabase URL and anon key are correct

Even one incorrect character in the URL, bucket name, or API key can prevent uploads from working.

---

# Part 6: Build the Compose UI 🎨

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

Replace your current **“Testing the App 🧪”** section and everything below it with this updated version:

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

# Resources 📚

## Official Supabase Documentation

Use these resources to learn more about how Supabase Storage works in Android applications.

| Resource                                                                                                                            | Purpose                              |
| ----------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| [Supabase Official Website](https://supabase.com?utm_source=chatgpt.com)                                                            | Main Supabase platform               |
| [Supabase Kotlin Documentation](https://supabase.com/docs/reference/kotlin/introduction?utm_source=chatgpt.com)                     | Official Kotlin setup guide          |
| [Supabase Storage Documentation](https://supabase.com/docs/guides/storage?utm_source=chatgpt.com)                                   | Learn how cloud file storage works   |
| [Supabase Kotlin GitHub Repository](https://github.com/supabase-community/supabase-kt?utm_source=chatgpt.com)                       | Kotlin client library source code    |
| [Android Photo Picker Documentation](https://developer.android.com/training/data-storage/shared/photopicker?utm_source=chatgpt.com) | Official Android Photo Picker guide  |
| [Coil Compose Documentation](https://coil-kt.github.io/coil/compose/?utm_source=chatgpt.com)                                        | Displaying images in Jetpack Compose |
| [Ktor Client Documentation](https://ktor.io/docs/client-create-new-application.html?utm_source=chatgpt.com)                         | Android networking with Ktor         |

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
