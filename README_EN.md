# WeChat Xposed Hook Example

English | [简体中文](README.md)

[![Android CI](https://github.com/TokenScience01/wechat-xposed-hook-example/actions/workflows/android.yml/badge.svg)](https://github.com/TokenScience01/wechat-xposed-hook-example/actions/workflows/android.yml)

An Android example module for learning the basic Xposed hook workflow. It shows how to locate a chat-related method in a historical WeChat version when the process loads, inspect its argument, and intercept the original call when a specific value is matched.

> [!IMPORTANT]
> This is a historical example created in 2018, not a ready-to-use module for current WeChat versions. WeChat internal class and method names are obfuscated and usually change between releases. Use this project only on devices, accounts, and test environments that you own or are explicitly authorized to test. Follow applicable laws, privacy requirements, and platform rules.

## What It Does

The current example in [`HookMain.java`](app/src/main/java/HookMain.java) performs the following steps:

1. Processes only packages whose names contain `com.tencent.mm`.
2. Hooks `com.tencent.mm.ui.chatting.o#EM(String)` from a historical WeChat version.
3. Writes the input argument to the Xposed log.
4. Calls `param.setResult(true)` to skip the original method and return `true` when the argument equals `Test`.


## Project Status

This repository is intended for code reading, Xposed learning, and authorized testing. The Android project uses a current stable build stack:

| Component | Version |
| --- | --- |
| Android Gradle Plugin | `9.2.0` |
| Gradle Wrapper | `9.5.1` |
| JDK | `17` |
| Android SDK | `compileSdk 36.1`, `targetSdk 36` |
| Xposed API | `54` |

The default baseline is the stable Android 16 QPR2 SDK. Android 17 API 37 is currently a preview and requires preview tooling, so it is not the default build target. The hook point still comes from a historical 2018 WeChat version. Compatibility contributions should document their test environments.

## Build

Make sure `JAVA_HOME` points to JDK 17. Configure the Android SDK path through `ANDROID_HOME` or an untracked `local.properties` file, and install Android SDK Platform `36.1`.

For example, on macOS:

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export ANDROID_HOME="$HOME/Library/Android/sdk"
./gradlew assembleDebug
```

The APK is usually generated at:

```text
app/build/outputs/apk/debug/app-debug.apk
```

## Usage

1. Prepare an Android device or emulator used only for authorized testing, with a compatible Xposed environment.
2. Build and install the APK.
3. Enable the module in your Xposed manager and restart the environment as required by your framework.
4. Trigger the test flow with an authorized account and inspect the Xposed logs.

If the target WeChat version changes, first identify the new class name and method signature within a legally authorized test environment.

## Project Layout

```text
app/src/main/java/HookMain.java                         Xposed entry point and hook logic
app/src/main/assets/xposed_init                         Xposed module entry declaration
app/src/main/AndroidManifest.xml                        Xposed module metadata
app/lib/XposedBridgeApi-54.jar                          Compile-time Xposed API
docs/ARCHITECTURE.md                                    Architecture notes in Chinese
docs/ARCHITECTURE_EN.md                                 Architecture notes in English
```

## Security and Privacy

- Do not use this project to read, collect, store, or distribute unauthorized message content.
- Do not post real conversations, account details, device identifiers, or logs in Issues or Pull Requests.
- This project is not affiliated with, authorized by, or endorsed by WeChat, Tencent, or the Xposed project.
- See [SECURITY_EN.md](SECURITY_EN.md) for security reporting guidance.

## Contributing

Documentation improvements, compatibility notes, and reproducible authorized test workflows are welcome. Read [CONTRIBUTING_EN.md](CONTRIBUTING_EN.md) and [CODE_OF_CONDUCT_EN.md](CODE_OF_CONDUCT_EN.md) before submitting changes. Every Push and Pull Request is checked by GitHub Actions with a build, unit tests, and lint.

## License

Project code and documentation are available under the [Apache License 2.0](LICENSE). Third-party components remain subject to their own licenses. See [NOTICE](NOTICE) for the Xposed API notices distributed with this repository.
