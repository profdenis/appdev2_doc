# 7. Screen Orientation

To detect screen orientation (portrait or landscape) with Jetpack Compose, you can use the `LocalConfiguration`
composable. Here's how to do it:

1- Import the necessary dependencies:

```kotlin
import android.content.res.Configuration
import androidx.compose.runtime.Composable
import androidx.compose.ui.platform.LocalConfiguration
```

2- Use `LocalConfiguration` in your composable to get the current configuration:

```kotlin
@Composable
fun MyScreen() {
    val configuration = LocalConfiguration.current

    when (configuration.orientation) {
        Configuration.ORIENTATION_LANDSCAPE -> {
            // Code for landscape orientation
            Text("Screen is in landscape mode")
        }
        else -> {
            // Code for portrait orientation
            Text("Screen is in portrait mode")
        }
    }
}
```

3- To observe orientation changes, you can use a `State`:

```kotlin
@Composable
fun OrientationAwareLayout() {
    val configuration = LocalConfiguration.current
    val orientation by remember { mutableStateOf(configuration.orientation) }

    LaunchedEffect(configuration) {
        snapshotFlow { configuration.orientation }
            .collect { orientation = it }
    }

    when (orientation) {
        Configuration.ORIENTATION_LANDSCAPE -> {
            // Layout for landscape mode
        }
        else -> {
            // Layout for portrait mode
        }
    }
}
```

This approach allows your composable to automatically recompose when the orientation changes.

By using these methods, you can create responsive user interfaces that adapt to screen orientation. This is particularly
useful for optimizing user experience across different devices and screen configurations.

Citations:

- [https://developer.android.com/guide/practices/device-compatibility-mode?hl=fr](https://developer.android.com/guide/practices/device-compatibility-mode?hl=fr)
- [https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/](https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/)
- [https://blog.ippon.fr/2023/04/28/developper-app-jetpack-compose-smartphones-pliables/](https://blog.ippon.fr/2023/04/28/developper-app-jetpack-compose-smartphones-pliables/)
- [https://developer.android.com/develop/ui/compose/touch-input/pointer-input/drag-swipe-fling?hl=fr](https://developer.android.com/develop/ui/compose/touch-input/pointer-input/drag-swipe-fling?hl=fr)
- [https://developer.android.com/develop/ui/compose/touch-input/stylus-input/advanced-stylus-features?hl=fr](https://developer.android.com/develop/ui/compose/touch-input/stylus-input/advanced-stylus-features?hl=fr)
- [https://stackoverflow.com/questions/64753944/orientation-on-jetpack-compose](https://stackoverflow.com/questions/64753944/orientation-on-jetpack-compose)
- [https://appmaster.io/fr/blog/comment-creer-une-interface-utilisateur-adaptative-avec-jetpack-compose](https://appmaster.io/fr/blog/comment-creer-une-interface-utilisateur-adaptative-avec-jetpack-compose)

## Complete Example

```kotlin
import android.content.res.Configuration
import androidx.compose.foundation.layout.*
import androidx.compose.material3.Button
import androidx.compose.material3.ExperimentalMaterial3Api
import androidx.compose.material3.Text
import androidx.compose.material3.TextField
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalConfiguration
import androidx.compose.ui.unit.dp

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun OrientationResponsiveLayout(modifier: Modifier = Modifier) {
    var text1 by remember { mutableStateOf("") }
    var text2 by remember { mutableStateOf("") }

    val configuration = LocalConfiguration.current
    val isLandscape = configuration.orientation == Configuration.ORIENTATION_LANDSCAPE

    if (isLandscape) {
        Column(
            modifier = modifier
                .fillMaxSize()
                .padding(16.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween
            ) {
                TextField(
                    value = text1,
                    onValueChange = { text1 = it },
                    label = { Text("Field 1") },
                    modifier = Modifier.weight(1f).padding(end = 8.dp)
                )
                TextField(
                    value = text2,
                    onValueChange = { text2 = it },
                    label = { Text("Field 2") },
                    modifier = Modifier.weight(1f).padding(start = 8.dp)
                )
            }
            Spacer(modifier = Modifier.height(16.dp))
            Button(onClick = { /* Button action */ }) {
                Text("Validate")
            }
        }
    } else {
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(16.dp),
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.spacedBy(16.dp)
        ) {
            TextField(
                value = text1,
                onValueChange = { text1 = it },
                label = { Text("Field 1") },
                modifier = Modifier.fillMaxWidth()
            )
            TextField(
                value = text2,
                onValueChange = { text2 = it },
                label = { Text("Field 2") },
                modifier = Modifier.fillMaxWidth()
            )
            Button(onClick = { /* Button action */ }) {
                Text("Validate")
            }
        }
    }
}
```

In this example:

1. We use `LocalConfiguration.current` to get the current screen configuration.

2. We check if the orientation is in landscape mode by comparing `configuration.orientation` with
   `Configuration.ORIENTATION_LANDSCAPE`.

3. In portrait mode (default):
    - We use a `Column` to vertically arrange two `TextField`s and a `Button`.
    - Elements are evenly spaced using `verticalArrangement = Arrangement.spacedBy(16.dp)`.

4. In landscape mode:
    - We use a main `Column` for the overall layout.
    - Inside, we use a `Row` to place the two `TextField`s side by side.
    - The `Button` is placed below the `Row` with a `Spacer` to add spacing.

5. The `TextField`s use `Modifier.weight(1f)` in landscape mode to occupy equal space in the `Row`.

6. We use `remember` and `mutableStateOf` to manage the text fields' state, allowing users to interact with them.

This composable will automatically adapt to the screen orientation, providing an optimized layout for both portrait and
landscape modes. It demonstrates how to create responsive user interfaces that adapt to different screen configurations
using Jetpack Compose.

Citations:

- [https://developer.android.com/develop/ui/compose/layouts/adaptive](https://developer.android.com/develop/ui/compose/layouts/adaptive)
- [https://www.blog.finotes.com/post/creating-responsive-layouts-in-android-using-jetpack-compose](https://www.blog.finotes.com/post/creating-responsive-layouts-in-android-using-jetpack-compose)
- [https://stackoverflow.com/questions/67157309/how-to-create-responsive-layouts-with-jetpack-compose](https://stackoverflow.com/questions/67157309/how-to-create-responsive-layouts-with-jetpack-compose)
- [https://composables.com/jetpack-compose-tutorials/responsive-layout](https://composables.com/jetpack-compose-tutorials/responsive-layout)
- [https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/](https://www.geeksforgeeks.org/detect-screen-orientation-in-android-using-jetpack-compose/)
- [https://eevis.codes/blog/2024-07-18/dont-lock-the-screen-orientation-handling-orientation-in-compose/](https://eevis.codes/blog/2024-07-18/dont-lock-the-screen-orientation-handling-orientation-in-compose/)
