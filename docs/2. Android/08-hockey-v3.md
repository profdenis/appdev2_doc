# 8. Hockey Application Version 3

## Different Versions of `PlayerListWithSearch`

### Previous Version

```kotlin
@Composable
fun PlayerListWithSearch(modifier: Modifier = Modifier) {
    var nameSearch by rememberSaveable { mutableStateOf("") }
    var numberSearch: Int? by rememberSaveable { mutableStateOf(null) }
    Column(modifier = modifier) {
        Column(modifier = Modifier.padding(top = 20.dp, bottom = 20.dp)) {
            TextField(
                value = nameSearch,
                label = { Text(text = "Name") },
                onValueChange = { nameSearch = it },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 20.dp, end = 20.dp)
            )
            TextField(
                value = (numberSearch ?: "").toString(),
                label = { Text(text = "Number") },
                onValueChange = {
                    numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch
                },
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = 20.dp, end = 20.dp)
            )
        }

        LazyColumn(modifier = Modifier.fillMaxSize()) {
            items(getPlayers(nameSearch, numberSearch)) {
                HockeyPlayerCard(player = it)
            }
        }
    }
}
```

#### General Description

This code defines a composable function named `PlayerListWithSearch` that creates a user interface to display a list of
hockey players with search functionality. The interface includes two text fields for searching (one for name and one for
number) and a scrollable list of players matching the search criteria.

#### Detailed Description

1- Function Declaration:

```kotlin
@Composable
fun PlayerListWithSearch(modifier: Modifier = Modifier)
```

- It's a composable function that accepts an optional `modifier` as a parameter.

2- State Variables:

```kotlin
var nameSearch by rememberSaveable { mutableStateOf("") }
var numberSearch: Int? by rememberSaveable { mutableStateOf(null) }
```

- `nameSearch`: A string to store the name search.
- `numberSearch`: A nullable integer to store the number search.
- These variables use `rememberSaveable` to preserve their state even after reconfiguration.

3- Main Structure:

```kotlin
Column(modifier = modifier) {
    // Content
}
```

- Uses a `Column` as the main container with the modifier passed as parameter.

4- Search Fields:

```kotlin
Column(modifier = Modifier.padding(top = 20.dp, bottom = 20.dp)) {
    // Text fields
}
```

- An inner `Column` with padding to contain the search fields.

5- Name Search Field:

```kotlin
TextField(
    value = nameSearch,
    label = { Text(text = "Name") },
    onValueChange = { nameSearch = it },
    modifier = Modifier
        .fillMaxWidth()
        .padding(start = 20.dp, end = 20.dp)
)
```

- A `TextField` for name search.
- The value is bound to `nameSearch`.
- The label displays "Name".
- `onValueChange` updates `nameSearch` with the new value.

6- Number Search Field:

```kotlin
TextField(
    value = (numberSearch ?: "").toString(),
    label = { Text(text = "Number") },
    onValueChange = {
        numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch
    },
    modifier = Modifier
        .fillMaxWidth()
        .padding(start = 20.dp, end = 20.dp)
)
```

- A `TextField` for number search.
- The displayed value is the string conversion of `numberSearch` (or empty string if null).
- The label displays "Number".
- `onValueChange` attempts to convert the input to an integer, keeping the previous value if conversion fails.

7- Player List:

```kotlin
LazyColumn(modifier = Modifier.fillMaxSize()) {
    items(getPlayers(nameSearch, numberSearch)) {
        HockeyPlayerCard(player = it)
    }
}
```

- Uses a `LazyColumn` to efficiently display a potentially long list of players.
- `getPlayers(nameSearch, numberSearch)` is called to get the filtered list of players.
- Each player is displayed using a `HockeyPlayerCard` component.

This code creates an interactive user interface allowing users to search for hockey players by name and number, with
dynamic updating of the displayed list based on search criteria.

### New Version

```kotlin
@Composable
fun PlayerListWithSearch(modifier: Modifier = Modifier) {
    var nameSearch by rememberSaveable { mutableStateOf("") }
    var numberSearch: Int? by rememberSaveable { mutableStateOf(null) }
    Column(modifier = modifier) {
        SearchTextFields(
            nameSearch = nameSearch,
            onNameChange = { nameSearch = it },
            numberSearch = numberSearch,
            onNumberChange =
                { numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch })

        LazyColumn(modifier = Modifier.fillMaxSize()) {
            items(getPlayers(nameSearch, numberSearch)) {
                HockeyPlayerCard(player = it)
            }
        }
    }
}

@Composable
private fun SearchTextFields(
    nameSearch: String,
    onNameChange: (String) -> Unit,
    numberSearch: Int?,
    onNumberChange: (String) -> Unit,
) {
    Column(modifier = Modifier.padding(top = 20.dp, bottom = 20.dp)) {
        TextField(
            value = nameSearch,
            label = { Text(text = "Name") },
            onValueChange = onNameChange,
            modifier = Modifier
                .fillMaxWidth()
                .padding(start = 20.dp, end = 20.dp)
        )
        TextField(
            value = (numberSearch ?: "").toString(),
            label = { Text(text = "Number") },
            onValueChange = onNumberChange,
            modifier = Modifier
                .fillMaxWidth()
                .padding(start = 20.dp, end = 20.dp)
        )
    }
}
```

This refactoring illustrates how to improve code structure and reusability in Jetpack Compose. Here's a detailed
explanation of the transition between the two versions, focusing on state management and lambda functions:

1. Creation of the new `SearchTextFields` composable:
    - A new private composable was created to encapsulate the search fields logic.
    - This improves code readability and reusability.

2. Parameters of the new composable:
    - `nameSearch: String`: The current value of the name search.
    - `onNameChange: (String) -> Unit`: A lambda function to handle name changes.
    - `numberSearch: Int?`: The current value of the number search.
    - `onNumberChange: (String) -> Unit`: A lambda function to handle number changes.

3. Passing state variables:
    - In `PlayerListWithSearch`, state variables `nameSearch` and `numberSearch` are passed directly to
      `SearchTextFields`.
    - This allows `SearchTextFields` to use these values without owning or modifying them directly.

4. Passing lambda functions:
    - For `onNameChange`, a simple lambda is passed: `{ nameSearch = it }`.
      This lambda directly updates the `nameSearch` state variable.
    - For `onNumberChange`, the conversion logic is passed as a lambda:
      ```kotlin
      { numberSearch = if (it.isEmpty()) null else it.toIntOrNull() ?: numberSearch }
      ```
      This lambda handles the string to integer conversion and updating `numberSearch`.

5. Usage in `SearchTextFields`:
    - The `TextField`s now use the passed parameters instead of directly accessing state variables.
    - `value = nameSearch` and `value = (numberSearch ?: "").toString()` use the passed values.
    - `onValueChange = onNameChange` and `onValueChange = onNumberChange` use the passed lambdas.

6. Advantages of this approach:
    - Separation of concerns: `SearchTextFields` only handles display and input collection.
    - State hoisting: State remains in the parent component, making `SearchTextFields` more flexible and reusable.
    - Improved testability: It's easier to test `SearchTextFields` independently.

7. Implications for state management:
    - State (`nameSearch` and `numberSearch`) is still managed in `PlayerListWithSearch`.
    - `SearchTextFields` becomes a stateless component, only reflecting the state passed to it.

This refactoring is an excellent example of how to apply the "state hoisting" principle in Jetpack Compose, by elevating
state and state modification functions to the parent component. This makes the code more modular, easier to maintain and
test, while maintaining a clear separation of responsibilities between components.
