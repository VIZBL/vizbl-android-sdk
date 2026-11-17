# Vizbl Android SDK

Vizbl Android SDK allows you to embed a fully interactive AR experience into your Android application and place Vizbl objects in the real world.

The SDK handles AR, placement, interaction, and UI automatically.

## Features

- real-time AR object placement
- floor and wall placement
- automatic placement guidance
- built-in interaction and controls
- material / variant selection UI

## Installation

The SDK is published to Maven Central. Current version: **0.2**. Minimum Android SDK: 27 (Android 8.1); the device must support ARCore.

Make sure `mavenCentral()` is in your repositories (it is in every project Android Studio creates), then add the dependency to your module's `build.gradle.kts`:

```kotlin
implementation("com.vizbl:ar-sdk:0.2")
```

No tokens or extra repositories are needed. (Older versions were published to GitHub Packages under `com.us.vizbl.ar.sdk:ar-sdk`; that repository still works but is no longer updated.)

The SDK ships ARCore, SceneView and Jetpack Compose as transitive dependencies, so your app must use Compose and target Java 17 or later.

## Minimal integration

`VizblARScene` is a composable that runs the whole AR flow. Add an object once the session is ready; `add` is a suspend function, so call it from a coroutine scope.

```kotlin
import androidx.compose.runtime.*
import com.us.vizbl.ar.sdk.api.ARObjectIdentifier
import com.us.vizbl.ar.sdk.api.ARObjectReference
import com.us.vizbl.ar.sdk.api.VizblARController
import com.us.vizbl.ar.sdk.api.VizblARScene
import kotlinx.coroutines.launch

@Composable
fun ARScreen(tinuuid: String) {
    val scope = rememberCoroutineScope()
    var controller by remember { mutableStateOf<VizblARController?>(null) }

    VizblARScene(
        onControllerReady = { controller = it },
        onSessionReady = {
            scope.launch {
                controller?.add(
                    model = ARObjectReference.Single(
                        id = ARObjectIdentifier.Tinuuid(tinuuid),
                        materialId = null
                    ),
                    configuration = {
                        allowsTapToSelected(true)
                        allowsMove(true)
                        allowRotation(true)
                    }
                )
            }
        }
    )
}
```

`tinuuid` is the Object ID of a model in your Vizbl library (shown in the Vizbl cabinet and returned by the API). `ARObjectIdentifier.ObjectId(UUID)` works the same way with the UUID form.

Ask for the camera permission before showing the scene. The SDK handles the AR session, surface detection, placement, interaction and the on-screen controls.

## Documentation

Full Android SDK documentation:  
https://developer.vizbl.com/docs/android

## Demo

https://github.com/VIZBL/vizbl-android-demo

## Requirements

- Android device with AR support
- Camera permission
