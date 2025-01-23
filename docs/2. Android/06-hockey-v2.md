# 6. Hockey Application Version 2

## Explanation of `PlayerListWithSearch`

This new version adds a search functionality to the player list. Here's a detailed explanation of the modifications:

### `getPlayers` Function

```kotlin
fun getPlayers(name: String? = null): List<Player> =
    if (name == null)
        getSamplePlayers()
    else
        getSamplePlayers().filter { it.name.lowercase().contains(name.lowercase()) }.toList()
```

This function allows filtering the player list based on a search criterion:

- If `name` is `null`, it returns all players.
- Otherwise, it filters the list to return only players whose name contains the search string (case-insensitive).

### `PlayerListWithSearch` Composable

```kotlin
@Composable
fun PlayerListWithSearch(modifier: Modifier = Modifier) {
    var searchCriteria by rememberSaveable { mutableStateOf("") }
    Column {
        TextField(
            value = searchCriteria,
            onValueChange = { searchCriteria = it },
            modifier = modifier
        )
        LazyColumn(modifier = modifier.fillMaxSize()) {
            items(getPlayers(searchCriteria)) {
                PlayerCard(player = it)
            }
        }
    }
}
```

This composable replaces `PlayerList` and adds search functionality:

1. `searchCriteria`:
    - Uses `rememberSaveable` to preserve the search state even during recompositions.
    - `mutableStateOf("")` initializes the search value to an empty string.

2. `Column`:
    - Vertically organizes the search field and player list.

3. `TextField`:
    - Allows users to enter a search criterion.
    - `value = searchCriteria` displays the current value.
    - `onValueChange = { searchCriteria = it }` updates `searchCriteria` with each change.

4. `LazyColumn`:
    - Similar to the previous version, but now uses `getPlayers(searchCriteria)`.
    - This dynamically filters the player list based on the search criterion.

5. `items`:
    - Creates a `PlayerCard` for each filtered player.

This new version offers a more interactive user experience, allowing users to search for specific players in the list. The list updates automatically with each change in the search field text, thanks to the use of state (`searchCriteria`) and Jetpack Compose's automatic recomposition.

## Connections between `TextField`, `mutableStateOf`, `onValueChange`, and `getPlayers()`

### 1. `mutableStateOf` and Composable State

```kotlin
var searchCriteria by rememberSaveable { mutableStateOf("") }
```

- `mutableStateOf("")` creates a mutable state initialized with an empty string.
- `rememberSaveable` allows preserving this state even during recompositions or configuration changes.
- `by` is used to delegate the property, allowing direct access and modification of `searchCriteria`.

### 2. `TextField` and `onValueChange`

```kotlin
TextField(
    value = searchCriteria,
    onValueChange = { searchCriteria = it },
    modifier = modifier
)
```

- `value = searchCriteria`: The `TextField` displays the current value of `searchCriteria`.
- `onValueChange = { searchCriteria = it }`: When the user types in the field:
    - `it` represents the new field value.
    - This new value is assigned to `searchCriteria`.
    - This triggers a recomposition of the composable.

### 3. Connection with `getPlayers()`

```kotlin
LazyColumn(modifier = modifier.fillMaxSize()) {
    items(getPlayers(searchCriteria)) {
        PlayerCard(player = it)
    }
}
```

- `getPlayers(searchCriteria)` is called with the current value of `searchCriteria`.
- Each time `searchCriteria` changes:
    - `getPlayers()` is recalled with the new value.
    - The player list is filtered based on this new value.
    - `LazyColumn` recomposes with the new filtered list.

### 4. Data Flow and Recomposition

1. The user types in the `TextField`.
2. `onValueChange` is called, updating `searchCriteria`.
3. Updating `searchCriteria` triggers a recomposition.
4. During recomposition:
    - `TextField` displays the new value of `searchCriteria`.
    - `getPlayers(searchCriteria)` is called with the new value.
    - `LazyColumn` recomposes with the new filtered list.

This reactive architecture enables real-time updates of the user interface. Each keystroke in the search field triggers a chain of reactions that results in the filtered display of players, providing a smooth and reactive user experience.