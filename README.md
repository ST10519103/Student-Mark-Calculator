# Student Mark Calculator

## Overview

The Student Mark Calculator is a native Android application developed using **Kotlin** in **Android Studio**.

The application allows a user to enter a student’s details and three assessment marks:

* Test mark
* Assignment mark
* Exam mark

The app then:

* Validates the information entered
* Calculates the student’s average
* Determines a letter grade
* Determines whether the student passed or failed
* Opens a second screen to display the result
* Allows the result to be saved temporarily
* Displays previously saved results

The project was created to practise the fundamental concepts covered in **IMAD5112 – Introduction to Mobile Application Development**.

---

## Project Objectives

The main objectives of this project were to:

* Create a working native Android application
* Use Kotlin for application logic
* Use XML for interface design
* Collect input using `EditText`
* Display output using `TextView` and `Toast`
* Use variables and different data types
* Perform arithmetic calculations
* Use `if`, `else` and `when` statements
* Use logical operators
* Create and use arrays
* Use `while` and `for` loops
* Create reusable Kotlin functions
* Create a data class and objects
* Move between two Activities using an explicit Intent
* Send information between Activities using Intent extras
* Use Git and GitHub for version control
* Use GitHub Actions to automatically build the application

---

## Technologies Used

The project uses:

* Kotlin
* Android Studio
* XML
* Android SDK
* Gradle
* Git
* GitHub
* GitHub Actions
* Android Emulator

---

## Prerequisites

Before opening and running the project, make sure you have:

* Android Studio installed
* An Android emulator or physical Android device
* Git installed
* A GitHub account
* An internet connection for Gradle dependencies

---

## Project Creation Process

The project was created in Android Studio using the **Empty Views Activity** template.

The following steps were followed:

1. Opened Android Studio.
2. Selected **New Project**.
3. Selected **Empty Views Activity**.
4. Entered the project name.
5. Selected Kotlin as the programming language.
6. Allowed Android Studio to complete the initial Gradle sync.
7. Ran the default starter application.
8. Designed Screen 1 in `activity_main.xml`.
9. Created a second Activity named `MainActivity2`.
10. Designed Screen 2 in `activity_main2.xml`.
11. Connected XML elements to Kotlin using `findViewById`.
12. Added validation for empty fields and invalid marks.
13. Stored the three marks inside an array.
14. Used a loop to validate the marks.
15. Used a function to calculate the average.
16. Used a `when` expression to determine the grade.
17. Used `if` and `else` to determine pass or fail.
18. Created a `StudentResult` data class.
19. Used an Intent to open Screen 2.
20. Sent the student’s result to Screen 2 using Intent extras.
21. Added an `ArrayList` to save results temporarily.
22. Used a loop to display saved results.
23. Tested the application using an Android emulator.
24. Uploaded the project to GitHub.
25. Added a GitHub Actions workflow.

---

## Project Structure

The main project files are:

```text
app/
├── src/
│   └── main/
│       ├── java/com/example/student_mark_calculator/
│       │   ├── MainActivity.kt
│       │   └── MainActivity2.kt
│       │
│       ├── res/
│       │   ├── layout/
│       │   │   ├── activity_main.xml
│       │   │   └── activity_main2.xml
│       │   │
│       │   └── values/
│       │       ├── colors.xml
│       │       ├── strings.xml
│       │       └── themes.xml
│       │
│       └── AndroidManifest.xml
│
├── build.gradle.kts
└── gradle/
    └── libs.versions.toml
```

---

# Screen 1: Student Mark Entry

Screen 1 is controlled by:

```text
MainActivity.kt
```

Its interface is stored in:

```text
activity_main.xml
```

The screen allows the user to enter:

* Student name
* Subject name
* Test mark
* Assignment mark
* Exam mark

It also contains:

* Calculate Result button
* Clear Fields button

---

## Screen 1 Interface

The first screen uses:

* `ScrollView`
* `LinearLayout`
* `TextView`
* `EditText`
* `Button`

A `ScrollView` was used so that the screen can be used on smaller devices.

### Screen 1 Screenshot

Replace the placeholder below with your uploaded GitHub screenshot URL:

```html
<img width="350" alt="Student Mark Calculator Main Screen" src="PASTE_SCREEN_1_SCREENSHOT_URL_HERE" />
```

---

## Connecting XML to Kotlin

The controls from `activity_main.xml` are connected to Kotlin using `findViewById`.

Example:

```kotlin
val studentNameInput =
    findViewById<EditText>(R.id.student_name_input)

val calculateButton =
    findViewById<Button>(R.id.calculate_button)
```

The XML ID:

```xml
android:id="@+id/student_name_input"
```

must match the Kotlin reference:

```kotlin
R.id.student_name_input
```

Kotlin is case-sensitive, so the spelling must match exactly.

---

# Reading User Input

The values are read from the `EditText` fields:

```kotlin
val studentName =
    studentNameInput.text.toString().trim()

val subjectName =
    subjectInput.text.toString().trim()

val testMarkText =
    testMarkInput.text.toString().trim()

val assignmentMarkText =
    assignmentMarkInput.text.toString().trim()

val examMarkText =
    examMarkInput.text.toString().trim()
```

### Explanation

* `.text` gets the content entered into the field.
* `.toString()` converts it into a normal Kotlin String.
* `.trim()` removes unnecessary spaces.

---

# Input Validation

The application checks whether any required field is empty:

```kotlin
if (
    studentName.isBlank() ||
    subjectName.isBlank() ||
    testMarkText.isBlank() ||
    assignmentMarkText.isBlank() ||
    examMarkText.isBlank()
) {
    Toast.makeText(
        this,
        "Please complete all the fields.",
        Toast.LENGTH_SHORT
    ).show()

    return@setOnClickListener
}
```

The logical OR operator:

```kotlin
||
```

means that the condition becomes true when at least one field is blank.

---

## Number Conversion

Marks entered into `EditText` controls are originally text.

The application converts them into `Double` values:

```kotlin
val testMark =
    testMarkText.toDoubleOrNull()

val assignmentMark =
    assignmentMarkText.toDoubleOrNull()

val examMark =
    examMarkText.toDoubleOrNull()
```

`toDoubleOrNull()` was used because it safely returns `null` when conversion fails.

Example:

```text
"75"    → 75.0
"80.5"  → 80.5
"hello" → null
```

The app checks for invalid conversions:

```kotlin
if (
    testMark == null ||
    assignmentMark == null ||
    examMark == null
) {
    Toast.makeText(
        this,
        "Marks must contain valid numbers.",
        Toast.LENGTH_SHORT
    ).show()

    return@setOnClickListener
}
```

---

# Arrays

The three marks are stored inside an array:

```kotlin
val marks: Array<Double> = arrayOf(
    testMark,
    assignmentMark,
    examMark
)
```

The array contains:

```text
Index 0 → Test mark
Index 1 → Assignment mark
Index 2 → Exam mark
```

Arrays start at index `0`.

---

## Validating Marks with a While Loop

The application checks that all marks are between `0` and `100`.

```kotlin
private fun marksAreValid(
    marks: Array<Double>
): Boolean {

    var index = 0

    while (index < marks.size) {

        val currentMark = marks[index]

        if (currentMark < 0 || currentMark > 100) {
            return false
        }

        index++
    }

    return true
}
```

### How the loop works

1. `index` starts at `0`.
2. The mark at the current index is checked.
3. If the mark is below `0` or above `100`, the function returns `false`.
4. `index++` moves to the next mark.
5. The loop stops after all marks have been checked.
6. The function returns `true` if every mark is valid.

The condition:

```kotlin
currentMark < 0 || currentMark > 100
```

means:

> The mark is invalid when it is below zero OR above one hundred.

---

# Calculating the Average

The average is calculated inside a reusable function:

```kotlin
private fun calculateAverage(
    marks: Array<Double>
): Double {

    var total = 0.0

    for (mark in marks) {
        total += mark
    }

    return total / marks.size
}
```

The `for` loop visits every mark in the array.

Example:

```text
Test mark       = 70
Assignment mark = 80
Exam mark       = 60
```

Calculation:

```text
70 + 80 + 60 = 210
210 ÷ 3 = 70
```

The average is:

```text
70%
```

---

# Determining the Grade

A `when` expression is used to determine the letter grade:

```kotlin
private fun calculateGrade(
    average: Double
): String {

    return when {
        average >= 80 -> "A"
        average >= 70 -> "B"
        average >= 60 -> "C"
        average >= 50 -> "D"
        else -> "F"
    }
}
```

The conditions are checked from top to bottom.

For an average of `74`:

```text
74 >= 80 → false
74 >= 70 → true
```

The grade is therefore:

```text
B
```

---

# Determining Pass or Fail

The status is calculated using `if` and `else`:

```kotlin
private fun calculateStatus(
    average: Double
): String {

    return if (average >= 50) {
        "PASS"
    } else {
        "FAIL"
    }
}
```

The student passes when the average is `50` or higher.

---

# StudentResult Data Class

The application uses a data class to store one complete student result:

```kotlin
data class StudentResult(
    val studentName: String,
    val subjectName: String,
    val testMark: Double,
    val assignmentMark: Double,
    val examMark: Double,
    val average: Double,
    val grade: String,
    val status: String
)
```

A class is a blueprint.

An object is created from the class:

```kotlin
val studentResult = StudentResult(
    studentName = studentName,
    subjectName = subjectName,
    testMark = testMark,
    assignmentMark = assignmentMark,
    examMark = examMark,
    average = average,
    grade = grade,
    status = status
)
```

The object groups all the information belonging to one student result.

---

# Opening Screen 2

An explicit Intent is used to open `MainActivity2`:

```kotlin
val intent = Intent(
    this,
    MainActivity2::class.java
)
```

### Explanation

```kotlin
this
```

refers to the current Activity.

```kotlin
MainActivity2::class.java
```

identifies the destination Activity.

The `.java` part is normal Kotlin Android syntax. It does not mean the project is written in Java.

---

## Sending Information to Screen 2

The result is sent using Intent extras:

```kotlin
intent.putExtra(
    "student_name",
    studentResult.studentName
)

intent.putExtra(
    "subject_name",
    studentResult.subjectName
)

intent.putExtra(
    "test_mark",
    studentResult.testMark
)

intent.putExtra(
    "assignment_mark",
    studentResult.assignmentMark
)

intent.putExtra(
    "exam_mark",
    studentResult.examMark
)

intent.putExtra(
    "average",
    studentResult.average
)

intent.putExtra(
    "grade",
    studentResult.grade
)

intent.putExtra(
    "status",
    studentResult.status
)
```

The second screen is opened using:

```kotlin
startActivity(intent)
```

---

# Clear Fields Button

The Clear button removes all entered information:

```kotlin
clearButton.setOnClickListener {

    studentNameInput.text.clear()
    subjectInput.text.clear()
    testMarkInput.text.clear()
    assignmentMarkInput.text.clear()
    examMarkInput.text.clear()

    studentNameInput.requestFocus()

    Toast.makeText(
        this,
        "All fields have been cleared.",
        Toast.LENGTH_SHORT
    ).show()
}
```

`requestFocus()` places the typing cursor back into the first input field.

---

# Screen 2: Student Result

Screen 2 is controlled by:

```text
MainActivity2.kt
```

Its interface is stored in:

```text
activity_main2.xml
```

Screen 2 displays:

* Student name
* Subject name
* Test mark
* Assignment mark
* Exam mark
* Average
* Grade
* Pass or fail status
* Saved results
* Save Result button
* Back button

### Screen 2 Screenshot

Replace the placeholder below with your uploaded screenshot URL:

```html
<img width="350" alt="Student Result Screen" src="PASTE_SCREEN_2_SCREENSHOT_URL_HERE" />
```

---

# Receiving Intent Information

Screen 2 receives the values using the same keys used by Screen 1.

```kotlin
val studentName =
    intent.getStringExtra("student_name")
        ?: "Unknown student"

val subjectName =
    intent.getStringExtra("subject_name")
        ?: "Unknown subject"
```

Numeric values are received using:

```kotlin
val testMark =
    intent.getDoubleExtra("test_mark", 0.0)

val assignmentMark =
    intent.getDoubleExtra("assignment_mark", 0.0)

val examMark =
    intent.getDoubleExtra("exam_mark", 0.0)

val average =
    intent.getDoubleExtra("average", 0.0)
```

The grade and status are received using:

```kotlin
val grade =
    intent.getStringExtra("grade") ?: "F"

val status =
    intent.getStringExtra("status") ?: "FAIL"
```

---

## Elvis Operator

The operator:

```kotlin
?:
```

is called the Elvis operator.

Example:

```kotlin
val studentName =
    intent.getStringExtra("student_name")
        ?: "Unknown student"
```

This means:

> Use the received student name. If it is null, use `"Unknown student"`.

---

# Displaying the Result

The received values are displayed inside `TextView` controls.

Example:

```kotlin
studentText.text =
    "Student: ${result.studentName}"

subjectText.text =
    "Subject: ${result.subjectName}"

averageText.text =
    "Average: ${formatMark(result.average)}%"

gradeText.text =
    "Grade: ${result.grade}"

statusText.text =
    "Status: ${result.status}"
```

The application uses String templates:

```kotlin
${result.studentName}
```

to place variable values inside text.

---

# Formatting Decimal Values

Marks are displayed using two decimal places:

```kotlin
private fun formatMark(
    mark: Double
): String {

    return String.format(
        Locale.getDefault(),
        "%.2f",
        mark
    )
}
```

Examples:

```text
70.0   → 70.00
72.456 → 72.46
```

---

# Grade Colour

A `when` expression changes the grade colour:

```kotlin
val gradeColour = when (grade) {
    "A" -> Color.parseColor("#15803D")
    "B" -> Color.parseColor("#2563EB")
    "C" -> Color.parseColor("#7C3AED")
    "D" -> Color.parseColor("#D97706")
    else -> Color.parseColor("#DC2626")
}
```

The selected colour is applied using:

```kotlin
gradeText.setTextColor(gradeColour)
```

---

# Status Colour

The pass or fail colour is selected using `if` and `else`:

```kotlin
if (status == "PASS") {

    statusText.setTextColor(
        Color.parseColor("#15803D")
    )

} else {

    statusText.setTextColor(
        Color.parseColor("#DC2626")
    )
}
```

A passing result is displayed in green.

A failing result is displayed in red.

---

# Saving Results

Screen 2 contains an `ArrayList`:

```kotlin
private val savedResults =
    arrayListOf<StudentResult>()
```

An `ArrayList` is similar to an array, but it can grow when new objects are added.

The result is saved using:

```kotlin
savedResults.add(result)
```

The application also checks for duplicate results:

```kotlin
if (savedResults.contains(result)) {
    // Result already exists
}
```

Because `StudentResult` is a data class, Kotlin can compare its properties automatically.

---

# Displaying Saved Results

A `while` loop displays every result inside the `ArrayList`:

```kotlin
var index = 0

while (index < savedResults.size) {

    val result =
        savedResults[index]

    resultsBuilder.append(
        "${index + 1}. ${result.studentName}\n"
    )

    resultsBuilder.append(
        "Subject: ${result.subjectName}\n"
    )

    resultsBuilder.append(
        "Average: ${formatMark(result.average)}%\n"
    )

    resultsBuilder.append(
        "Grade: ${result.grade}\n"
    )

    resultsBuilder.append(
        "Status: ${result.status}"
    )

    index++
}
```

`index++` is important because it moves the loop to the next item.

Without it, the loop would never end.

---

# Returning to Screen 1

The Back button uses:

```kotlin
backButton.setOnClickListener {
    finish()
}
```

`finish()` closes only Screen 2.

Screen 1 remains underneath it in the Android back stack.

---

# AndroidManifest Configuration

The second Activity must be registered inside `AndroidManifest.xml`:

```xml
<activity
    android:name=".MainActivity2"
    android:exported="false" />
```

The starting Activity contains:

```xml
<activity
    android:name=".MainActivity"
    android:exported="true">

    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>

</activity>
```

`MAIN` identifies the starting Activity.

`LAUNCHER` allows the Activity to open when the user selects the application icon.

---

# Application Flow

```text
Application starts
        ↓
MainActivity opens
        ↓
User enters student information
        ↓
User enters three marks
        ↓
User presses Calculate Result
        ↓
The application checks for empty fields
        ↓
Text marks are converted into Double values
        ↓
Marks are stored inside an array
        ↓
A while loop validates the marks
        ↓
A for loop calculates the total
        ↓
A function calculates the average
        ↓
A when expression determines the grade
        ↓
An if/else expression determines PASS or FAIL
        ↓
A StudentResult object is created
        ↓
An Intent sends the result to MainActivity2
        ↓
Screen 2 displays the result
        ↓
The user can save the result
        ↓
A while loop displays saved results
        ↓
The user presses Back
        ↓
MainActivity is displayed again
```

---

# Kotlin Concepts Used

The project demonstrates the following concepts.

## Variables

```kotlin
val studentName = "Sarah"
var index = 0
```

`val` is used when a variable is not reassigned.

`var` is used when a value must change.

---

## Data Types

The application uses:

* `String`
* `Int`
* `Double`
* `Boolean`
* `Array<Double>`
* `ArrayList<StudentResult>`

---

## Arithmetic Operators

The application uses:

```kotlin
+
/
+=
```

Example:

```kotlin
total += mark
```

---

## Comparison Operators

The application uses:

```kotlin
<
>
>=
<=
==
```

Example:

```kotlin
average >= 50
```

---

## Logical Operators

The application uses:

```text
||  OR
!   NOT
```

Example:

```kotlin
if (currentMark < 0 || currentMark > 100)
```

---

## Decisions

The application uses:

* `if`
* `else`
* `when`

---

## Arrays

The three marks are stored inside:

```kotlin
Array<Double>
```

---

## Loops

The project uses:

* `while` loop for validation
* `for` loop for calculating the total
* `while` loop for displaying saved results

---

## Functions

Functions used in the project include:

```kotlin
marksAreValid()
calculateAverage()
calculateGrade()
calculateStatus()
displayCurrentResult()
formatMark()
setGradeColour()
setStatusColour()
saveStudentResult()
displaySavedResults()
```

---

## Object-Oriented Programming

The two Activities are classes:

```kotlin
class MainActivity : AppCompatActivity()
```

```kotlin
class MainActivity2 : AppCompatActivity()
```

The student result is represented by:

```kotlin
data class StudentResult(...)
```

---

## Functional Programming

Several functions receive values and return new values.

Example:

```kotlin
private fun calculateGrade(
    average: Double
): String
```

The function receives a `Double` and returns a `String`.

---

# Error Handling

The application handles:

* Empty student name
* Empty subject name
* Empty mark fields
* Invalid numeric input
* Marks below zero
* Marks above one hundred
* Missing Intent values
* Duplicate saved results
* Empty saved-results list

Toast messages provide feedback to the user.

---

# Testing

The application was tested using an Android emulator.

| Test                       | Expected result                 |
| -------------------------- | ------------------------------- |
| Enter valid information    | Result screen opens             |
| Leave a field blank        | Error Toast is displayed        |
| Enter text as a mark       | Invalid number message appears  |
| Enter a mark below 0       | Validation error appears        |
| Enter a mark above 100     | Validation error appears        |
| Enter average above 50     | Status is PASS                  |
| Enter average below 50     | Status is FAIL                  |
| Save a result              | Result appears in Saved Results |
| Save the same result twice | Duplicate is prevented          |
| Press Back                 | User returns to Screen 1        |
| Press Clear                | All fields are cleared          |

---

# GitHub Usage

GitHub was used to store and manage the project.

The process included:

1. Creating a GitHub repository.
2. Connecting the Android Studio project to Git.
3. Committing project changes.
4. Pushing updates to the `main` branch.
5. Tracking the history of the project.
6. Using meaningful commit messages.

Example commit messages:

```text
Create Student Mark Calculator interface
```

```text
Add mark calculation logic
```

```text
Add result Activity
```

```text
Add validation and grading
```

```text
Add saved results feature
```

```text
Fix AndroidX Core dependency
```

---

# GitHub Actions

GitHub Actions was configured to build the Android project automatically.

The workflow file is stored inside:

```text
.github/workflows/android-ci.yml
```

The workflow:

* Runs when code is pushed to the `main` branch
* Downloads the project
* Installs Java 17
* Prepares Gradle
* Runs unit tests
* Builds the debug APK
* Uploads the APK as an artifact

### GitHub Actions Screenshot

Replace the placeholder below:

<img width="1456" height="877" alt="image" src="https://github.com/user-attachments/assets/d0ef7674-9254-4a81-ada5-0c2c4110ab80" />


# Dependency Compatibility

During development, the project initially used:

```toml
coreKtx = "1.19.0"
```

This dependency required newer Android build tools.

The project was using:

```text
Android Gradle Plugin 9.0.1
compileSdk 36
```

The dependency was therefore changed to:

```toml
coreKtx = "1.17.0"
```

After changing `libs.versions.toml`, the project was synchronised with Gradle and rebuilt successfully.

---

# Debugging

Android Studio was used to identify and correct issues such as:

* Incorrect package names
* Missing Activity classes
* Incorrect XML IDs
* Unresolved `R` references
* Missing Manifest entries
* Incorrect Intent destinations
* Gradle dependency incompatibility
* Unsafe number conversion
* Running the unit-test configuration instead of the app configuration

The following Android Studio tools were useful:

* Problems panel
* Build Output
* Logcat
* Gradle Sync
* Clean Project
* Rebuild Project
* Android Emulator

---

# Current Limitations

The saved results are stored only in memory.

This means:

* Results remain available while the application process is running.
* Results may disappear when the app is fully closed.
* No database is used.
* No permanent file storage is used.

This was intentional because the project focuses on beginner Kotlin concepts.

---

# Future Improvements

Possible improvements include:

* Add permanent storage using Room
* Add SharedPreferences
* Allow users to delete saved results
* Allow users to edit a result
* Add weighted assessments
* Add more subjects
* Add class-average calculations
* Add a student search feature
* Use RecyclerView for saved results
* Add a Clear Saved Results button
* Add more unit tests
* Improve the user interface
* Create a signed release APK
* Publish the application

---

# Demo Video

Add the demonstration video link here:



The video should demonstrate:

* Screen 1
* Entering student information
* Entering marks
* Validation errors
* Calculating a result
* Screen 2 output
* Saving a result
* Displaying saved results
* Returning to Screen 1
* Clearing fields

---

# Conclusion

The Student Mark Calculator demonstrates how to create a native Android application using Kotlin and XML.

The project applies important programming concepts, including:

* Variables
* Data types
* Input and output
* Arithmetic operators
* Comparison operators
* Logical operators
* Decisions
* Arrays
* Loops
* Functions
* Classes
* Objects
* Activities
* Intents
* Validation
* Android interface design
* GitHub version control
* GitHub Actions

The application provides a practical example of how user input can be processed, calculated, stored and displayed across multiple Android screens.

---

# Author

**Student Name:** Add your full name
**Student Number:** Add your student number
**Module:** IMAD5112 – Introduction to Mobile Application Development
**Project:** Student Mark Calculator

---

# References

- Goodtal (2023). Everything You Need to Know About Kotlin. [online] Goodtal.com. Available at: https://www.goodtal.com/app-developer/blog/everything-you-need-to-know-about-kotlin [Accessed 30 March. 2026].
- joash (2024). Comprehensive Guide to Kotlin Programming. [online] DEV Community. Available at: https://dev.to/javadevone/comprehensive-guide-to-kotlin-programming-40kj [Accessed 30 March. 2026].
- Kotlin Help. (n.d.). Conditions and loops | Kotlin. [online] Available at: https://kotlinlang.org/docs/control-flow.html [Accessed 30 March. 20226].
- Learn (2021). Learn Kotlin. [online] Codecademy. Available at: https://www.codecademy.com/learn/learn-kotlin?g_network=o&g_productchannel=&g_adid=&g_locinterest=&g_keyword=learn%20kotlin&g_acctid=243-039-7011&g_adtype=&g_keywordid=kwd-79715182650931:loc-168&g_ifcreative=&g_campaign=account&g_locphysical=137792&g_adgroupid=1275434296087406&g_productid=&g_source= [Accessed 30 March. 2026].
- www.w3schools.com. (n.d.). Kotlin If ... Else Expressions. [online] Available at: https://www.w3schools.com/KOTLIN/kotlin_conditions.php [Accessed 30 Mar. 2026].
- Hexley21 (2020). How to make long clickable clear button in Kotlin Android Studio. [online] Stack Overflow. Available at: https://stackoverflow.com/questions/63801950/how-to-make-long-clickable-clear-button-in-kotlin-android-studio [Accessed 29 March. 2026].
- Bing. (2018). exit button kotline - Bing. [online] Available at: https://www.bing.com/search?pglt=299&q=exit+button+kotline&cvid=5c05d9f5e0994bff9edbc7f78e018042&gs_lcrp=EgRlZGdlKgYIABBFGDkyBggAEEUYOdIBCDc4ODFqMGoxqAIIsAIB&FORM=ANNTA1&PC=HCTS [Accessed 30 March. 2026].
- Chatterjee, A. (2020). Baeldung. [online] Baeldung on Kotlin. Available at: https://www.baeldung.com/kotlin/lists [Accessed 30 March. 2026].
- mr kingcade (exit button )
- ANIL (2025). Control Flow in Kotlin: if, when, loops, ranges (2025 Guide). [online] Medium. Available at: https://medium.com/@anilkumar2681/control-flow-in-kotlin-if-when-loops-ranges-2025-guide-a329e0830054 [Accessed 10 June. 2026].
- Appframework.org. (2026). Activities | Android Developers. [online] Available at: https://www.appframework.org/android_sdk_docs/docs/guide/components/activities.html [Accessed 10 June. 2026].
-CodeSignal Learn. (2024). Conditional Statements and Loop Control in Kotlin. [online] Available at: https://codesignal.com/learn/courses/revisiting-kotlin-basics/lessons/conditional-statements-and-loop-control-in-kotlin [Accessed 10 Jun. 2026].
-GeeksforGeeks (2019a). Android UI Layouts. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/android/android-ui-layouts/ [Accessed 10 June. 2026].
- GeeksforGeeks (2019b). Implicit and Explicit Intents in Android with Examples. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/android/implicit-and-explicit-intents-in-android-with-examples/ [Accessed 10 June. 2026].
- -GeeksforGeeks (2019c). Kotlin Array. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/kotlin/kotlin-array/ [Accessed 10 June. 2026].
- GeeksforGeeks (2019d). Kotlin Class and Objects. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/kotlin/kotlin-class-and-objects/ [Accessed 10 June. 2026].
- GeeksforGeeks (2019e). Kotlin Data Classes. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/kotlin/kotlin-data-classes/ [Accessed 10 June. 2026].
- GeeksforGeeks (2019f). Kotlin functions. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/kotlin/kotlin-functions/ [Accessed 10 June. 2026].
- GeeksforGeeks (2019g). Kotlin Null Safety. [online] GeeksforGeeks. Available at: https://www.geeksforgeeks.org/kotlin/kotlin-null-safety/ [Accessed 10 June. 2026].


- 
