# 11. Reading and Writing JSON Files

## Saving and Reading JSON Data in Kotlin

### Step 1: Preparation

For example, the `Player` class is defined as follows:

```kotlin
data class Player(
    val id: Int,
    val number: Int,
    val name: String,
    val image_resource: Int = R.drawable.nhl_logo
)
```

We'll use the `Gson` library for JSON serialization and deserialization. Add the following dependency in the module's
`build.gradle` file directly in the dependencies section of the `app/build.gradle.kts` file:

```kotlin
implementation("com.google.code.gson:gson:2.10.1")
```

Another option offered by Android Studio is to convert this dependency to the Version Catalogs format. You can accept
this format change. The line above will be replaced by:

```kotlin
implementation(libs.gson)
```

And two lines will be added to `gradle/libs.versions.toml` for `gson`:

```toml
[versions]
gson = "2.10.1"

[libraries]
gson = { module = "com.google.code.gson:gson", version.ref = "gson" }
```

### Step 2: Saving Data to a JSON File

To save a list of `Player` to a JSON file, we'll create an extension function:

```kotlin
import com.google.gson.Gson
import java.io.File

fun List<Player>.saveToFile(filename: String) {
    val jsonString = Gson().toJson(this)
    File(filename).writeText(jsonString)
}
```

This function converts the list of `Player` to a JSON string, then writes it to a file.

### Step 3: Reading Data from a JSON File

To read the list of `Player` from a JSON file, we'll create another function:

```kotlin
import com.google.gson.Gson
import com.google.gson.reflect.TypeToken
import java.io.File

fun readPlayersFromFile(filename: String): List<Player> {
    val jsonString = File(filename).readText()
    val playerListType = object : TypeToken<List<Player>>() {}.type
    return Gson().fromJson(jsonString, playerListType)
}
```

This function reads the content of the JSON file, then converts it to a list of `Player`.

### Step 4: Using the Functions

Here's an example of using these functions:

```kotlin
fun main() {
    // Creating a list of players
    val players = listOf(
        Player(1, 99, "Wayne Gretzky"),
        Player(2, 87, "Sidney Crosby"),
        Player(3, 9, "Bobby Orr")
    )

    // Saving players to a JSON file
    players.saveToFile("players.json")
    println("Players saved to players.json")

    // Reading players from the JSON file
    val loadedPlayers = readPlayersFromFile("players.json")
    println("Players loaded from players.json:")
    loadedPlayers.forEach { println(it) }
}
```

### Important Points

1. **Serialization**: This is the process of converting objects to JSON format.
2. **Deserialization**: This is the reverse process, converting JSON to Kotlin objects.
3. **File Management**: Kotlin offers simple methods for reading and writing files.
4. **Gson Library**: It greatly simplifies working with JSON in Kotlin.
5. **Data Class**: Using `data class` for Player facilitates serialization/deserialization.

## File Management in Android

### Overwriting and Creating Files

1. **Overwriting Existing File**
   When using the `File(filename).writeText(jsonString)` method, if the specified file already exists, its content will
   be effectively overwritten. This operation completely replaces the previous content with new data.

2. **Creating a New File**
   If the specified file doesn't exist, Android will automatically create it before writing the data. This means you
   don't need to check for the file's existence or create it manually.

### File Location

The file location depends on the path you specify in the `filename` parameter. In the context of an Android application,
it's crucial to understand the different storage options:

1. **Internal Storage**
    - By default, if you use a relative path, the file will be created in your application's private directory.
    - Typical path: `/data/data/[your_app_package_name]/files/`
    - This directory is private and accessible only by your application.

2. **External Storage**
    - To write to external storage (like SD card), you must request appropriate permissions and use an absolute path (
      see another section).
    - Example: `Environment.getExternalStorageDirectory().absolutePath + "/MyFolder/myfile.json"`

### Best Practices for Android Applications

#### Using Application Context

For better file management in an Android application, it's recommended to use the application context to get the
appropriate directory:

```kotlin
fun Context.saveToFile(players: List<Player>, filename: String) {
    val file = File(this.filesDir, filename)
    file.writeText(Gson().toJson(players))
}

fun Context.readPlayersFromFile(filename: String): List<Player> {
    val file = File(this.filesDir, filename)
    val jsonString = file.readText()
    val playerListType = object : TypeToken<List<Player>>() {}.type
    return Gson().fromJson(jsonString, playerListType)
}
```

#### Error Handling

It's important to add error handling to handle cases where writing or reading might fail:

```kotlin
fun Context.saveToFile(players: List<Player>, filename: String) {
    try {
        val file = File(this.filesDir, filename)
        file.writeText(Gson().toJson(players))
    } catch (e: Exception) {
        Log.e("FileIO", "Error writing file", e)
    }
}
```

#### Checking File Existence

Before reading a file, it's wise to check its existence:

```kotlin
fun Context.readPlayersFromFile(filename: String): List<Player>? {
    val file = File(this.filesDir, filename)
    return if (file.exists()) {
        try {
            val jsonString = file.readText()
            val playerListType = object : TypeToken<List<Player>>() {}.type
            Gson().fromJson(jsonString, playerListType)
        } catch (e: Exception) {
            Log.e("FileIO", "Error reading file", e)
            emptyList()
        }
    } else {
        emptyList()
    }
}
```