# Android SDK

The SDK confirms that a tester installed the app from Google Play via their personal TestSwap link and checks in
test days automatically. It is optional.

## How it works

1. The tester gets a link like
   `https://play.google.com/store/apps/details?id=<package>&referrer=ts_<token>`.
2. Google Play passes the `ts_<token>` tag to the app via the Install Referrer.
3. On first launch the SDK sends the app key and the tag to `https://testswap.yednamo.com/sdk/v1/install`.
4. On later launches the SDK sends `/sdk/v1/ping` (at most every 3 hours). The server counts one day per day
   in the tester’s time zone.
5. Once the test is over, the SDK stops sending requests.

Users who did not install the app via a TestSwap link send nothing.

## Setup

```kotlin
// settings.gradle.kts
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven { url = uri("https://jitpack.io") }
    }
}
```

```kotlin
// app/build.gradle.kts
dependencies {
    implementation("com.github.yednamo:testswap-sdk-android:0.1.0")
}
```

```kotlin
class App : Application() {
    override fun onCreate() {
        super.onCreate()
        TestSwap.init(this, "tsk_...") // key from the app dashboard on TestSwap
    }
}
```

Register the `App` class in `AndroidManifest.xml` (`android:name=".App"`). The library adds the `INTERNET` permission.

## Data

Only the app key (public, embedded in the build), the participation token and the SDK version are sent. No device
identifiers, advertising IDs or user data.

If your app worked without internet access before, the SDK adds the `INTERNET` permission. Update your privacy policy
and the Data safety section in Play Console. For such apps the mode without SDK is simpler.

## Checking

1. List the app in “With SDK” mode and copy the key.
2. Join the test from another account, install the app via the personal link and open it.
3. The test page should show “The app confirmed the installation”, and the day should be marked as SDK.
