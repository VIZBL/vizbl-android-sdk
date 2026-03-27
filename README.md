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

The SDK is distributed via GitHub Packages.

Add the repository to your `settings.gradle`:

```gradle
dependencyResolutionManagement {
    repositories {
        maven {
            url = uri("https://maven.pkg.github.com/VIZBL/vizbl-android-sdk")
            credentials {
                username = "YOUR_GITHUB_USERNAME"
                password = "YOUR_GITHUB_TOKEN"
            }
        }
    }
}
```

Add dependency:

```gradle
implementation "com.vizbl:sdk:VERSION"
```

## Usage

1. Create and display `VizblARScene`
2. Obtain `VizblARController`
3. Load object using `tinuuid`

```kotlin
controller.loadObject(
    tinuuid = "your-tinuuid",
    hid = null
)
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
