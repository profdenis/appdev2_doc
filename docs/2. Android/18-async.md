# 18. Async and await

Asynchronous programming in Kotlin provides powerful tools to handle operations that might take time to complete without
blocking the main thread. This approach is crucial for creating responsive applications, especially on platforms like
Android.

## Asynchronous Programming in Pure Kotlin

Kotlin's approach to asynchronous programming is centered around **coroutines**, which are lightweight threads that can
be suspended without blocking the underlying thread.

### Coroutines Basics

At the core of Kotlin's asynchronous programming are suspending functions and the `kotlinx.coroutines` library. Here's a
simple example:

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    println("Start")

    val deferred = async {
        delay(1000) // Simulates a long-running operation
        42 // Return value
    }

    println("The computed value is: ${deferred.await()}")
    println("End")
}
```

In this example:

- `runBlocking` creates a coroutine scope that bridges regular and coroutine code
- `async` starts a coroutine that returns a `Deferred` object
- `await()` suspends the coroutine until the result is available[5]

### Understanding Async/Await

The async/await pattern is fundamental in Kotlin coroutines:

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val deferredValue: Deferred = async {
        delay(1000) // Simulate a long computation
        42
    }
    println("The deferred value will be computed soon...")
    val result = deferredValue.await() // Wait for the result without blocking
    println("Computed value: $result")
}
```

This pattern allows you to:

1. Start asynchronous operations with `async`
2. Continue execution without waiting
3. Get the result with `await()` when needed[5]

### Parallel Decomposition

One of the powerful features of coroutines is the ability to run multiple operations in parallel:

```kotlin
import kotlinx.coroutines.*

suspend fun fetchMostRecentOrderId(): String {
    delay(1000) // Simulate network request
    return "order123"
}

suspend fun fetchDeliveryCost(orderId: String): Double {
    delay(500) // Simulate network request
    return 5.99
}

suspend fun fetchStockInformation(orderId: String): String {
    delay(500) // Simulate network request
    return "In stock"
}

suspend fun processOrder() = coroutineScope {
    val orderId = fetchMostRecentOrderId()

    val deliveryCostDeferred = async { fetchDeliveryCost(orderId) }
    val stockInfoDeferred = async { fetchStockInformation(orderId) }

    // Wait for both results
    val deliveryCost = deliveryCostDeferred.await()
    val stockInfo = stockInfoDeferred.await()

    println("Order: $orderId, Delivery: $deliveryCost, Stock: $stockInfo")
}

fun main() = runBlocking {
    processOrder()
}
```

This example demonstrates how to:

- Fetch data sequentially when needed (orderId)
- Launch parallel operations with `async`
- Await results only when required[1]

## Asynchronous Programming in Jetpack Compose

Jetpack Compose integrates seamlessly with Kotlin coroutines to handle UI updates and asynchronous operations.

### LaunchedEffect for Side Effects

```kotlin
@Composable
fun GreetingDisplay() {
    var greeting by remember { mutableStateOf("Loading...") }

    LaunchedEffect(Unit) {
        delay(2000) // Simulate network request
        greeting = "Hello Android!"
    }

    Text(text = greeting)
}
```

This example shows how `LaunchedEffect` launches a coroutine that follows the composable's lifecycle, allowing you to
perform asynchronous operations and update the UI when complete.[3]

??? note "Citations"

      - [1] https://hackernoon.com/asynchronous-programming-techniques-with-kotlin-fg9l3wjn
      - [2] https://www.kodeco.com/books/kotlin-coroutines-by-tutorials/v2.0/chapters/5-async-await
      - [3] https://erecinos98.wordpress.com/2023/08/25/demystifying-asynchronous-programming-in-jetpack-compose-and-apk-sharing/
      - [4] https://kodaschool.com/blog/implementing-coroutines-using-jetpack
      - [5] https://www.dhiwise.com/post/mastering-asynchronous-programming-a-deep-dive
      - [6] https://coldfusion-example.blogspot.com/2025/01/efficiently-handle-asynchronous.html
      - [7] https://developer.android.com/kotlin/coroutines
      - [8] https://30dayscoding.com/blog/android-jetpack-compose-coroutines-integration
      - [9] https://developer.android.com/develop/ui/compose/kotlin
      - [10] https://www.youtube.com/watch?v=Wpco6IK1hmY
      - [11] https://proandroiddev.com/kotlin-coroutines-101-async-programming-in-practice-062b359d502b
      - [12] https://www.simplilearn.com/tutorials/kotlin-tutorial/ultimate-guide-on-kotlin-coroutines
      - [13] https://www.youtube.com/watch?v=9vvNUNyFTFM
      - [14] https://outcomeschool.com/blog/kotlin-coroutines
      - [15] https://www.youtube.com/watch?v=RmkI2HH4fqU
      - [16] https://proandroiddev.com/async-image-loading-the-jetpack-compose-way-2686d1ac5a53


