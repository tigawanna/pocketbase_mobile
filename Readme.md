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

![](https://github.com/rohitsangwan01/pocketbase_mobile/assets/59526499/39aeb478-a291-4354-9387-30d8992ee7f9)


# Exampels


checkout [Pocketbase Server Flutter](https://github.com/rohitsangwan01/pocketbase_server_flutter) for android and ios implementation in flutter

<img src="https://github.com/rohitsangwan01/pocketbase_server_flutter/assets/59526499/7d20a2a4-0df7-4f2a-90bf-2577289e0f7e" height="300">
<img src="https://github.com/rohitsangwan01/pocketbase_server_flutter/assets/59526499/370c007d-51c3-45a9-928c-1287c8def0d3" height="300">
<img src="https://github.com/rohitsangwan01/pocketbase_server_flutter/assets/59526499/657a6e4c-8431-4f49-b29d-a0f599524f6c" height="300">
<img src="https://github.com/rohitsangwan01/pocketbase_server_flutter/assets/59526499/4ecd5f1c-ae2b-4406-a10d-0d9ae3e9900e" height="300">
<img src="https://github.com/rohitsangwan01/pocketbase_mobile/assets/59526499/5ec533af-1b6f-4c79-afd8-e3e65e2d55a1" height="300">
<img src="https://github.com/rohitsangwan01/pocketbase_server_flutter/assets/59526499/f58f7f5e-d3d0-4328-a8be-f5cf12e15cdb" height="300">

checkout [Pocketbase Server Android](https://github.com/rohitsangwan01/pocketbase_server_android_example) for native android implementation

<img src="https://github.com/rohitsangwan01/pocketbase_mobile/assets/59526499/ff2c277a-bc9e-456c-b089-42fd264f61e3" height="300">
<img src="https://github.com/rohitsangwan01/pocketbase_mobile/assets/59526499/93b668c8-600f-4232-b2bb-3562ccbde32e" height="300">

# Extras

Checkout a Flutter chatapp built using pocketbase: [flutter_pocketbase_chat](https://github.com/rohitsangwan01/flutter_pocketbase_chat)


