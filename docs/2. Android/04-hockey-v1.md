# 4. Hockey Application Version 1

This code defines a structure to represent hockey players and creates user interface components using Jetpack Compose to
display these players in a list. It includes a data class for players, a function to generate sample players, and two
main composable functions: one to display an individual player card and another to display a list of these cards.

## Detailed Explanation of Composable Functions

### `PlayerCard`

```kotlin
@Composable
fun PlayerCard(player: Player, modifier: Modifier = Modifier) {
    // ...
}
```

This composable function creates a card to display information about an individual player.

- It takes a `Player` object and an optional `Modifier` as parameters.
- Uses a `Card` component to create a card with rounded corners and a border.
- Inside the card, it organizes content in a `Column` (vertical column).
- The first `Row` (horizontal line) contains the player's image.
- The second `Row` displays the player's number and name.

Components used:

- `Card`: Creates an elevated surface with shadow and content.
- `Column`: Arranges elements vertically.
- `Row`: Arranges elements horizontally.
- `Image`: Displays the player's image.
- `Text`: Displays text (player's number and name).
- `Spacer`: Creates space between elements.

Modifiers used:

- `fillMaxWidth()`: Fills all available width.
- `padding()`: Adds space around elements.
- `background()`: Sets the background color.
- `width()`: Sets a specific width.

### `PlayerList`

```kotlin
@Composable
fun PlayerList(modifier: Modifier = Modifier) {
    LazyColumn(modifier = modifier) {
        items(getSamplePlayers()) {
            PlayerCard(player = it)
        }
    }
}
```

This composable function creates a scrollable list of player cards.

- It uses `LazyColumn`, which is optimized for displaying long lists of items.
- The `items()` function is used to dynamically generate list items from the player list returned by
  `getSamplePlayers()`.
- For each player in the list, it creates a `PlayerCard`.

Main component:

- `LazyColumn`: A vertical scrolling container that only loads and displays items visible on screen, making it efficient
  for long lists.

These two composable functions work together to create an interactive and efficient user interface for displaying a list
of hockey players. `PlayerCard` handles the display of individual player details, while `PlayerList` organizes these
cards in a scrollable list.