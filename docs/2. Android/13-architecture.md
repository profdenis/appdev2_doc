# 13. Architecture

## Layered Architecture

A modern Android application with Jetpack Compose should follow a distinct layered architecture:

### UI Layer (Presentation)

The UI layer is responsible for displaying data and handling user interactions. With Jetpack Compose, it includes:

**Main Components:**

- UI Composables
- ViewModels
- UI State
- UI Events

```kotlin
@Composable
fun MyScreen(
    viewModel: MyViewModel = viewModel()
) {
    val uiState by viewModel.uiState.collectAsState()

    Column {
        // UI elements
    }
}
```

### Domain Layer (Optional)

This intermediate layer contains the business logic and bridges UI and Data:

- Use cases
- Domain models
- Business logic

```kotlin
class GetUserUseCase(
    private val userRepository: UserRepository
) {
    suspend operator fun invoke(userId: String): User {
        return userRepository.getUser(userId)
    }
}
```

### Data Layer

Manages application data:

- Repositories
- Data sources (API, database)
- Data models

```kotlin
class UserRepository(
    private val api: ApiService,
    private val database: AppDatabase
) {
    suspend fun getUser(id: String): User {
        return database.userDao().getUser(id)
            ?: api.fetchUser(id).also { user ->
                database.userDao().insert(user)
            }
    }
}
```

## State Management with Compose

### UI State

UI state is managed through:

- State hoisting
- `ViewModel` with `StateFlow`/`SharedFlow`
- `remember`/`rememberSaveable`

```kotlin
class MyViewModel : ViewModel() {
    private val _uiState = MutableStateFlow(MyUiState())
    val uiState: StateFlow<MyUiState> = _uiState.asStateFlow()

    fun handleEvent(event: MyUiEvent) {
        // Update state based on events
    }
}
```

### Side Effects

Effects are managed with Compose APIs:

```kotlin
@Composable
fun MyScreen() {
    LaunchedEffect(key1) {
        // One-time setup
    }

    DisposableEffect(key1) {
        onDispose {
            // Cleanup
        }
    }
}
```

## Navigation

Navigation is handled by Navigation Compose:

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    NavHost(navController, startDestination = "home") {
        composable("home") { HomeScreen() }
        composable("details/{id}") { backStackEntry ->
            DetailsScreen(backStackEntry.arguments?.getString("id"))
        }
    }
}
```

## Dependency Injection

Hilt is recommended for dependency injection:

```kotlin
@HiltViewModel
class MainViewModel @Inject constructor(
    private val userRepository: UserRepository
) : ViewModel()
```

## Best Practices

1. **Unidirectional Data Flow (UDF)**
    - UI events flow up to the ViewModel
    - State flows down to UI composables

2. **State Hoisting**
    - Separate state from its manipulation
    - Lift state to appropriate level

3. **Single Source of Truth**
    - One source of truth for data
    - Use Flow for reactivity

4. **Stateless Composables**
    - Prefer stateless composables when possible
    - Separate logic from display

5. **Modularization**
    - Divide application into functional modules
    - Use feature modules for better scalability

This architecture enables creating maintainable, testable, and scalable applications with Jetpack Compose.

Citations:

- https://www.geeksforgeeks.org/android-architecture/
- https://www.simform.com/blog/mobile-application-architecture/
- https://www.intelivita.com/blog/android-architecture-patterns/
- https://developer.android.com/topic/architecture?hl=en
- https://w3r.one/fr/blog/mobile/android/architecture-android/comprendre-architecture-android-vie-ensemble-composants-modeles
- https://www.zucisystems.com/be/blog/limportance-de-larchitecture-mobile-concevoir-des-applications-pour-reussir/
- https://appmaster.io/fr/blog/kotlin-avec-jetpack-compose-les-meilleures-pratiques
- https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=fr
