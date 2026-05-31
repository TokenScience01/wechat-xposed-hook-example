# 实现说明

[English](ARCHITECTURE_EN.md) | 简体中文

## 模块加载流程

```text
Xposed 框架
  -> 读取 app/src/main/assets/xposed_init
  -> 实例化 HookMain
  -> 调用 handleLoadPackage(...)
  -> 检查当前进程包名
  -> 在微信进程中注册方法 Hook
```

[`app/src/main/assets/xposed_init`](../app/src/main/assets/xposed_init) 中声明了入口类 `HookMain`。该类实现 Xposed 的 `IXposedHookLoadPackage` 接口，因此每次应用进程加载时都可以收到回调。

## Hook 行为

[`app/src/main/java/HookMain.java`](../app/src/main/java/HookMain.java) 使用 `XposedHelpers.findAndHookMethod(...)` 注册 `beforeHookedMethod(...)`：

```text
com.tencent.mm.ui.chatting.o#EM(String)
  -> 读取第一个 String 参数
  -> 写入 Xposed 日志
  -> 参数等于 Test 时提前返回 true
```

`param.setResult(true)` 会跳过目标方法的原始实现。这个分支适合演示“拦截”概念，不应在不了解目标方法语义时用于真实环境。

## 兼容性提示

微信内部实现经过混淆，不属于稳定公开 API。类名 `com.tencent.mm.ui.chatting.o` 和方法名 `EM` 仅代表示例创建时的历史版本。微信升级后，Hook 点可能失效，也可能对应不同逻辑。

如果需要维护兼容性，请在授权测试环境中记录：

- 微信版本；
- Android 版本；
- Xposed 框架及版本；
- 目标类名、方法名和参数签名；
- 验证步骤和预期结果。

## 第三方组件

项目将 [`app/lib/XposedBridgeApi-54.jar`](../app/lib/XposedBridgeApi-54.jar) 作为编译期依赖。该文件内包含自己的 Apache License 2.0 声明和第三方 notices，仓库根目录的 [`NOTICE`](../NOTICE) 保留了这些信息。
