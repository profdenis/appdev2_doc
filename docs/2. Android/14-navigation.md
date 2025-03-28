# 14. Navigation in Jetpack Compose

[Git Repository](https://github.com/profdenis/NavigationSimple)

## Example with Buttons to Navigate Between Composables

### Initial Setup

First, add the dependency in the `build.gradle` file:

```kotlin
dependencies {
    implementation("androidx.navigation:navigation-compose:2.8.3")
}
```

### Example Structure

We will create an application with three screens:

- Home screen
- Profile screen
- Settings screen

#### Route Definition

```kotlin
sealed class Screen(val route: String) {
    object Home : Screen("home")
    object Profile : Screen("profile")
    object Settings : Screen("settings")
}
```

#### Screen Composable Creation

```kotlin
@Composable
fun HomeScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Home Screen")
        Button(onClick = { navController.navigate(Screen.Profile.route) }) {
            Text("Go to Profile")
        }
        Button(onClick = { navController.navigate(Screen.Settings.route) }) {
            Text("Go to Settings")
        }
    }
}

@Composable
fun ProfileScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Profile Screen")
        Button(onClick = { navController.navigate(Screen.Settings.route) }) {
            Text("Go to Settings")
        }
        Button(onClick = { navController.popBackStack() }) {
            Text("Back")
        }
    }
}

@Composable
fun SettingsScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Settings Screen")
        Button(onClick = { navController.popBackStack() }) {
            Text("Back")
        }
        Button(
            onClick = {
                navController.navigate(Screen.Home.route) {
                    popUpTo(Screen.Home.route) { inclusive = true }
                }
            }
        ) {
            Text("Back to Home")
        }
    }
}
```

#### Navigation Configuration

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    NavHost(
        navController = navController,
        startDestination = Screen.Home.route
    ) {
        composable(Screen.Home.route) {
            HomeScreen(navController)
        }
        composable(Screen.Profile.route) {
            ProfileScreen(navController)
        }
        composable(Screen.Settings.route) {
            SettingsScreen(navController)
        }
    }
}
```

#### Integration in MainActivity

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    AppNavigation()
                }
            }
        }
    }
}
```

### Demonstrated Features

1. **Basic Navigation**: Navigation between screens using `navigate()`
2. **Back Navigation**: Using `popBackStack()`
3. **Navigation with Stack Clearing**: Using `popUpTo` to return to home

### Important Points

- The `NavController` manages the navigation state
- The `NavHost` defines the navigation graph
- Each screen receives the `NavController` to manage navigation
- `popBackStack()` allows returning to the previous screen
- `popUpTo` with `inclusive = true` allows going back to a destination while including it in the removal

This example shows the basics of navigation in Jetpack Compose while remaining simple and understandable for beginners.

Citations:
[1] https://proandroiddev.com/android-jetpack-compose-navigation-1cdfc488b891?gi=d31e0323b815
https://saurabhjadhavblogs.com/ultimate-guide-to-jetpack-compose-navigation
https://blog.kotlin-academy.com/mastery-navigation-in-jetpack-compose-db00b0a0ef75?gi=007e50484ede
https://developer.android.com/develop/ui/compose/navigation
https://proandroiddev.com/mastering-navigation-in-jetpack-compose-a-guide-to-using-the-inclusive-attribute-b66916a5f15c?gi=401071494588
https://developer.android.com/codelabs/basic-android-kotlin-compose-navigation
https://developer.android.com/develop/ui/compose/navigation?hl=fr
https://www.youtube.com/watch?v=AIC_OFQ1r3k

## Example with Menu in Application Top Bar

### Application Structure

```kotlin
sealed class Screen(val route: String, val title: String) {
    object Home : Screen("home", "Home")
    object Profile : Screen("profile", "Profile")
    object Settings : Screen("settings", "Settings")
}
```

### Navigation Configuration

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    Scaffold(
        topBar = {
            TopAppBarWithMenu(navController)
        }
    ) { paddingValues ->
        NavHost(
            navController = navController,
            startDestination = Screen.Home.route,
            modifier = Modifier.padding(paddingValues)
        ) {
            composable(Screen.Home.route) {
                HomeScreen()
            }
            composable(Screen.Profile.route) {
                ProfileScreen()
            }
            composable(Screen.Settings.route) {
                SettingsScreen()
            }
        }
    }
}
```

### TopBar with Menu

```kotlin
@Composable
fun TopAppBarWithMenu(navController: NavController) {
    var showMenu by remember { mutableStateOf(false) }

    TopAppBar(
        title = { Text("My Application") },
        actions = {
            IconButton(onClick = { showMenu = !showMenu }) {
                Icon(Icons.Default.MoreVert, contentDescription = "Menu")
            }
            DropdownMenu(
                expanded = showMenu,
                onDismissRequest = { showMenu = false }
            ) {
                DropdownMenuItem(
                    text = { Text(Screen.Home.title) },
                    onClick = {
                        navController.navigate(Screen.Home.route) {
                            popUpTo(Screen.Home.route) { inclusive = true }
                        }
                        showMenu = false
                    }
                )
                DropdownMenuItem(
                    text = { Text(Screen.Profile.title) },
                    onClick = {
                        navController.navigate(Screen.Profile.route)
                        showMenu = false
                    }
                )
                DropdownMenuItem(
                    text = { Text(Screen.Settings.title) },
                    onClick = {
                        navController.navigate(Screen.Settings.route)
                        showMenu = false
                    }
                )
            }
        }
    )
}
```

### Application Screens

```kotlin
@Composable
fun HomeScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Home Screen")
    }
}

@Composable
fun ProfileScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Profile Screen")
    }
}

@Composable
fun SettingsScreen() {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Settings Screen")
    }
}
```

### Important Points to Note

1. The menu is managed by a `showMenu` state that controls the `DropdownMenu` display

2. Each menu item uses `navController.navigate()` for navigation

3. For the home screen, we use `popUpTo` with `inclusive = true` to avoid accumulating screens in the navigation stack

4. The `Scaffold` automatically manages the necessary padding to prevent content from being hidden behind the TopBar

5. The screens are simple but can be enriched according to the application's needs

This implementation offers clear and intuitive navigation via a dropdown menu in the application's top bar.

??? note "Citations"

     - https://saurabhjadhavblogs.com/ultimate-guide-to-jetpack-compose-navigation
     - https://proandroiddev.com/android-jetpack-compose-navigation-1cdfc488b891?gi=d31e0323b815
     - https://proandroiddev.com/implement-bottom-bar-navigation-in-jetpack-compose-b530b1cd9ee2?gi=5c1ab6e9d027
     - https://proandroiddev.com/mastering-navigation-in-jetpack-compose-a-guide-to-using-the-inclusive-attribute-b66916a5f15c?gi=401071494588
     - https://blog.kotlin-academy.com/mastery-navigation-in-jetpack-compose-db00b0a0ef75?gi=007e50484ede
     - https://developer.android.com/codelabs/basic-android-kotlin-compose-navigation
     - https://www.youtube.com/watch?v=JLICaBEiJS0
     - https://developer.android.com/develop/ui/compose/navigation

## Example with Icons in Bottom Bar

### Application Structure

```kotlin
sealed class Screen(
    val route: String,
    val title: String,
    val icon: ImageVector
) {
    object Home : Screen(
        route = "home",
        title = "Home",
        icon = Icons.Default.Home
    )
    object Profile : Screen(
        route = "profile",
        title = "Profile",
        icon = Icons.Default.Person
    )
    object Settings : Screen(
        route = "settings",
        title = "Settings",
        icon = Icons.Default.Settings
    )

    companion object {
        val items = listOf(Home, Profile, Settings)
    }
}
```

### Navigation Configuration

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()
    val navBackStackEntry by navController.currentBackStackEntryAsState()
    val currentRoute = navBackStackEntry?.destination?.route

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("My Application") }
            )
        },
        bottomBar = {
            NavigationBar {
                Screen.items.forEach { screen ->
                    NavigationBarItem(
                        icon = {
                            Icon(screen.icon, contentDescription = screen.title)
                        },
                        label = { Text(screen.title) },
                        selected = currentRoute == screen.route,
                        onClick = {
                            navController.navigate(screen.route) {
                                // Avoid stacking destinations
                                popUpTo(navController.graph.startDestinationId) {
                                    saveState = true
                                }
                                // Avoid multiple copies of the same destination
                                launchSingleTop = true
                                // Restore state when reselecting
                                restoreState = true
                            }
                        }
                    )
                }
            }
        }
    ) { paddingValues ->
        NavHost(
            navController = navController,
            startDestination = Screen.Home.route,
            modifier = Modifier.padding(paddingValues)
        ) {
            composable(Screen.Home.route) {
                HomeScreen()
            }
            composable(Screen.Profile.route) {
                ProfileScreen()
            }
            composable(Screen.Settings.route) {
                SettingsScreen()
            }
        }
    }
}
```

### Application Screens

```kotlin
@Composable
fun HomeScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Icon(
            Icons.Default.Home,
            contentDescription = null,
            modifier = Modifier.size(48.dp)
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            "Home Screen",
            style = MaterialTheme.typography.headlineMedium
        )
    }
}

@Composable
fun ProfileScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Icon(
            Icons.Default.Person,
            contentDescription = null,
            modifier = Modifier.size(48.dp)
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            "Profile Screen",
            style = MaterialTheme.typography.headlineMedium
        )
    }
}

@Composable
fun SettingsScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Icon(
            Icons.Default.Settings,
            contentDescription = null,
            modifier = Modifier.size(48.dp)
        )
        Spacer(modifier = Modifier.height(16.dp))
        Text(
            "Settings Screen",
            style = MaterialTheme.typography.headlineMedium
        )
    }
}
```

### Important Points

1. The `NavigationBar` uses `NavigationBarItem` for each destination

2. The `selected` state is managed by comparing the current route with the item's route

3. Navigation includes important options:
    - `launchSingleTop` avoids multiple copies
    - `popUpTo` with `saveState` properly manages the navigation stack
    - `restoreState` preserves state during reselection

4. Icons and titles are defined in the sealed `Screen` class

5. `currentBackStackEntryAsState()` allows tracking the current destination

This implementation offers smooth and intuitive navigation with a bottom navigation bar, commonly used in modern mobile
applications. Users can easily switch between different sections of the application by touching the corresponding icons.




