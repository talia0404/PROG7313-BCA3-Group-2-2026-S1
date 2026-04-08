# 03 🃏 Game Categories

In this part of the app, users must be able to create and manage **categories** for different gambling or gaming session types.

A category helps organise sessions properly so that each session belongs to a specific type, such as **Poker** or **Slots**. No floating rogue sessions allowed. Everything needs a home.

---

## 🎯 Goal of This Feature

You are going to add a new **Game Categories** section to the BetWise app.

This section must allow users to:

* view pre-created categories
* add their own custom categories
* optionally include a description for each category
* optionally add an image for a category
* tap on a category to open its details
* view all sessions linked to that category
* save all of this data using **RoomDB**

---

## A **category** is not the same as a **session**.

### A category:

is a type or group, such as:

* Slots
* Poker
* Roulette
* Blackjack
* Lucky Wheel

### A session:

is an individual play record created by the user, for example:

* “Played Poker for 2 hours”
* “Lost R150 on Slots”
* “Won R500 on Blackjack”

This means:

* one **category** can have **many sessions**
* each **session** must belong to **one category**

This is a **one-to-many relationship** in RoomDB.

---

# ✅ Functional Requirements

Your Game Categories feature must support the following:

## 1. Display pre-created categories 🎰

When the user opens the categories page for the first time, the app should already contain a few default categories, such as:

* Slots
* Poker
* Roulette
* Blackjack
* Lucky Wheel

These categories should be inserted into the database so the user has something to start with.

---

## 2. Allow the user to create their own categories ✍️

The user must be able to add a new category manually.

Each custom category should include:

* **Category name** → required
* **Description** → optional
* **Image** → optional

This means the user can create categories like:

* Sports Betting
* Bingo
* Scratch Cards
* Online Tournaments

---

## 3. Save everything in RoomDB 💾

All category data must be stored locally using **RoomDB**.

That includes:

* pre-created categories
* user-created categories
* descriptions
* image path or image reference
* session links to categories

Nothing should disappear when the app closes.

---

## 4. Show a list of all categories 📂

The categories screen should display all available categories in a clean list or card layout.

Each item should show useful summary information such as:

* category name
* maybe the image if one exists
* possibly a short preview of the description

This screen acts as the main entry point into category management.

---

## 5. Open a category details page when clicked 🔍

When a user taps on a category, they must be taken to a **Category Details** page.

This page should show:

* category name
* full description
* image if one was added
* all sessions that belong to that category

This helps the user understand what the category is for and review all related session history.

---

## 6. Show sessions linked to the selected category 📊

Inside the category details screen, the app should display any sessions created under that category.

For example:

### Poker category:

* Session 1: Lost R100
* Session 2: Won R350
* Session 3: Played for 1 hour 20 min

If no sessions exist yet, the app should show a friendly message such as:

* “No sessions created in this category yet.”

---

# 🗂️ Data Design Concept

You should think about the data in terms of **two connected tables/entities**:

## Category

Stores information about the category itself:

* id
* name
* description
* image path or image uri

## Session

Stores information about individual sessions:

* id
* session title or session info
* date
* duration
* win/loss amount
* categoryId

The important part is that the **Session** table must store a **categoryId** so RoomDB knows which category that session belongs to.

---
