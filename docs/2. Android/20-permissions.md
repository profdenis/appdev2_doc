# 20. Requesting permissions

## Android Permission Request Process  

### **1. Internet Permissions**  
For internet access, declare these **normal permissions** (automatically granted at install time) in `AndroidManifest.xml`:  
```xml
<uses-permission android:name="android.permission.INTERNET" />  
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
No runtime request is needed for these permissions[1][8].  

### **2. Storage Permissions**  
Storage permissions vary by Android version:  

**For Android 12 and below**:  
```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />  
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"  
                 android:maxSdkVersion="32" />
```

**For Android 13+**:  
Replace broad storage permissions with granular media-type permissions:  
```xml
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />  
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />  
<uses-permission android:name="android.permission.READ_MEDIA_AUDIO" />
```

**Runtime Request (Kotlin Example)**:  
```kotlin
private fun requestStoragePermissions() {  
    val permissions = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {  
        arrayOf(  
            Manifest.permission.READ_MEDIA_IMAGES,  
            Manifest.permission.READ_MEDIA_VIDEO  
        )  
    } else {  
        arrayOf(Manifest.permission.READ_EXTERNAL_STORAGE)  
    }  

    ActivityCompat.requestPermissions(  
        this,  
        permissions,  
        STORAGE_PERMISSION_REQUEST_CODE  
    )  
}  
```

**Handling Denials**:  
If permissions are denied, guide users to app settings via:  
```kotlin
val intent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS).apply {  
    data = Uri.fromParts("package", packageName, null)  
}  
startActivity(intent)  
```

### **Key Notes**  
- **Internet permissions** require no runtime checks[8].  
- **Storage permissions** need dynamic handling for Android 13+ compatibility[5][7].  
- Use `maxSdkVersion` for legacy storage permissions to avoid redundancy[7].  


??? note "Citations"

      - [1] https://cknotes.com/dont-forget-to-give-your-android-application-internet-permissions/
      - [2] https://www.youtube.com/watch?v=8nzlIDybGwM
      - [3] https://www.youtube.com/watch?v=7vrCYXj3ma0
      - [4] https://techboltify.com/how-to-enable-storage-permission-in-android/
      - [5] https://sentry.io/answers/storage-permissions-in-android-13/
      - [6] https://www.bluespace.tech/blog/internet-permission/
      - [7] https://sreyas.com/blog/permission-for-storage-in-android-13-or-higher/
      - [8] https://developer.android.com/develop/connectivity/network-ops/connecting
      - [9] https://developer.android.com/training/permissions/declaring
      - [10] https://developer.android.com/reference/android/Manifest.permission
      - [11] https://learn.microsoft.com/en-us/dotnet/api/android.manifest.permission?view=net-android-34.0
      - [12] https://www.reddit.com/r/androidapps/comments/12dixs7/how_can_i_block_an_android_app_from_the_internet/
      - [13] https://developer.android.com/training/permissions/requesting
      - [14] https://developer.android.com/training/data-storage/manage-all-files
      - [15] https://support.google.com/googleplay/answer/9431959
      - [16] https://discussions.unity.com/t/unity-removes-android-permission-internet-in-the-build-apk-after-build-is-completed/924798
      - [17] https://stackoverflow.com/questions/2169294/how-to-add-manifest-permission-to-an-application
      - [18] https://www.youtube.com/watch?v=LIhDB035X4M
      - [19] https://stackoverflow.com/questions/2378607/what-permission-do-i-need-to-access-internet-from-an-android-application
      - [20] https://www.youtube.com/watch?v=j247eYZAaHo
      - [21] https://www.youtube.com/watch?v=55XRw87I5N4
      - [22] https://forum.juce.com/t/android-permission-write-external-storage-should-be-allowed-on-sdk-29/54372

## Example : reading and writing files on external storage

```kotlin
package permissions

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.rememberLauncherForActivityResult
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.activity.result.contract.ActivityResultContracts
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Button
import androidx.compose.material3.Scaffold
import androidx.compose.material3.Text
import androidx.compose.material3.TextField
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.unit.dp
import permissions.ui.theme.PermissionsTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            PermissionsTheme {
                Scaffold(modifier = Modifier.fillMaxSize()) { innerPadding ->
                    TextFileSaver(
                        modifier = Modifier.padding(innerPadding)
                    )
                }
            }
        }
    }
}

@Composable
fun TextFileSaver(modifier: Modifier = Modifier) {
    var text by remember { mutableStateOf("") }
    var status by remember { mutableStateOf("") }
    val context = LocalContext.current

    // For saving files
    val createFileLauncher = rememberLauncherForActivityResult(
        ActivityResultContracts.CreateDocument("text/plain")
    ) { uri ->
        uri?.let {
            try {
                context.contentResolver.openOutputStream(uri)?.use { stream ->
                    stream.write(text.toByteArray())
                }
                status = "File saved successfully!"
            } catch (e: Exception) {
                status = "Error saving: ${e.localizedMessage}"
            }
        }
    }

    // New: For reading files
    val openFileLauncher = rememberLauncherForActivityResult(
        ActivityResultContracts.OpenDocument()
    ) { uri ->
        uri?.let {
            try {
                context.contentResolver.openInputStream(uri)?.use { stream ->
                    text = stream.bufferedReader().use { it.readText() }
                    status = "File loaded successfully!"
                }
            } catch (e: Exception) {
                status = "Error reading: ${e.localizedMessage}"
            }
        }
    }

    Column(
        modifier = modifier
            .padding(16.dp)
            .fillMaxSize(),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        TextField(
            value = text,
            onValueChange = { text = it },
            modifier = Modifier
                .fillMaxWidth()
                .height(200.dp),
            placeholder = { Text("Enter your text here") }
        )

        Row(
            horizontalArrangement = Arrangement.spacedBy(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Button(onClick = {
                openFileLauncher.launch(arrayOf("text/plain"))
            }) {
                Text("Load File")
            }

            Button(onClick = {
                createFileLauncher.launch("public_file.txt")
            }) {
                Text("Save File")
            }
        }
        Text(status)
    }
}
```