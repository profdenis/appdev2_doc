# 10. Localization

Here's a guide to integrate localization in an Android application developed with Jetpack Compose:

## General Principles of Localization

Localizing an Android application involves adapting its content and user interface to different languages and cultures.
Here are the key principles to follow:

1. Externalize all strings in resource files.
2. Use neutral identifiers for resources (e.g., "welcome_message" instead of "english_welcome").
3. Avoid hardcoding text in your code.
4. Consider cultural differences (date formats, measurement units, etc.).
5. Plan for extra space in your interface for longer translations.
6. Test your application in different languages and configurations.

## Simple Example with Strings in English and French

### Step 1: Configure Language Resources

1- In the `res` folder, create a `values-fr` folder for French resources.

2- In `res/values/strings.xml` (default English):

```xml

<resources>
    <string name="app_name">My App</string>
    <string name="welcome_message">Welcome to My App!</string>
    <string name="language_selection">Select a language</string>
</resources>
```

3- In `res/values-fr/strings.xml`:

```xml

<resources>
    <string name="app_name">Mon Application</string>
    <string name="welcome_message">Bienvenue dans Mon Application !</string>
    <string name="language_selection">Sélectionnez une langue</string>
</resources>
```

### Step 2: Use Localized Strings in Jetpack Compose

```kotlin
@Composable
fun WelcomeScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(
            text = stringResource(R.string.welcome_message),
            style = MaterialTheme.typography.h4
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            text = stringResource(R.string.language_selection),
            style = MaterialTheme.typography.body1
        )
    }
}
```

## Date Formatting

To format dates according to the user's locale, use the `DateTimeFormatter` class with `ofLocalizedDate()`:

```kotlin
import java.time.LocalDate
import java.time.format.DateTimeFormatter
import java.time.format.FormatStyle

@Composable
fun DisplayLocalizedDate() {
    val currentDate = LocalDate.now()
    val dateFormatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.LONG)
    val formattedDate = remember(currentDate) {
        currentDate.format(dateFormatter)
    }

    Text(
        text = formattedDate,
        style = MaterialTheme.typography.body1
    )
}
```

This code will display the current date in the appropriate long format for the user's locale. For example:

- In English (US): "September 18, 2024"
- In French: "18 septembre 2024"

By following these steps, your Jetpack Compose application will be properly localized in English and French, with the
ability to easily add other languages in the future.

Citations:

- [https://www.translized.com/blog/android-localization-with-jetpack-compose---a-comprehensive-guide](https://www.translized.com/blog/android-localization-with-jetpack-compose---a-comprehensive-guide)
- [https://phrase.com/blog/posts/localized-date-time-android/](https://phrase.com/blog/posts/localized-date-time-android/)
- [https://www.youtube.com/watch?v=VdwDawvfH98](https://www.youtube.com/watch?v=VdwDawvfH98)
- [https://phrase.com/blog/posts/internationalizing-jetpack-compose-android-apps/](https://phrase.com/blog/posts/internationalizing-jetpack-compose-android-apps/)
- [https://www.adamormsby.com/posts/013-android-localization-formatting-dates/](https://www.adamormsby.com/posts/013-android-localization-formatting-dates/)

## Complete Example

Here's a complete example of a simple application using Jetpack Compose with localization, without buttons to change the
language. This application will display a localized welcome message and the current date formatted according to the
device's locale.

```kotlin
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.material.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.unit.dp
import java.time.LocalDate
import java.time.format.DateTimeFormatter
import java.time.format.FormatStyle

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyApp()
        }
    }
}

@Composable
fun MyApp() {
    MaterialTheme {
        Surface(
            modifier = Modifier.fillMaxSize(),
            color = MaterialTheme.colors.background
        ) {
            LocalizedContent()
        }
    }
}

@Composable
fun LocalizedContent() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text(
            text = stringResource(R.string.welcome_message),
            style = MaterialTheme.typography.h4
        )

        Spacer(modifier = Modifier.height(16.dp))

        DisplayLocalizedDate()
    }
}

@Composable
fun DisplayLocalizedDate() {
    val currentDate = remember { LocalDate.now() }
    val dateFormatter = remember { DateTimeFormatter.ofLocalizedDate(FormatStyle.LONG) }
    val formattedDate = remember(currentDate) {
        currentDate.format(dateFormatter)
    }

    Text(
        text = formattedDate,
        style = MaterialTheme.typography.body1
    )
}
```

For this application to work correctly, you also need to configure the following resource files:

- In `res/values/strings.xml` (default English):

```xml

<resources>
    <string name="app_name">My Localized App</string>
    <string name="welcome_message">Welcome to My Localized App!</string>
</resources>
```

- In `res/values-fr/strings.xml` (for French):

```xml

<resources>
    <string name="app_name">Mon Application Localisée</string>
    <string name="welcome_message">Bienvenue dans Mon Application Localisée !</string>
</resources>
```

- Make sure you have the necessary dependencies in your `build.gradle` (Module: app) file:

```gradle
dependencies {
    implementation "androidx.compose.ui:ui:1.5.0"
    implementation "androidx.compose.material:material:1.5.0"
    implementation "androidx.compose.ui:ui-tooling-preview:1.5.0"
    implementation "androidx.activity:activity-compose:1.7.2"
}
```

This simple application displays:

1. A localized welcome message
2. The current date formatted according to the device's locale

The application will automatically use the appropriate resources based on the language configured on the user's device.
If the device is set to French, it will use the strings from the `values-fr/strings.xml` file. For any other language,
it will use the default strings from the `values/strings.xml` file.

To test different languages, you can change the language of your device or emulator in the system settings. The
application will automatically adapt to the new language without requiring a restart.

