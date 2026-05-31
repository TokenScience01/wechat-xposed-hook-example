# Contributing Guide

English | [简体中文](CONTRIBUTING.md)

Thank you for improving WeChat Xposed Hook Example. This project is an Xposed learning example. Documentation improvements, build fixes, compatibility notes, and reproducible authorized test workflows are especially welcome.

## Before You Start

- Search existing Issues before opening a new one.
- Use only devices, accounts, data, and test environments that you own or are explicitly authorized to test.
- Do not submit real conversations, account details, device identifiers, access tokens, or other sensitive information.
- Document the origin, version, and license of any added third-party code, binaries, or assets.

## Submitting Changes

1. Fork the repository and create a feature branch from the latest revision.
2. Keep changes focused and update the README or `docs/` when needed.
3. Validate changes with tests appropriate to the affected scope.
4. Open a Pull Request and describe the purpose, verification steps, and compatibility details.

The project uses JDK 17, Android SDK Platform 36.1, AGP 9.2.0, and Gradle 9.5.1. Run the baseline verification command:

```bash
./gradlew assembleDebug test lintDebug
```

## Code Guidelines

- Keep the example simple and readable. Avoid dependencies unrelated to the learning goal.
- Do not commit IDE state, build outputs, or logs containing personal information.
- When changing a hook point, record the applicable WeChat version, Android version, and Xposed environment.
- Explain the intended use when a change may affect privacy or security boundaries.

## Out of Scope

This project does not accept features for unauthorized monitoring, bulk message collection, bypassing platform security restrictions, or infringing privacy.

By submitting a contribution, you agree to provide it under the repository's [Apache License 2.0](LICENSE).
