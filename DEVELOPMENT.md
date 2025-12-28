# IndiePass Android Development Guide

## Prerequisites

### 1. Install Java 17

```bash
brew install openjdk@17
```

### 2. Install Android Command Line Tools

```bash
brew install --cask android-commandlinetools
```

### 3. Install Android SDK Components

```bash
export JAVA_HOME="/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"
yes | sdkmanager --sdk_root=/opt/homebrew/share/android-commandlinetools \
    "platforms;android-34" \
    "build-tools;34.0.0" \
    "platform-tools"
```

### 4. Create local.properties

Create a file called `local.properties` in the project root:

```
sdk.dir=/opt/homebrew/share/android-commandlinetools
```

## Building the App

Set Java home and run Gradle:

```bash
export JAVA_HOME="/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"
./gradlew assembleDebug
```

This creates two APK variants:
- `app/build/outputs/apk/fdroid/debug/app-fdroid-debug.apk` (F-Droid version)
- `app/build/outputs/apk/playstore/debug/app-playstore-debug.apk` (Play Store version)

## Installing to Your Phone

### 1. Enable USB Debugging on Your Phone

1. Go to **Settings > About Phone**
2. Tap **Build Number** 7 times to enable Developer Options
3. Go to **Settings > Developer Options**
4. Enable **USB Debugging**

### 2. Connect Your Phone

Connect via USB cable. When prompted on the phone, allow USB debugging from your computer.

### 3. Install the APK

```bash
# Using full path to adb
/opt/homebrew/share/android-commandlinetools/platform-tools/adb install -r \
    app/build/outputs/apk/fdroid/debug/app-fdroid-debug.apk
```

Or add adb to your PATH (add to `~/.zshrc`):

```bash
export PATH="$PATH:/opt/homebrew/share/android-commandlinetools/platform-tools"
```

Then you can just run:

```bash
adb install -r app/build/outputs/apk/fdroid/debug/app-fdroid-debug.apk
```

## Quick Reference

Build and install in one command:

```bash
export JAVA_HOME="/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"
./gradlew assembleDebug && \
    /opt/homebrew/share/android-commandlinetools/platform-tools/adb install -r \
    app/build/outputs/apk/fdroid/debug/app-fdroid-debug.apk
```

## Troubleshooting

### "SDK location not found"

Make sure `local.properties` exists with the correct SDK path.

### "device unauthorized"

Check your phone for a dialog asking to authorize USB debugging. Tap "Allow".

### "INSTALL_FAILED_UPDATE_INCOMPATIBLE"

The installed app was signed with a different key. Uninstall it first:

```bash
adb uninstall com.indieweb.indigenous
```

Then install again.

### "Unable to locate a Java Runtime"

Make sure to set JAVA_HOME before running Gradle:

```bash
export JAVA_HOME="/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"
```
