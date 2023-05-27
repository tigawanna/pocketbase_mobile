# Pocketbase Mobile

Pocketbase mobile is used to generate android and ios packages for using pocketbase in mobiles


## Get started

make sure [gomobile](https://pkg.go.dev/golang.org/x/mobile/cmd/gomobile) is installed 

## To build for android

run :  `gomobile bind -androidapi 19`

This will generate two files: `pocketbaseMobile-sources.jar` and `pocketbaseMobile.aar`, import these in android and use

## To build for ios

run : `gomobile bind --target ios`


## Native android setup

add a folder in `Project>app>libs` and add `pocketbaseMobile.aar` file genreated using gomobile

import in app level `build.gradle`

```gradle
dependencies {
    ...
    implementation fileTree(include: ['*.jar', '*.aar'], dir: 'libs')
}
```

## Usage

Use CoroutineScope to call pocketbase methods ( import kotlin coroutines libraries)

```kotlin
private val uiScope = CoroutineScope(Dispatchers.Main + Job())
```

To start pocketbase

```kotlin
// use dataPath where app have write access, for example temporary cache path `context.cacheDir.absolutePath`
uiScope.launch {
    withContext(Dispatchers.IO) {
        PocketbaseMobile.startPocketbase(dataPath, hostname, port)
    }
}
```

To stop pocketbase

```kotlin
uiScope.launch {
    withContext(Dispatchers.IO) {
        PocketbaseMobile.stopPocketbase()
    }
}
```

To listen pocketbase events, and also handle custom api requests

`pocketbaseMobile` have two custom routes as well ,`/api/nativeGet` and `/api/nativePost`, we can
get these routes in this callback and return response from kotlin

```kotlin
PocketbaseMobile.registerNativeBridgeCallback { command, data ->
    this.runOnUiThread {
        // Update ui from here
    }
    // return response back to pocketbase
    "response from native"
}
```
