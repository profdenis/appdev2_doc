# 19. Making API calls

Making web API calls in Kotlin can be accomplished using several libraries. Here are examples using both pure Kotlin and
within a Jetpack Compose application.

## Making API Calls in Pure Kotlin

You can use libraries like OkHttp, Ktor Client, or the built-in HttpClient. Here's an example using OkHttp:

```kotlin
// Add dependency: implementation("com.squareup.okhttp3:okhttp:4.9.0") or latest version 4.12.0
import okhttp3.OkHttpClient
import okhttp3.Request
import okhttp3.MediaType.Companion.toMediaType
import okhttp3.RequestBody.Companion.toRequestBody
import java.net.URL
import java.util.concurrent.TimeUnit

// Constants for timeouts
private const val CONNECT_TIMEOUT = 15L
private const val READ_TIMEOUT = 15L
private const val WRITE_TIMEOUT = 15L

// GET request example
fun performGetRequest(urlString: String, token: String = ""): String? {
    return try {
        val client = OkHttpClient.Builder()
            .connectTimeout(CONNECT_TIMEOUT, TimeUnit.SECONDS)
            .writeTimeout(WRITE_TIMEOUT, TimeUnit.SECONDS)
            .readTimeout(READ_TIMEOUT, TimeUnit.SECONDS)
            .build()

        val request = Request.Builder()
            .url(URL(urlString))
            .apply { if (token.isNotEmpty()) header("Authorization", token) }
            .get()
            .build()

        val response = client.newCall(request).execute()
        response.body?.string()
    } catch (e: Exception) {
        e.printStackTrace()
        null
    }
}

// POST request example
fun performPostRequest(urlString: String, jsonString: String, token: String = ""): String? {
    return try {
        val client = OkHttpClient.Builder()
            .connectTimeout(CONNECT_TIMEOUT, TimeUnit.SECONDS)
            .writeTimeout(WRITE_TIMEOUT, TimeUnit.SECONDS)
            .readTimeout(READ_TIMEOUT, TimeUnit.SECONDS)
            .build()

        val mediaType = "application/json; charset=utf-8".toMediaType()
        val body = jsonString.toRequestBody(mediaType)

        val request = Request.Builder()
            .url(URL(urlString))
            .apply { if (token.isNotEmpty()) header("Authorization", token) }
            .post(body)
            .build()

        val response = client.newCall(request).execute()
        response.body?.string()
    } catch (e: Exception) {
        e.printStackTrace()
        null
    }
}

// Usage example
fun main() {
    // GET request
    val getResponse = performGetRequest("https://jsonplaceholder.typicode.com/posts/1")
    println("GET Response: $getResponse")

    // POST request
    val jsonPayload = """{"title": "foo", "body": "bar", "userId": 1}"""
    val postResponse = performPostRequest("https://jsonplaceholder.typicode.com/posts", jsonPayload)
    println("POST Response: $postResponse")
}
```

### Output

```text
GET Response: {
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
}
POST Response: {
  "title": "foo",
  "body": "bar",
  "userId": 1,
  "id": 101
}
```


## Making API Calls in a Jetpack Compose App

For a Compose app, you'll typically use a ViewModel to handle API calls. Here's an example using Retrofit with Jetpack
Compose:

First, set up your project:

1. Add internet permission in `AndroidManifest.xml`:

```xml

```

2. Add dependencies:

```kotlin
// In build.gradle.kts
implementation("com.squareup.retrofit2:retrofit:2.9.0")
implementation("com.squareup.retrofit2:converter-gson:2.9.0")
implementation("com.google.code.gson:gson:2.10.1")
```

3. Create API interface:

```kotlin
// Define your API endpoints
interface UserApi {
    @GET("users/{id}")
    fun getUserById(@Path("id") id: String): Call
}

// Data model
data class UserModel(
    val name: String,
    val email: String,
    val age: String
)
```

4. Create a ViewModel:

```kotlin
class UserViewModel : ViewModel() {
    private val _userData = MutableStateFlow(null)
    val userData: StateFlow = _userData.asStateFlow()

    private val _isLoading = MutableStateFlow(false)
    val isLoading: StateFlow = _isLoading

    private val _error = MutableStateFlow(null)
    val error: StateFlow = _error

    fun fetchUserData(id: String) {
        viewModelScope.launch {
            _isLoading.value = true
            _error.value = null

            try {
                val retrofit = Retrofit.Builder()
                    .baseUrl("https://api.example.com/")
                    .addConverterFactory(GsonConverterFactory.create())
                    .build()

                val api = retrofit.create(UserApi::class.java)
                val call = api.getUserById(id)

                call.enqueue(object : Callback {
                    override fun onResponse(call: Call, response: Response) {
                        _isLoading.value = false
                        if (response.isSuccessful) {
                            _userData.value = response.body()
                        } else {
                            _error.value = "Error: ${response.code()}"
                        }
                    }

                    override fun onFailure(call: Call, t: Throwable) {
                        _isLoading.value = false
                        _error.value = "Network error: ${t.message}"
                    }
                })
            } catch (e: Exception) {
                _isLoading.value = false
                _error.value = "Exception: ${e.message}"
            }
        }
    }
}
```

5. Create a Compose UI:

```kotlin
@Composable
fun UserProfileScreen(viewModel: UserViewModel = viewModel()) {
    val userData by viewModel.userData.collectAsState()
    val isLoading by viewModel.isLoading.collectAsState()
    val error by viewModel.error.collectAsState()

    var userId by remember { mutableStateOf("") }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        TextField(
            value = userId,
            onValueChange = { userId = it },
            label = { Text("User ID") },
            modifier = Modifier.fillMaxWidth()
        )

        Spacer(modifier = Modifier.height(16.dp))

        Button(
            onClick = { viewModel.fetchUserData(userId) },
            enabled = userId.isNotEmpty() && !isLoading
        ) {
            Text("Fetch User Data")
        }

        Spacer(modifier = Modifier.height(16.dp))

        when {
            isLoading -> CircularProgressIndicator()
            error != null -> Text(
                text = error ?: "",
                color = Color.Red,
                modifier = Modifier.padding(16.dp)
            )
            userData != null -> UserDataDisplay(userData!!)
        }
    }
}

@Composable
fun UserDataDisplay(user: UserModel) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp),
        elevation = 4.dp
    ) {
        Column(modifier = Modifier.padding(16.dp)) {
            Text(text = "Name: ${user.name}", style = MaterialTheme.typography.h6)
            Text(text = "Email: ${user.email}")
            Text(text = "Age: ${user.age}")
        }
    }
}
```

This example demonstrates how to make API calls in both pure Kotlin and within a Jetpack Compose application. The
Compose example follows modern Android architecture patterns with ViewModel and StateFlow for state management.

??? note "Citations"

      - [1] https://www.delasign.com/blog/android-studio-kotlin-api-call/
      - [2] https://stackoverflow.com/questions/46177133/http-request-in-android-with-kotlin
      - [3] https://dev.to/ethand91/android-jetpack-compose-api-tutorial-1kh5
      - [4] https://rhythamnegi.com/http-request-with-ktor-client-jetpack-compose-android-project-example
      - [5] https://zetcode.com/kotlin/getpostrequest/
      - [6] https://ktor.io/docs/client-requests.html
      - [7] https://developer.android.com/reference/java/net/HttpURLConnection
      - [8] https://stackoverflow.com/questions/45219379/how-to-make-an-api-request-in-kotlin
      - [9] https://dev.to/gaurav-nandankar/how-to-make-api-calls-in-android-using-kotlin-mpd
      - [10] https://www.mobileinsights.dev/making-get-requests-to-a-rest-api-with-kotlin-and-gson-in-android-9005ef75546e
      - [11] https://developer.android.com/develop/ui/compose/mental-model
      - [12] https://www.youtube.com/watch?v=bLIWWOMVxts
      - [13] https://www.youtube.com/watch?v=LEeCcS5qhAs
      - [14] https://www.youtube.com/watch?v=OHdXOoBwLOs
      - [15] https://ktor.io/docs/server-requests-and-responses.html


