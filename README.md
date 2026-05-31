# WeChat Xposed Hook Example

[English](README_EN.md) | 简体中文

[![Android CI](https://github.com/TokenScience01/wechat-xposed-hook-example/actions/workflows/android.yml/badge.svg)](https://github.com/TokenScience01/wechat-xposed-hook-example/actions/workflows/android.yml)

一个用于学习 Xposed Hook 基本流程的 Android 示例模块。项目演示了如何在微信进程加载时定位历史版本中的聊天相关方法、读取方法参数，并在命中特定内容时拦截原始调用。

> [!IMPORTANT]
> 这是一个创建于 2018 年的历史示例，不是适配新版微信的成品工具。微信内部类名和方法名经过混淆，升级后通常会变化。请仅在你拥有或已获得明确授权的设备、账号和测试环境中使用本项目，并遵守适用法律、隐私要求和平台规则。

## 功能说明

当前示例在 [`HookMain.java`](app/src/main/java/HookMain.java) 中实现以下逻辑：

1. 仅处理包名包含 `com.tencent.mm` 的进程。
2. Hook 历史版本微信中的 `com.tencent.mm.ui.chatting.o#EM(String)` 方法。
3. 将传入参数写入 Xposed 日志。
4. 参数等于 `Test` 时，通过 `param.setResult(true)` 跳过原方法并返回 `true`。


## 项目状态

本仓库主要用于代码阅读、Xposed 入门和授权测试。Android 工程已更新到当前稳定构建栈：

| 项目 | 版本 |
| --- | --- |
| Android Gradle Plugin | `9.2.0` |
| Gradle Wrapper | `9.5.1` |
| JDK | `17` |
| Android SDK | `compileSdk 36.1`、`targetSdk 36` |
| Xposed API | `54` |

默认构建基线使用稳定的 Android 16 QPR2 SDK。Android 17 API 37 当前仍属于预览版，需要预览版 Android Studio 和 SDK，因此没有作为默认构建目标。Hook 点仍然来自 2018 年的历史微信版本；提交 Hook 兼容性改进时，请在 Pull Request 中说明测试环境。

## 构建

构建前请确保 `JAVA_HOME` 指向 JDK 17，并通过 `ANDROID_HOME` 或未纳入版本控制的 `local.properties` 配置 Android SDK 路径。请安装 Android SDK Platform `36.1`。

例如，在 macOS 上可以运行：

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export ANDROID_HOME="$HOME/Library/Android/sdk"
./gradlew assembleDebug
```

构建产物通常位于：

```text
app/build/outputs/apk/debug/app-debug.apk
```

## 使用方式

1. 准备仅用于测试的 Android 设备或模拟器，并安装兼容的 Xposed 环境。
2. 构建并安装 APK。
3. 在 Xposed 管理工具中启用模块，按所用框架要求重启相关环境。
4. 在授权测试账号中触发测试流程，通过 Xposed 日志查看参数记录。

如果目标微信版本发生变化，需要先在合法授权范围内重新确认待 Hook 的类名和方法签名。

## 目录结构

```text
app/src/main/java/HookMain.java                         Xposed 入口与 Hook 逻辑
app/src/main/assets/xposed_init                         Xposed 模块入口声明
app/src/main/AndroidManifest.xml                        Xposed 模块元数据
app/lib/XposedBridgeApi-54.jar                          编译期 Xposed API
docs/ARCHITECTURE.md                                    实现说明
docs/ARCHITECTURE_EN.md                                 Architecture notes in English
```

## 安全与隐私

- 不要使用本项目读取、收集、保存或传播未经授权的消息内容。
- 不要将真实聊天内容、账号信息、设备标识或日志提交到 Issue 和 Pull Request。
- 本项目与微信、腾讯或 Xposed 项目不存在隶属、授权或背书关系。
- 安全问题请参考 [SECURITY.md](SECURITY.md)。

## 参与贡献

欢迎改进文档、兼容性说明和可复现的测试流程。提交前请阅读 [CONTRIBUTING.md](CONTRIBUTING.md) 和 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。每次 Push 和 Pull Request 都会通过 GitHub Actions 执行构建、单元测试和 lint。

## 开源许可

项目自身代码和文档采用 [Apache License 2.0](LICENSE) 开源。仓库包含的第三方组件仍适用各自的许可条款；随仓库分发的 Xposed API 声明见 [NOTICE](NOTICE)。
