# 17. Displaying Remote Images and Videos

[Git Repository](https://github.com/profdenis/ImagesVideos)

## Displaying an Image from a URL in Jetpack Compose

### Prerequisites

To display an image from a URL, we need to:

1. Add the Coil dependency in the `build.gradle` file (app level)
2. Add Internet permission in `AndroidManifest.xml`

```gradle
// build.gradle
dependencies {
    implementation("io.coil-kt:coil-compose:2.5.0")
}
```

```xml
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.INTERNET"/>
```

### Basic Composable

Here's a simple usage example:

```kotlin
import androidx.compose.foundation.layout.size
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import coil.compose.AsyncImage

@Composable
fun RemoteImage() {
    AsyncImage(
        model = "https://example.com/image.jpg",
        contentDescription = "Image description",
        modifier = Modifier.size(200.dp)
    )
}
```

### More Complete Version with Loading Management

```kotlin
import androidx.compose.foundation.layout.*
import androidx.compose.material3.CircularProgressIndicator
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.layout.ContentScale
import androidx.compose.ui.unit.dp
import coil.compose.SubcomposeAsyncImage

@Composable
fun AdvancedRemoteImage() {
    SubcomposeAsyncImage(
        model = "https://example.com/image.jpg",
        contentDescription = "Image description",
        modifier = Modifier.size(200.dp),
        contentScale = ContentScale.Fit,
        loading = {
            Box(
                modifier = Modifier.fillMaxSize(),
                contentAlignment = Alignment.Center
            ) {
                CircularProgressIndicator()
            }
        },
        error = {
            // You can customize the error display
            Box(
                modifier = Modifier.fillMaxSize(),
                contentAlignment = Alignment.Center
            ) {
                Text("Loading error")
            }
        }
    )
}
```

### Important Points

1. **Coil** is an image loading library for Android, optimized for Kotlin and Jetpack Compose.

2. Two main composables are available:
    - `AsyncImage`: simple version for basic cases
    - `SubcomposeAsyncImage`: advanced version allowing management of loading states

3. Main parameters:
    - `model`: the image URL
    - `contentDescription`: description for accessibility
    - `modifier`: to customize size and appearance
    - `contentScale`: to define how the image should adapt to its container

4. For testing, here are some royalty-free image URLs:

```kotlin
"https://picsum.photos/200"  // Random 200x200 image
"https://via.placeholder.com/200"  // 200x200 placeholder image
```

### Complete Usage Example

```kotlin
@Composable
fun ExampleImageScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        // Simple image
        AsyncImage(
            model = "https://picsum.photos/200",
            contentDescription = "Random image",
            modifier = Modifier.size(200.dp)
        )

        // Image with loading management
        SubcomposeAsyncImage(
            model = "https://picsum.photos/300",
            contentDescription = "Random image with loading",
            modifier = Modifier.size(300.dp),
            loading = { CircularProgressIndicator() }
        )
    }
}
```

## Video Integration in Jetpack Compose

### Playing MP4 Videos with ExoPlayer

To play MP4 videos, we'll use ExoPlayer, which is recommended by Google for media playback on Android.

First, add the dependencies in `build.gradle`:

```gradle
dependencies {
    implementation("androidx.media3:media3-exoplayer:1.2.0")
    implementation("androidx.media3:media3-ui:1.2.0")
}
```

Then, create the video composable:

```kotlin
import androidx.compose.runtime.*
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.viewinterop.AndroidView
import androidx.media3.common.MediaItem
import androidx.media3.common.Player
import androidx.media3.exoplayer.ExoPlayer
import androidx.media3.ui.PlayerView

@Composable
fun VideoPlayer(
    videoUrl: String,
    modifier: Modifier = Modifier
) {
    val context = LocalContext.current

    // Creating ExoPlayer
    val exoPlayer = remember {
        ExoPlayer.Builder(context).build().apply {
            setMediaItem(MediaItem.fromUri(videoUrl))
            prepare()
        }
    }

    // Lifecycle management
    DisposableEffect(Unit) {
        onDispose {
            exoPlayer.release()
        }
    }

    // Player user interface
    AndroidView(
        factory = { context ->
            PlayerView(context).apply {
                player = exoPlayer
            }
        },
        modifier = modifier
    )
}
```

Usage:

```kotlin
@Composable
fun VideoPlayerScreen() {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {
        // Example with MP4 video
        VideoPlayer(
            videoUrl = "https://example.com/video.mp4",
            modifier = Modifier
                .fillMaxWidth()
                .aspectRatio(16f / 9f)
        )
    }
}
```

### Complete Version with Error and Loading Management

```kotlin
@Composable
fun AdvancedVideoPlayer(
    videoUrl: String,
    modifier: Modifier = Modifier
) {
    var isLoading by remember { mutableStateOf(true) }
    var hasError by remember { mutableStateOf(false) }
    val context = LocalContext.current

    val exoPlayer = remember {
        ExoPlayer.Builder(context).build().apply {
            addListener(object : Player.Listener {
                override fun onPlaybackStateChanged(state: Int) {
                    when (state) {
                        Player.STATE_READY -> isLoading = false
                        Player.STATE_ENDED -> { /* Handle end */
                        }
                        Player.STATE_BUFFERING -> isLoading = true
                        Player.STATE_IDLE -> { /* Initial state */
                        }
                    }
                }

                override fun onPlayerError(error: PlaybackException) {
                    hasError = true
                    isLoading = false
                }
            })

            setMediaItem(MediaItem.fromUri(videoUrl))
            prepare()
        }
    }

    DisposableEffect(Unit) {
        onDispose {
            exoPlayer.release()
        }
    }

    Box(modifier = modifier) {
        AndroidView(
            factory = { context ->
                PlayerView(context).apply {
                    player = exoPlayer
                }
            },
            modifier = Modifier.matchParentSize()
        )

        if (isLoading) {
            CircularProgressIndicator(
                modifier = Modifier.align(Alignment.Center)
            )
        }

        if (hasError) {
            Text(
                text = "Video playback error",
                modifier = Modifier.align(Alignment.Center),
                color = Color.Red
            )
        }
    }
}
```

### Important Points

1. Add Internet permission in `AndroidManifest.xml`:
   ```xml
   <uses-permission android:name="android.permission.INTERNET"/>
   ```

2. For ExoPlayer:
    - It's Google's recommended solution
    - Supports many video formats
    - Handles caching automatically
    - Offers advanced playback controls

3. Performance Considerations:
    - Videos consume a lot of resources
    - Important to properly manage lifecycle
    - Plan for cache and bandwidth management