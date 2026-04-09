# 📸 What is a Photo Picker?

A **Photo Picker** is a **modern Android feature** that allows users to **select images or videos from their device** in a safe and simple way.

 Instead of your app digging through the user’s entire storage (a little invasive), Android gives you a **controlled system UI** where the user chooses what to share.

---

# 🧠 Why we use the Photo Picker

Using the Photo Picker is:

✅ **Safer** – users only share what they select

✅ **No storage permissions needed** 

✅ **Modern & recommended by Android**

✅ **Cleaner UX** – consistent gallery interface

---

# ⚡ Key Idea

> The Photo Picker opens a system screen -> user selects an image -> your app receives a **Uri** (location of the image)

---

# 📦 What is a Uri?

A **Uri** is basically:

> “Where the image lives on the device”

Example:

```kotlin
val imageUri: Uri? = ...
```

You don’t get the actual file — just a reference to it.

---

# 🚀 How to Implement 

We use:

 **Activity Result API**
**PickVisualMedia contract**

---

## 🪜 Step 1: Create a launcher

```kotlin
val photoPickerLauncher = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.PickVisualMedia()
) { uri ->
    // This runs AFTER the user selects an image

    if (uri != null) {
        // Image selected 
    } else {
        // User cancelled 
    }
}
```

---

## 🪜 Step 2: Launch the photo picker

```kotlin
photoPickerLauncher.launch(
    PickVisualMediaRequest(ActivityResultContracts.PickVisualMedia.ImageOnly)
)
```

> This opens the gallery-like picker UI.

---

## 🪜 Step 3: Store the selected image

```kotlin
var selectedImageUri by remember { mutableStateOf<Uri?>(null) }

val photoPickerLauncher = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.PickVisualMedia()
) { uri ->
    selectedImageUri = uri
}
```

---

## 🪜 Step 4: Display the image

```kotlin
if (selectedImageUri != null) {
    Image(
        painter = rememberAsyncImagePainter(selectedImageUri),
        contentDescription = "Selected image"
    )
}
```

👉 We use **Coil** to load the image from the Uri.

---

# 🎯 Flow 

1. User clicks button
2. Photo Picker opens
3. User selects image
4. Android returns a **Uri**
5. App stores Uri in state
6. UI updates automatically (Compose magic ✨)

---

# Important Notes:

### Old way 

```kotlin
Intent(Intent.ACTION_PICK)
```

👉 Requires storage permissions → outdated

---

###  Modern way 

```kotlin
ActivityResultContracts.PickVisualMedia()
```

👉 No permissions needed 
👉 Cleaner 
👉 Recommended 


> The Photo Picker lets users safely choose images from their device and gives your app a Uri to display them.

---
