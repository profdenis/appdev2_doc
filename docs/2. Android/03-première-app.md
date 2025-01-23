# 3. First Application with Jetpack Compose

Here's a simple example of a "Hello!" application using Jetpack Compose. I will present the code, then explain each
important element step by step.

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            Box(modifier = Modifier.fillMaxSize()) {
                Text(
                    text = "Hello!",
                    modifier = Modifier.align(Alignment.Center)
                )
            }
        }
    }
}
```

Now, let's explain the important elements of this application step by step:

## Explanation of Key Elements

### 1. Imports

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
```

- These imports are necessary to use Jetpack Compose classes and functions.

### 2. Main Activity Definition

```kotlin
class MainActivity : ComponentActivity() {
    // ...
}
```

- `MainActivity` inherits from `ComponentActivity`, which is the recommended base class for activities using Compose.

### 3. onCreate Method

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // ...
}
```

- This method is called when the activity is created.

### 4. setContent

```kotlin
setContent {
    // Compose content
}
```

- `setContent` is a Compose-specific function that defines the user interface content.
- Everything inside this function is Compose code.

### 5. Box Layout

```kotlin
Box(modifier = Modifier.fillMaxSize()) {
    // Box content
}
```

- `Box` is a layout component that allows stacking elements.
- `Modifier.fillMaxSize()` makes the Box occupy all available screen space.

### 6. Text Composable

```kotlin
Text(
    text = "Hello!",
    modifier = Modifier.align(Alignment.Center)
)
```

- `Text` is a Compose component for displaying text.
- `text = "Hello!"` defines the text to display.
- `Modifier.align(Alignment.Center)` centers the text in the Box.

## Key Points to Remember

1. **Composables**: In Compose, the user interface is built from composable functions (like `Text`).

2. **Declarative**: The code describes what should be displayed, not how to build it step by step.

3. **Modifiers**: `Modifier`s are used to customize component appearance and behavior.

4. **Hierarchy**: Components are organized hierarchically (here, `Text` is inside `Box`).

5. **No XML**: Unlike traditional Android development, there are no XML files to define the layout.

This simple application displays "Hello!" in the center of the screen. It's an ideal starting point to begin exploring
Jetpack Compose, as it illustrates the basic concepts without too much complexity.

## Defining Composable Functions

Here's the modified example with a composable function defined for the `Box`, and its call in `onCreate`. I will then
explain the modifications.

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.tooling.preview.Preview

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            HelloScreen()
        }
    }
}

@Composable
fun HelloScreen() {
    Box(modifier = Modifier.fillMaxSize()) {
        Text(
            text = "Hello World!",
            modifier = Modifier.align(Alignment.Center)
        )
    }
}

@Preview(showBackground = true)
@Composable
fun HelloPreview() {
    HelloScreen()
}
```

Let's now explain the modifications and their implications:

## Explanation of Changes

### 1. New `HelloScreen` Composable Function

```kotlin
@Composable
fun HelloScreen() {
    Box(modifier = Modifier.fillMaxSize()) {
        Text(
            text = "Hello World!",
            modifier = Modifier.align(Alignment.Center)
        )
    }
}
```

- This new function is annotated with `@Composable`, indicating it's a Jetpack Compose composable function.
- It encapsulates the display logic that was previously directly in `setContent`.
- This approach improves code reusability and readability.

### 2. Modification of `onCreate`

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent {
        HelloScreen()
    }
}
```

- Instead of defining the content directly in `setContent`, we now call our `HelloScreen` composable function.
- This makes the `onCreate` code more concise and clear.

### 3. Adding a Preview Function

```kotlin
@Preview(showBackground = true)
@Composable
fun HelloPreview() {
    HelloScreen()
}
```

- This new function is annotated with `@Preview`, which allows previewing the component in Android Studio without
  running the application.
- `showBackground = true` adds a background to the preview for better visibility.
- It calls `HelloScreen()`, allowing you to see exactly what will be displayed in the application.

## Implications and Benefits of These Changes

1. **Separation of Concerns**: Display logic is now separated from activity logic, which is a good programming practice.

2. **Reusability**: `HelloScreen` can be easily reused elsewhere in the application if needed.

3. **Testability**: It's easier to test `HelloScreen` in isolation.

4. **Preview**: Adding the preview function allows developers to quickly see interface changes without having to compile
   and run the entire application.

5. **Modularity**: This structure facilitates future addition of features or modifications to the user interface.

6. **Readability**: The code is clearer and easier to understand, with each part having a well-defined role.

7. **Scalability**: As the application grows, this structure makes it easy to add new components and screens.

This approach is typical of development with Jetpack Compose, where the user interface is built from reusable and
previewable components. It's a solid foundation for developing more complex applications while maintaining clean and
organized code.

