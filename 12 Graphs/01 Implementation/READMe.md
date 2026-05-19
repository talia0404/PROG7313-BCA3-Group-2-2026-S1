# Activity: Interactive Graphs in Jetpack Compose 📊

## Scenario: CampusFit Tracker 🏋️‍♀️📱

You have been asked to design a Jetpack Compose app for a campus wellness programme called **CampusFit Tracker**.

Students use the app to record their weekly wellness activity. The app must help users understand their progress visually using an **interactive graph**.

Your graph must display **one** of the following datasets:

| Dataset Option | Information to Display                |
| -------------- | ------------------------------------- |
| Option 1       | Steps walked each day for 7 days      |
| Option 2       | Calories burned per workout session   |
| Option 3       | Water intake per day                  |
| Option 4       | Study breaks taken per day            |
| Option 5       | Gym attendance per week for one month |

---

## Your Task 🎯

You must decide:

1. **Which dataset you want to visualise**
2. **Which graph type is most suitable**

   * Line chart
   * Bar chart
   * Pie/donut chart
   * Custom chart
3. **Which graph option/library you want to use**

   * Vico
   * MPAndroidChart
   * KoalaPlot
   * Custom Compose Canvas
   * Other

---

## Part 1: Planning Your Graph 🧠

Answer these questions before coding:

| Question                                            | Your Answer |
| --------------------------------------------------- | ----------- |
| What data are you displaying?                       |             |
| Is the data showing change over time or comparison? |             |
| Which graph type is best?                           |             |
| Why did you choose that graph type?                 |             |
| Which graph library/option will you use?            |             |
| Why did you choose that option?                     |             |
| What interaction will your graph have?              |             |

Example interactions:

* Tap a graph value to show details 👆
* Filter by week/month 🎯
* Show a tooltip 💬
* Highlight the highest value ⭐
* Animate graph changes ✨

---

## Part 2: Design Your Screen 🎨

Your app screen must include:

| UI Component      | Purpose                                      |
| ----------------- | -------------------------------------------- |
| App title         | Shows the name of the app                    |
| Summary card      | Displays total, average, or highest value    |
| Graph section     | Displays the interactive graph               |
| Filter buttons    | Allows the user to change what data is shown |
| Short explanation | Explains what the graph shows                |

Example screen layout:

```text
CampusFit Tracker

Total Steps This Week: 48 000
Average Steps Per Day: 6 857

[ Interactive Graph Here ]

[ This Week ] [ Last Week ] [ All ]

This graph shows your daily steps and helps you track your activity progress.
```

---

## Part 3: Prepare Your Data 📋

Create sample data manually in your app.

Example:

| Day       | Steps |
| --------- | ----- |
| Monday    | 6000  |
| Tuesday   | 7500  |
| Wednesday | 5000  |
| Thursday  | 8200  |
| Friday    | 9000  |
| Saturday  | 7000  |
| Sunday    | 5300  |

You do **not** need a database for this activity.

Keep it simple. Hardcoded sample data is fine.

---

## Part 4: Implementation Instructions ⚙️

### Step 1: Create a new Jetpack Compose project

Create a new project in Android Studio.

Suggested project name:

```text
CampusFitGraphDemo
```

Use:

```text
Empty Activity
Kotlin
Jetpack Compose
```

---

### Step 2: Choose your graph option

Choose one:

| Option         | Recommended Use                                |
| -------------- | ---------------------------------------------- |
| Vico           | Best for modern Compose graphs                 |
| MPAndroidChart | Good for advanced charts but older style       |
| KoalaPlot      | Good for multiplatform Compose apps            |
| Canvas         | Good if you want to draw your own simple graph |

---

### Step 3: Add the required dependency

Go to your app-level Gradle file.

Add the dependency for the graph library you selected.

Then sync your project.

Do not continue until Gradle syncs successfully.

---

### Step 4: Create your data model

Create a simple structure to represent your graph data.

Your data should include:

| Field | Example |
| ----- | ------- |
| Label | Monday  |
| Value | 6000    |

For example, each item could represent:

* A day and step count
* A workout and calories burned
* A week and attendance count

---

### Step 5: Create your sample data

Inside your screen or ViewModel, create a list of sample values.

Example:

```text
Monday: 6000
Tuesday: 7500
Wednesday: 5000
Thursday: 8200
Friday: 9000
Saturday: 7000
Sunday: 5300
```

Your graph must have at least **5 data points**.

---

### Step 6: Build the screen layout

Your screen must include:

1. A heading
2. A short description
3. A summary card
4. The graph
5. At least two filter buttons

Example filter buttons:

```text
This Week
Last Week
```

or

```text
Steps
Water Intake
Calories
```

---

### Step 7: Display the graph

Use your chosen graph option to display your data.

Make sure your graph:

* Has clear labels
* Uses the correct graph type
* Shows the values clearly
* Fits nicely on the screen
* Is easy to understand

Keep it simple.

---

### Step 8: Add interactivity

Add at least **one** interactive feature.

Choose one:

| Interaction             | Description                       |
| ----------------------- | --------------------------------- |
| Tap value               | Shows exact value                 |
| Filter button           | Changes displayed data            |
| Highlight highest value | Makes the highest value stand out |
| Tooltip                 | Shows more details                |
| Animation               | Graph updates smoothly            |

---

### Step 9: Test your graph

Check that:

| Test                     | Done |
| ------------------------ | ---- |
| Graph displays correctly |      |
| Data values are accurate |      |
| Labels are readable      |      |
| Interaction works        |      |
| Screen looks neat        |      |
| App does not crash       |      |

---
