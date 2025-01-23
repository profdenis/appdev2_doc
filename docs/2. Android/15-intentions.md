# 15. Intents

[Git Repository](https://github.com/profdenis/Activites_Multiples)

## General Structure

The code defines a main activity (`MainActivity`) that contains two Composable components to start other activities.

### MainActivity

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Column {
                StartOtherActivity()
                StartOtherActivityWithValue()
            }
        }
    }
}
```

- This is the main activity that inherits from `ComponentActivity`
- In `onCreate`, it defines its content with two Composable components in a `Column`

### StartOtherActivity

```kotlin
@Composable
fun StartOtherActivity() {
    val context = LocalContext.current
    Column(
        modifier = Modifier.fillMaxWidth(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Main Screen")
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = {
            val intent = Intent(context, SecondActivity::class.java)
            context.startActivity(intent)
        }) {
            Text("Go to secondary screen")
        }
    }
}
```

- This component displays a title and a button
- When the button is clicked, it creates an `Intent` to start `SecondActivity`
- The `Intent` is an Android mechanism to start another activity

### StartOtherActivityWithValue

```kotlin
@Composable
fun StartOtherActivityWithValue() {
    val context = LocalContext.current
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Button(onClick = {
            val intent = Intent(context, ThirdActivity::class.java)
            intent.putExtra("buttonValue", 1)
            context.startActivity(intent)
        }) {
            Text("Button 1")
        }
        // Similar second button with value 2
    }
}
```

- This component displays two buttons
- Each button starts `ThirdActivity` passing a different value
- `putExtra` allows passing data to the next activity

## Important Points to Note

1. The use of `LocalContext.current` to get the context needed for creating `Intent`
2. The difference between a simple activity change and a change with data passing
3. Layout organization with `Column`, `Spacer`, and modifiers for alignment
4. The use of Jetpack Compose components (`Button`, `Text`, etc.)

Let me explain this code that represents a secondary activity in the application.

## SecondActivity Structure

### The SecondActivity Class

```kotlin
class SecondActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            SecondScreen(
                onNavigateBack = {
                    finish()
                }
            )
        }
    }
}
```

- This class inherits from `ComponentActivity`, like the main activity
- In `onCreate`, it defines its content with the `SecondScreen` component
- The `finish()` function is passed as a callback to handle returning to the previous screen
- `finish()` is an Android method that ends the current activity and returns to the previous activity

### The SecondScreen Component

```kotlin
@Composable
fun SecondScreen(onNavigateBack: () -> Unit) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Secondary Screen")
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = onNavigateBack) {
            Text("Return to main screen")
        }
    }
}
```

- The component takes an `onNavigateBack` parameter which is a lambda function with no parameters (`() -> Unit`)
- The interface is simple with:
    - A "Secondary Screen" title
    - A 16dp vertical space
    - A button to return to the main screen
- The layout uses a `Column` centered horizontally and vertically
- The `fillMaxSize()` modifier makes the column occupy all available space

## Important Points to Note

1. **Navigation Management**
    - Back navigation is properly handled with `finish()`
    - The callback is passed from activity level to Composable component

2. **Code Structure**
    - Clear separation between activity and user interface
    - Use of reusable components

3. **Best Practices**
    - The `SecondScreen` component is decoupled from the activity
    - Navigation is handled via callback rather than direct activity reference

4. **User Interface**
    - Simple and centered layout
    - Appropriate use of spacing
    - Clear and intuitive interface


## ThirdActivity Structure

### The ThirdActivity Class

```kotlin
class ThirdActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val buttonValue = intent.getIntExtra("buttonValue", 0)

        setContent {
            ThirdScreen(
                buttonValue = buttonValue,
                onNavigateBack = {
                    finish()
                }
            )
        }
    }
}
```

- This activity retrieves a value passed via the `Intent` with `getIntExtra`
- The second parameter `0` is the default value if no value is found
- The retrieved value is passed to the `ThirdScreen` component

### The ThirdScreen Component

```kotlin
@Composable
fun ThirdScreen(
    buttonValue: Int,
    onNavigateBack: () -> Unit
) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text(text = "Received value: $buttonValue")
        Spacer(modifier = Modifier.height(16.dp))
        Button(onClick = onNavigateBack) {
            Text("Back")
        }
    }
}
```

- The component takes two parameters:
    - `buttonValue`: the value received from the previous activity
    - `onNavigateBack`: the callback to return to the previous screen
- The interface displays the received value and a back button

## Important Points to Note

1. **Data Passing**
    - Use of `getIntExtra` to retrieve the passed data
    - Corresponds to the `putExtra` seen in the first file
    - Important to specify a default value

2. **Communication Cycle**
    - The main activity sends a value
    - The third activity retrieves it
    - The value is displayed on screen

3. **Architecture**
    - Clear separation between logic (data retrieval) and display
    - The Composable component remains pure and reusable

4. **String Template**
    - Use of `$buttonValue` to insert the value in the text
    - Simple example of string interpolation in Kotlin

This code complements the first two files well by showing how to:

- Receive data from another activity
- Display this data in the interface
- Maintain a clean and modular architecture
