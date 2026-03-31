# Demo Project Instructions

## Jetpack Compose + RoomDB

## Topic: Grocery Store Product List

### Project Goal

Create a simple **Jetpack Compose Android app** that uses **Room Database** to store and display grocery products.

The app must demonstrate two core database actions:

1. **Write to the database**
   The user must be able to enter a grocery product and save it.

2. **Read from the database**
   The app must pull saved products from RoomDB and display them as labels on the screen.

---

# What the app should do

Your app will act like a very simple grocery store product manager.

The user should be able to:

* type a **product name**
* type a **category**
* type a **price**
* click **Add Product**
* save that product into RoomDB
* view the saved products on screen in a list of labels/cards/text items

Example products:

* Milk
* Bread
* Apples
* Rice
* Eggs

---

# App Requirements

## Functional Requirements

Your app must:

* use **Jetpack Compose** for the user interface
* use **RoomDB** as the local database
* use **one database table only**, called **Product**
* allow the user to enter product details
* save product details into the Room database
* retrieve all saved products from Room
* display saved products on the screen
* automatically update the UI when a new product is added

## Product Table Fields

Your **Product** table should include:

* **id** – primary key, auto-generated
* **name** – product name
* **category** – grocery category
* **price** – product price

Example:

| id | name   | category | price |
| -- | ------ | -------- | ----- |
| 1  | Milk   | Dairy    | 25.99 |
| 2  | Bread  | Bakery   | 18.50 |
| 3  | Apples | Fruit    | 30.00 |

