# 9. State and Event Management

## Example: InputExamples

Example on [GitHub](https://github.com/profdenis/ExemplesEntrees)

### First Version

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FruitSelector(modifier: Modifier = Modifier) {
    val fruits = listOf("Apple", "Banana", "Orange", "Strawberry", "Kiwi")
    var expanded by remember { mutableStateOf(false) }
    var selectedFruit by remember { mutableStateOf("") }

    Column(modifier = modifier.padding(16.dp)) {
        ExposedDropdownMenuBox(
            expanded = expanded,
            onExpandedChange = { expanded = !expanded }
        ) {
            TextField(
                value = selectedFruit,
                onValueChange = {},
                readOnly = true,
                label = { Text("Choose a fruit") },
                trailingIcon = { ExposedDropdownMenuDefaults.TrailingIcon(expanded = expanded) },
                modifier = Modifier
                    .menuAnchor()
                    .fillMaxWidth()
            )

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
        }

        Spacer(modifier = Modifier.height(16.dp))

        if (selectedFruit.isNotEmpty()) {
            Text("Selected fruit: $selectedFruit")
        }
    }
}

@Composable
fun App(modifier: Modifier = Modifier) {
    Column(modifier = modifier) {
        TextLengthCounter()
        Spacer(modifier = Modifier.height(100.dp))
        FruitSelector()
    }
}
```

#### General Description

1. `FruitSelector`: This is a composable function that creates a fruit selector in the form of a dropdown menu.

2. `App`: This is the main composable function that structures the application's user interface by combining different
   components.

#### Focus on FruitSelector

The `FruitSelector` function is a Jetpack Compose component that creates a dropdown menu allowing users to select a
fruit from a predefined list. Here are its main features:

1. **Fruit List**: A static list of fruits is defined at the beginning of the function.

2. **States**:
    - `expanded`: A boolean state that controls whether the dropdown menu is open or closed.
    - `selectedFruit`: A state that stores the currently selected fruit.

3. **User Interface**:
    - Uses `ExposedDropdownMenuBox` to create the dropdown menu container.
    - Displays a `TextField` that serves as a trigger to open the menu. This field is read-only and displays the
      selected fruit.
    - The dropdown menu (`ExposedDropdownMenu`) contains the list of fruits, each represented by a `DropdownMenuItem`.

4. **Interaction**:
    - When a fruit is selected, the menu closes and the chosen fruit is displayed in the TextField.
    - Additional text appears below to confirm the selected fruit.

5. **Layout**:
    - Uses a `Column` to vertically organize elements.
    - Adds spacing and padding to improve appearance.

6. **Customization**:
    - Accepts a `modifier` parameter to allow additional customization if needed.

This function demonstrates the use of several important Jetpack Compose concepts, such as state management, Material 3
UI components, and the creation of interactive components. It provides an intuitive user interface for selecting items
from a list, which is a common functionality in many applications.

### Second Version

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FruitSelector(
    modifier: Modifier = Modifier,
    fruits: List<String>,
    selectedFruit: String,
    onFruitSelected: (String) -> Unit = {}
) {
    var expanded by remember { mutableStateOf(false) }

    Column(modifier = modifier.padding(16.dp)) {
        ExposedDropdownMenuBox(
            expanded = expanded,
            onExpandedChange = { expanded = !expanded }
        ) {
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

            ExposedDropdownMenu(
                expanded = expanded,
                onDismissRequest = { expanded = false }
            ) {
                fruits.forEach { fruit ->
                    DropdownMenuItem(
                        text = { Text(fruit) },
                        onClick = {
                            onFruitSelected(fruit)
                            expanded = false
                        }
                    )
                }
            }
        }

        Spacer(modifier = Modifier.height(16.dp))

        if (selectedFruit.isNotEmpty()) {
            Text("Selected fruit: $selectedFruit")
        }
    }
}

@Composable
fun App(modifier: Modifier = Modifier) {
    var selectedFruit1 by rememberSaveable { mutableStateOf("") }
    var selectedFruit2 by rememberSaveable { mutableStateOf("") }

    Column(modifier = modifier) {
        TextLengthCounter()
        Spacer(modifier = Modifier.height(100.dp))
        FruitSelector(
            fruits = listOf("Apple", "Banana", "Orange", "Strawberry", "Kiwi"),
            selectedFruit = selectedFruit1,
            onFruitSelected = { fruit -> selectedFruit1 = fruit }
        )
        Spacer(modifier = Modifier.height(10.dp))
        FruitSelector(
            fruits = listOf("Pear", "Mango", "Orange", "Blueberry", "Grapefruit"),
            selectedFruit = selectedFruit2,
            onFruitSelected = { fruit -> selectedFruit2 = fruit }
        )
        Spacer(modifier = Modifier.height(10.dp))
        Text("Selected fruit 1: ${selectedFruit1 ?: "None"}")
        Text("Selected fruit 2: ${selectedFruit2 ?: "None"}")
    }
}
```

This new version of the application demonstrates significant refactoring, primarily focused on state hoisting and
component reusability. Let's examine the changes and their implications in detail:

#### FruitSelector Refactoring

1. **State Hoisting**:
    - `selectedFruit` is no longer managed inside `FruitSelector`. It's now passed as a parameter.
    - A new `onFruitSelected` function is added as a parameter to handle selection changes.

2. **Added Parameters**:
    - `fruits: List<String>`: The fruit list is now a parameter, making the component more flexible.
    - `selectedFruit: String`: The selected fruit state is passed as a parameter.
    - `onFruitSelected: (String) -> Unit`: A callback function to handle fruit selection.

3. **Remaining Local State**:
    - `expanded` remains a local state as it only concerns the dropdown menu display.

#### Changes in App

1. **State Management**:
    - Two new state variables are introduced: `selectedFruit1` and `selectedFruit2`.
    - These states are created with `rememberSaveable` to persist across recompositions and configuration changes.

2. **FruitSelector Usage**:
    - Two instances of `FruitSelector` are created, each with its own fruit list and state.
    - The state and update function are passed to each `FruitSelector`.

3. **Selection Display**:
    - Selected fruits are displayed at the bottom of the App, demonstrating that state is now managed at the top level.

#### Benefits of this Refactoring

1. **Reusability**: `FruitSelector` can now be used multiple times with different fruit lists.

2. **Separation of Concerns**: State management is separated from display, making the code more modular.

3. **Increased Control**: The App now has full control over selection states, allowing for more complex interactions if
   needed.

4. **Improved Testability**: It's easier to test `FruitSelector` as its behavior depends entirely on passed props.

5. **Flexibility**: The fruit list can be dynamic, coming from an API or database, for example.

#### Focus on State Variables

- In `App`, `selectedFruit1` and `selectedFruit2` are state variables created with `rememberSaveable`. This means they
  will retain their value even during configuration changes (like screen rotation).

- In `FruitSelector`, only `expanded` remains a local state variable, as it only concerns the internal display of the
  component.

This refactoring illustrates important Jetpack Compose principles, particularly state hoisting and creating reusable,
independent components. It allows for better application state management and greater flexibility in building the user
interface.
