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

The SDK is distributed via GitHub Packages. Current version: **0.2**.

GitHub Packages requires authentication even for public packages. Create a GitHub personal access token with the `read:packages` scope and keep it out of your repository, for example in `~/.gradle/gradle.properties`:

```properties
gpr.user=YOUR_GITHUB_USERNAME
gpr.key=YOUR_GITHUB_TOKEN
```

Add the repository to your `settings.gradle.kts`:

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven {
            url = uri("https://maven.pkg.github.com/VIZBL/vizbl-android-sdk")
            credentials {
                username = providers.gradleProperty("gpr.user").orNull ?: System.getenv("GITHUB_ACTOR")
                password = providers.gradleProperty("gpr.key").orNull ?: System.getenv("GITHUB_TOKEN")
            }
        }
    }
}
```

Never commit the token to source control.

Add the dependency to your module's `build.gradle.kts`:

```kotlin
implementation("com.us.vizbl.ar.sdk:ar-sdk:0.2")
```

## Minimal integration

```kotlin
@Composable
fun ARScreen() {
    var controller: VizblARController? = null

    VizblARScene(
        onControllerReady = { controller = it },
        onSessionReady = {
            controller?.let {
                val model = ARObjectReference.Single(
                    id = ARObjectIdentifier.Tinuuid("your-tinuuid"),
                    materialId = null
                )

                // launch the suspend add call from your coroutine scope
            }
        }
    )
}
```

The SDK handles:

- AR session
- surface detection
- placement
- interaction
- UI

## Documentation

Full Android SDK documentation:  
https://developer.vizbl.com/docs/android

## Demo

https://github.com/VIZBL/vizbl-android-demo

## Requirements

- Android device with AR support
- Camera permission
