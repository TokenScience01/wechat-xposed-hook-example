# Architecture Notes

English | [简体中文](ARCHITECTURE.md)

## Module Loading Flow

```text
Xposed framework
  -> Reads app/src/main/assets/xposed_init
  -> Instantiates HookMain
  -> Calls handleLoadPackage(...)
  -> Checks the current process package name
  -> Registers a method hook inside the WeChat process
```

[`app/src/main/assets/xposed_init`](../app/src/main/assets/xposed_init) declares `HookMain` as the entry class. It implements Xposed's `IXposedHookLoadPackage` interface, so it receives callbacks when application processes load.

## Hook Behavior

[`app/src/main/java/HookMain.java`](../app/src/main/java/HookMain.java) registers `beforeHookedMethod(...)` through `XposedHelpers.findAndHookMethod(...)`:

```text
com.tencent.mm.ui.chatting.o#EM(String)
  -> Reads the first String argument
  -> Writes it to the Xposed log
  -> Returns true early when the argument equals Test
```

`param.setResult(true)` skips the original target method implementation. This branch demonstrates interception and should not be used in a real environment without understanding the target method semantics.

## Compatibility Notes

WeChat internals are obfuscated and are not a stable public API. The class name `com.tencent.mm.ui.chatting.o` and method name `EM` represent only the historical version used when this example was created. They may stop working or represent different logic after a WeChat update.

When maintaining compatibility, document the following from an authorized test environment:

- WeChat version;
- Android version;
- Xposed framework and version;
- Target class name, method name, and parameter signature;
- Verification steps and expected result.

## Third-Party Components

The project uses [`app/lib/XposedBridgeApi-54.jar`](../app/lib/XposedBridgeApi-54.jar) as a compile-time dependency. The archive includes its own Apache License 2.0 statement and third-party notices. The repository root [`NOTICE`](../NOTICE) preserves that information.
