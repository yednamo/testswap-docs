# Android SDK

The SDK uses Google Play Integrity to confirm that a tester installed the app from Google Play via their personal
TestSwap link and opens it every day on a real, certified device. Test days are checked in automatically.
It is optional.

## How it works

1. The tester gets a link like
   `https://play.google.com/store/apps/details?id=<package>&referrer=ts_<token>`.
2. Google Play passes the `ts_<token>` tag to the app via the Install Referrer, so TestSwap knows whose participation it is.
3. Once a day, when the tester opens the app (an Activity starts), the SDK gets a one-time code from TestSwap
   (`/sdk/v2/challenge`) and requests a Play Integrity token with that code from Google Play (a classic request).
   A process started in the background without opening a screen does not check in the day. The day is counted
   in the time zone the tester chose on the website when joining the test, not by the device’s clock.
4. The SDK sends the token to `/sdk/v2/checkin`. TestSwap decrypts it with your app’s keys and checks in the day
   only if Google confirms all of the following:
   - the request was made for your package with this one-time code, and it is fresh;
   - Google Play recognized the build (`PLAY_RECOGNIZED`);
   - the account got the app from Google Play (`LICENSED`);
   - the device is certified (`MEETS_DEVICE_INTEGRITY` or `MEETS_STRONG_INTEGRITY`).
5. Once the test is over, the SDK stops sending requests.

Users who did not install the app via a TestSwap link send nothing.

A test in SDK mode counts only if the app passed a Play Integrity check on the tester’s device at least once.
In SDK mode only the SDK checks in days: there is no manual check-in button on the website for such tests.

## Requirements

- The app’s `minSdk` is 26 (Android 8.0) or higher: the system certificate stores of older versions do not trust
  the certificate chain of the TestSwap site, so the SDK cannot connect to the server.
- The app is distributed through Google Play (closed testing works).
- The tester’s device has the Google Play Store and Google Play services.

## 1. Play Integrity keys

TestSwap verifies your app’s tokens itself, so it needs your app’s Play Integrity response keys.

1. In Play Console, open your app’s Play Integrity API section and make sure the API is enabled.
2. In the classic request settings (response encryption), choose to manage your own encryption keys instead of
   Google-managed ones and download them. Step by step: https://developer.android.com/google/play/integrity/classic
3. In the app dashboard on TestSwap, paste the **decryption key** and the **verification key** (both base64) and save.

Until the keys are added, the app is not shown in the catalog and does not accept testers.

The decryption key is stored encrypted and is never shown again. If your own server also decrypts classic
Play Integrity tokens, it has to switch to the same keys. Standard requests are not affected.

## 2. Setup

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
    implementation("com.github.yednamo:testswap-sdk-android:0.2.2")
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

Versions 0.1.x are no longer supported: the server answers their requests with 410.

## Data

Only the app key (public, embedded in the build), the participation token, the Google Play Integrity token and the
SDK version are sent. TestSwap stores the verification result (passed or not, and the rejection reason) and the SDK
version for each check-in, and for the app, the text of the last verification error, which the developer sees in the
dashboard. The client IP only ends up in ordinary request logs, which rotate (about 30 MB). No device identifiers,
advertising IDs or user data are stored.

The Integrity token is produced by Google Play. Describe your use of the Play Integrity API and TestSwap in your
privacy policy and the Data safety section in Play Console. If your app worked without internet access before,
the SDK adds the `INTERNET` permission; for such apps the mode without SDK is simpler.

## Checking

1. List the app in “With SDK” mode, copy the app key and add the Play Integrity keys.
2. Join the test from another account, install the app via the personal link and open it.
3. The test page should show “The app confirmed the installation through Google Play Integrity”, and the dashboard
   the time of the last successful verification.
4. If verification fails, the dashboard shows the reason of the last rejection (for example, keys of another app,
   not installed from Google Play, the device failed the integrity check).
