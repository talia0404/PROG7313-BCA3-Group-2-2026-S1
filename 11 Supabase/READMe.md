# Supabase 🚀

Supabase is basically **Firebase’s open-source friend**, but with a proper **PostgreSQL database** underneath.

It gives you:

| Feature            | What it does                                               |
| ------------------ | ---------------------------------------------------------- |
| **Database**       | Stores app data in PostgreSQL tables                       |
| **Auth**           | Lets users register, log in, reset passwords, etc.         |
| **Storage**        | Stores files like images, PDFs, profile pictures, receipts |
| **Realtime**       | Lets apps update live when data changes                    |
| **Edge Functions** | Lets you run backend/server logic                          |
| **APIs**           | Automatically creates APIs for your database               |

Supabase’s free plan currently includes **1 GB file storage**, and extra storage is charged after the plan quota, so it works well as a Firebase Storage alternative for small apps. ([Supabase][1])

## What can you use it for? 🚀

Real-world examples:

| App type       | Supabase use                              |
| -------------- | ----------------------------------------- |
| Budget app     | Store expenses, categories, receipts      |
| Social app     | Store users, posts, profile images        |
| Booking app    | Store venues, bookings, uploaded images   |
| E-commerce app | Store products, users, product photos     |
| Student app    | Store assignments, PDFs, screenshots      |
| Travel app     | Store saved places, photos, user profiles |

For your case, you can use it like **Azure Blob Storage / Firebase Storage**, meaning:

```text
Android app → uploads image/PDF → Supabase Storage bucket → stores file URL/path in database
```

## How Supabase Storage works 🪣

Supabase uses **buckets**.

A bucket is like a folder/container for files.

Example buckets:

```text
profile-images
receipts
product-images
documents
pet-images
```

You can make buckets:

| Bucket type | Meaning                              |
| ----------- | ------------------------------------ |
| **Public**  | Anyone with the file URL can view it |
| **Private** | Only authorised users can access it  |

For demo apps, I’d usually use:

```text
receipts → private bucket
profile-images → public or private
product-images → public
```

## How to use it with Android Studio 📱

The basic process is:

### 1. Create a Supabase project

Go to Supabase, create a new project, then get:

```kotlin
SUPABASE_URL
SUPABASE_ANON_KEY
```

The official Kotlin quickstart explains Android/Kotlin setup for Supabase projects. ([Supabase][2])

### 2. Add Supabase dependencies

In Android Studio, add the Supabase Kotlin library modules you need.

Common ones:

```kotlin
Auth
Storage
Postgrest
```

> Supabase says the Kotlin client is community-maintained, not fully official, but it is documented by Supabase. ([Supabase][3])

### 3. Create a Supabase client

Example structure:

```kotlin
val supabase = createSupabaseClient(
    supabaseUrl = "YOUR_SUPABASE_URL",
    supabaseKey = "YOUR_SUPABASE_ANON_KEY"
) {
    install(Auth)
    install(Storage)
    install(Postgrest)
}
```

### 4. Use it for authentication 🔐

Example use cases:

```text
Register user
Log in user
Log out user
Check current user
Reset password
```

So instead of Firebase Auth, you can use Supabase Auth.

### 5. Use it for file storage 📂

Example real-world flow:

```text
User selects image from phone
App uploads image to Supabase Storage
Supabase returns file path
App saves that path in database
Later, app loads and displays the image
```

Example:

```kotlin
supabase.storage
    .from("receipts")
    .upload("user123/receipt1.jpg", imageBytes)
```

Then store this path in your database:

```text
user123/receipt1.jpg
```

## In a student project, it could work like this ✨

For a budgeting app:

| Feature                  | Supabase part      |
| ------------------------ | ------------------ |
| Register/login           | Supabase Auth      |
| Store expenses           | Supabase Database  |
| Upload receipt photo     | Supabase Storage   |
| View user’s own expenses | Row Level Security |
| Profile picture          | Supabase Storage   |
| Live dashboard updates   | Realtime           |

## Supabase vs Firebase

| Feature     | Firebase                     | Supabase                         |
| ----------- | ---------------------------- | -------------------------------- |
| Database    | Firestore/Realtime DB        | PostgreSQL                       |
| Auth        | Yes                          | Yes                              |
| Storage     | Paid/limited depending setup | Free tier includes storage quota |
| SQL support | No proper SQL                | Yes                              |
| Open source | No                           | Mostly yes                       |
| Best for    | Google ecosystem apps        | SQL/backend-style apps           |

> Supabase is a **Backend-as-a-Service** platform. Instead of building your own backend server, database, authentication system, and file storage manually, Supabase gives you those tools ready-made. In Android Studio, your Kotlin app can connect to Supabase to register users, save app data, and upload files like images or documents.

> In this module, we’ll mainly use it for **file storage**, similar to blob storage, because Firebase Storage is no longer the easiest free option. We can also use Supabase Auth if we want users to register and log in.

[1]: https://supabase.com/docs/guides/storage/pricing?utm_source=chatgpt.com "Pricing | Supabase Docs"
[2]: https://supabase.com/docs/guides/getting-started/quickstarts/kotlin?utm_source=chatgpt.com "Use Supabase with Android Kotlin"
[3]: https://supabase.com/docs/reference/kotlin/introduction?utm_source=chatgpt.com "Kotlin: Introduction | Supabase Docs"
