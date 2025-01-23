# 12. Lifecycle and Architecture

## General Android Application Lifecycle

### Fundamental Concepts

An Android application consists of different components, each with its own lifecycle, but the main element is the *
*Activity**. An activity represents a screen of the application with which the user can interact.

The lifecycle of an activity extends from its creation to its destruction, when the system reclaims its resources.
Understanding this lifecycle is essential for:

- Avoiding crashes during interruptions (phone calls, app switching)
- Optimizing system resource usage
- Preserving application data and state
- Properly handling configuration changes (screen rotation)

### Main Activity States

An activity can be in four main states:

- **Active/Resumed**: The activity is in the foreground and interactive
- **Paused**: Visible but has lost focus
- **Stopped**: Not visible but kept in memory
- **Destroyed**: The activity is terminated

Lifecycle

### Lifecycle Methods

The main methods called during state transitions are:

1. `onCreate()`: Activity initialization
2. `onStart()`: Activity becomes visible
3. `onResume()`: Activity becomes interactive
4. `onPause()`: Activity loses focus
5. `onStop()`: Activity is no longer visible
6. `onDestroy()`: Activity is destroyed

## Jetpack Compose Specificities

### Declarative Approach

Jetpack Compose uses a different approach from the traditional lifecycle. Instead of directly managing state changes
through callback methods, Compose uses a declarative paradigm.

### State Management

In Compose, the lifecycle is closely tied to state management:

- Composables are either stateless or stateful
- State is managed through `State<T>` objects and the `remember{}` function
- Recomposition occurs automatically when state changes

### Effects and Lifecycle

Compose introduces specific effects to manage lifecycle-related operations:

```kotlin
@Composable
fun MyComposable() {
    // Effect executed at each recomposition
    LaunchedEffect(key1) {
        // Code to execute
    }

    // Effect executed only at first composition
    DisposableEffect(key1) {
        onDispose {
            // Resource cleanup
        }
    }
}
```

### Best Practices

For an efficient Compose application:

- Favor stateless components
- Hoist state to the appropriate level
- Use effects for operations with side effects
- Avoid side effects in composables
- Properly manage recomposition to optimize performance

By understanding these concepts, you can develop robust Android applications that properly handle their lifecycle,
whether using the traditional approach or Jetpack Compose.

Citations:

- https://developer.android.com/codelabs/basic-android-kotlin-compose-activity-lifecycle?hl=fr
- https://appmaster.io/fr/blog/jetpack-compose-un-guide-du-debutant
- https://www.weblineindia.com/fr/blog/android-app-development-lifecycle.html
- https://appmaster.io/fr/blog/kotlin-avec-jetpack-compose-les-meilleures-pratiques
- http://www.iro.umontreal.ca/~dift1155/cours/ift1155/communs/Cours/2P/C02_CycledeVie_2P.pdf
- https://developer.android.com/jetpack/androidx/releases/lifecycle?hl=fr
- https://developer.android.com/codelabs/basic-android-kotlin-training-activity-lifecycle?hl=fr
- https://developer.android.com/develop/ui/compose/state?hl=fr
- https://www.yeeply.com/fr/blog/developpement-applications-mobiles/cycle-de-vie-developpement-logiciels-mobiles/
- https://developer.android.com/guide/components/activities/activity-lifecycle?hl=fr
- https://openclassrooms.com/fr/courses/8150246-developpez-votre-premiere-application-android/8256687-apprehendez-le-cycle-de-vie-d-une-application
- https://www.editions-eni.fr/livre/jetpack-compose-developpez-des-interfaces-accessibles-et-modernes-pour-android-9782409039669/gestion-des-etats-et-des-effets
- https://www.lirmm.fr/~fmichel/ens/android/cours/Android_lifecycle.pdf
- https://mathias-seguy.developpez.com/tutoriels/android/comprendre-cyclevie-activite/


