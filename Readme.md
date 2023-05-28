# Pocketbase Mobile

Pocketbase mobile is used to generate android and ios packages for using pocketbase in mobiles


## To build

Make sure [gomobile](https://pkg.go.dev/golang.org/x/mobile/cmd/gomobile) is installed 

run :  `gomobile bind -androidapi 19` for android

This will generate two files: `pocketbaseMobile-sources.jar` and `pocketbaseMobile.aar`, import these in android and use

run : `gomobile bind --target ios` for ios

or try : `gomobile bind -ldflags='-extldflags=-libresolv.tbd' -target=ios`

# Native android setup

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
// use dataPath where app have write access, for example temporary cache path `context.cacheDir.absolutePath` or filePath
uiScope.launch {
    withContext(Dispatchers.IO) {
        PocketbaseMobile.startPocketbase(dataPath, hostname, port, enableApiLogs)
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


# Native ios setup

Download `PocketbaseMobile.xcframework.zip` and extract, then add this to ios project, checkout [this](https://github.com/golang/go/issues/58416) if you get any error while compiling ios app after including this framework


# Exampels

checkout [pocketbase_server_android_example](https://github.com/rohitsangwan01/pocketbase_server_android_example)

![](https://github.com/rohitsangwan01/pocketbase_mobile/assets/59526499/ff2c277a-bc9e-456c-b089-42fd264f61e3)


# Extras

Checkout a Flutter chatapp built using pocketbase: [flutter_pocketbase_chat](https://github.com/rohitsangwan01/flutter_pocketbase_chat)


