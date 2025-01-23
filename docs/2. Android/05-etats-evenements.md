# 5. State and Event Management

## Example: InputExamples

Example on [GitHub](https://github.com/profdenis/ExemplesEntrees)

### `TextLengthCounter` Function

The `TextLengthCounter` function is a Jetpack Compose composable that creates a user interface allowing users to enter text and display its length in characters. It uses states to manage the entered text and its length, and dynamically updates the display.

Here's a detailed description of the function:

### Function Declaration:

```kotlin
@Composable
fun TextLengthCounter(modifier: Modifier = Modifier)
```
- It's a composable function that can receive a `Modifier` as a parameter.

### State Management:

```kotlin
var text by remember { mutableStateOf("") }
var length by remember { mutableIntStateOf(0) }
```
- Two state variables are created: `text` to store the entered text and `length` for the text length.
- `remember` is used to preserve these states between recompositions.

### Interface Structure:

```kotlin
Column(
    modifier = Modifier
        .padding(16.dp)
        .fillMaxWidth(),
    horizontalAlignment = Alignment.CenterHorizontally
) {
    // Column content
}
```

- A `Column` is used to organize interface elements vertically.
- A padding of 16dp is applied, and the column occupies all available width.
- Elements are horizontally centered.

### Input Field:

```kotlin
TextField(
    value = text,
    onValueChange = {
        text = it
        length = text.length
    },
    label = { Text("Enter text") },
    modifier = Modifier.fillMaxWidth()
)
```

- A `TextField` allows users to enter text.
- The field value is bound to the `text` state.
- With each change, `text` is updated and `length` is recalculated.

### Spacing:

```kotlin
Spacer(modifier = Modifier.height(16.dp))
```

- `Spacer`s are used to add vertical space between elements.

### Calculate Button:

```kotlin
Button(
    onClick = { length = text.length }
) {
    Text("Calculate length")
}
```

- A button allows manual recalculation of the text length.

### Length Display:

```kotlin
Text("Text length: $length characters")
```

- A `Text` displays the current text length.

This function illustrates several important Jetpack Compose concepts, including state management, component reactivity, and user interface organization. It shows how to create a simple but functional interactive interface.

## State Management with `remember` and `mutableStateOf`

In Jetpack Compose, state is a crucial concept. It represents any data that can change over time and, when changed, can trigger a recomposition of the user interface.

```kotlin
var text by remember { mutableStateOf("") }
var length by remember { mutableIntStateOf(0) }
```

### Role of `remember`

- `remember` is a function that allows preserving an object between recompositions.
- Without `remember`, each recomposition would create a new object, losing the previous state.
- `remember` "memorizes" the initial object and reuses it during subsequent recompositions.

### Function of `mutableStateOf`

- `mutableStateOf` creates a `MutableState<T>` object that encapsulates a mutable value.
- When this value changes, Compose is notified and can trigger a recomposition if necessary.
- `mutableIntStateOf` is a specialized version for integers, optimized for performance.

### Delegation with `by`

- The `by` keyword is used for property delegation in Kotlin.
- It allows using `text` and `length` directly as if they were normal variables, while benefiting from `MutableState` reactivity.

## Lambdas and State Updates

Lambdas are used to define behaviors in response to events, such as value changes or clicks.

### In TextField

```kotlin
onValueChange = {
    text = it
    length = text.length
}
```

- This lambda is called each time the `TextField` content changes.
- `it` represents the new text field value.
- The lambda updates `text` with the new value and immediately recalculates `length`.
- These updates trigger a recomposition, updating the user interface.

### In Button

```kotlin
onClick = { length = text.length }
```

- This lambda is simpler, called when the button is clicked.
- It recalculates `length` based on the current value of `text`.
- While this may seem redundant here (since `length` is already up to date), it could be useful in more complex scenarios.

### Reactivity and Data Flow

1. When the user types text, `onValueChange` is called.
2. `text` is updated, which notifies Compose of a state change.
3. `length` is also immediately updated.
4. Compose detects these state changes and triggers a recomposition.
5. During recomposition, the `TextField` displays the new text, and the `Text` below displays the new length.

This approach ensures that the user interface always stays synchronized with the application's internal state, providing a reactive and consistent user experience.

## FruitSelector Function

The `FruitSelector` function is a Jetpack Compose composable that creates a fruit selector as a dropdown menu. It allows users to choose a fruit from a predefined list and displays the selected fruit.

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FruitSelector(modifier: Modifier = Modifier) {
```

### Function Declaration
    - `@OptIn(ExperimentalMaterial3Api::class)` indicates the use of experimental Material 3 APIs.
    - It's a composable function that accepts an optional `modifier`.

```kotlin
    val fruits = listOf("Apple", "Banana", "Orange", "Strawberry", "Kiwi")
    var expanded by remember { mutableStateOf(false) }
    var selectedFruit by rememberSaveable { mutableStateOf("") }
```

### State Initialization

- `fruits` is a static list of fruits.
- `expanded` is a boolean state to control opening/closing the dropdown menu.
- `selectedFruit` is a state to store the selected fruit. `rememberSaveable` is used to preserve the selection even after a configuration change (like screen rotation).

```kotlin
    Column(modifier = modifier.padding(16.dp)) {
       ExposedDropdownMenuBox(
           expanded = expanded,
           onExpandedChange = { expanded = !expanded }
       ) {
```

### Interface Structure

- A `Column` contains all elements with 16dp padding.
- `ExposedDropdownMenuBox` is the main container for the dropdown menu.

```kotlin
            TextField(
                value = selectedFruit,
                onValueChange = {},
                readOnly = true,
                label = { Text("Choose a fruit") },
                trailingIcon = {
                    ExposedDropdownMenuDefaults
                        .TrailingIcon(expanded = expanded)
                },
                modifier = Modifier
                    .menuAnchor()
                    .fillMaxWidth()
            )
```

### Selector Text Field

- A `TextField` displays the selected fruit.
- It is read-only (`readOnly = true`).
- A dropdown icon is added on the right.
- `.menuAnchor()` links this field to the dropdown menu.

```kotlin
            ExposedDropdownMenu(
                expanded = expanded,
                onDismissRequest = { expanded = false }
            ) {
                fruits.forEach { fruit ->
                    DropdownMenuItem(
                        text = { Text(fruit) },
                        onClick = {
                            selectedFruit = fruit
                            expanded = false
                        }
                    )
                }
            }
```

### Dropdown Menu

- `ExposedDropdownMenu` contains the list of fruits.
- Each fruit is represented by a `DropdownMenuItem`.
- When clicking a fruit, `selectedFruit` is updated and the menu closes.

```kotlin
        Spacer(modifier = Modifier.height(16.dp))
        if (selectedFruit.isNotEmpty()) {
            Text("Selected fruit: $selectedFruit")
        }
    }
}
```

### Selection Display

- A vertical space is added.
- If a fruit is selected, it is displayed below the menu.

### Overall Operation

- The user sees a text field with the label "Choose a fruit".
- Clicking on the field opens a dropdown menu with the list of fruits.
- Selecting a fruit updates the `selectedFruit` state and closes the menu.
- The selected fruit is displayed in the text field and below it.

This function illustrates several advanced Jetpack Compose concepts:

- Using Material 3 components to create an interactive dropdown menu.
- State management with `remember` and `rememberSaveable`.
- Creating a dynamic and reactive user interface.
- Using lambdas to handle user interactions.
- Conditional display based on state (`if (selectedFruit.isNotEmpty())`).

