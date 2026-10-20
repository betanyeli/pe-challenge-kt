# Android Hotwire Native shell

This Android app wraps the Rails-rendered application flow with Hotwire Native. It loads the same
web pages as the browser app; the application flow and its data remain in Rails/web.

## Run against the local Rails app

Start the web project with a `DEV_HOST` that matches the address Android will use to reach the Mac.
The Rails server binds to `0.0.0.0:3000`; Vite serves development assets on port `5173`.

### Android Emulator

Use Android Emulator's host alias `10.0.2.2`:

```sh
DEV_HOST=10.0.2.2 npm run dev:all
```

In `app/src/main/java/com/beta/pe_challenge/MainActivity.kt`, set `startLocation` to
`http://10.0.2.2:3000`.

### Physical Android device

Put the device and Mac on the same Wi-Fi network. Set `DEV_HOST` to the Mac's current LAN IP and
use that IP in `startLocation`, for example:

```sh
DEV_HOST=192.168.1.96 npm run dev:all
```

```kotlin
startLocation = "http://192.168.1.96:3000"
```

Update both values if the Mac's LAN IP changes.

## Run the app

Open this directory in Android Studio, sync Gradle, select an emulator or connected device, and run
the `app` configuration. The app needs the `INTERNET` permission, which is declared in the main
manifest.

## Local networking

HTTP cleartext traffic is enabled only by `app/src/debug/AndroidManifest.xml`, so the local Rails
URL works in Debug. Release builds do not have this exception. Use the production HTTPS host for
production builds; do not ship a local IP or enable cleartext traffic in Release.

## App entry points

- `app/src/main/java/com/beta/pe_challenge/MainActivity.kt` configures the Hotwire Native navigator
  and its start URL.
- `app/src/main/res/layout/activity_main.xml` declares the `NavigatorHost`.
- `gradle/libs.versions.toml` declares Hotwire Native Android dependencies.
