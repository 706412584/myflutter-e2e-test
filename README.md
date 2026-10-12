# MyFlutter

由 **Android Code Studio** 的 *Flutter* 模板生成：
用 Dart 写一份代码，构建 Android APK 与 Web 站点。

## 重要：本项目在手机上不构建，由 GitHub Actions 云端出包

Flutter 的构建**必须依赖完整的 Flutter SDK**（Dart 编译器 + 引擎工具链，
官方不支持 Android 宿主机）——手机上无法安装。因此：

- **在 ACS 里编辑 `lib/main.dart`**
- **推送到 GitHub**，`.github/workflows/flutter.yml` 自动构建
- **在 Actions 页面下载产物**：`MyFlutter-android-apk`（APK）与
  `MyFlutter-web`（网页站点，可直接部署到任意静态托管）

> 在手机上点「构建」会报 `flutter.sdk not set in local.properties`
> ——这是预期行为，不是配置错误。

## 代码补全与诊断（Dart LSP）

ACS 支持 Dart 语言服务器（基于官方 Dart SDK 的分析服务器，
提供补全/诊断/跳转/格式化）。**需要先安装 Dart SDK**（约 67MB，
解压后约 169MB）——它不是 APK 内置组件：

1. 打开 **设置 → 编辑器 → 语言服务器 → Dart / Flutter**
2. 点击「下载」等待完成（安装在应用私有目录，不占用项目空间）
3. 重新打开本项目的 `.dart` 文件，补全与诊断即生效

安装后打开 `lib/main.dart` 即可看到：输入 `setState` / `Widget` 等
标识符时的补全、未定义变量的红线提示、`Ctrl+点击` 跳转定义。
分析基于项目内的 `pubspec.yaml`——Flutter SDK 自身的包
（`package:flutter/...`）在设备上未安装，因此对它们的引用会提示
未解析，这是预期行为（云端 CI 构建不受影响）。

## 项目结构

```
MyFlutter/
├── lib/main.dart            # 你的 Dart 代码（编辑这里）
├── pubspec.yaml             # 依赖声明
├── test/widget_test.dart    # 冒烟测试
├── web/                     # Web 平台（index.html / manifest）
└── android/                 # Android 平台（Gradle 工程）
    ├── settings.gradle.kts  # flutter.sdk 引入点
    └── app/                 # 模块（MainActivity / manifest / res）
```

## 云端构建说明

workflow 使用 `subosito/flutter-action@v2` 安装 Flutter
3.47.7（与本地工程一致），跑 `flutter build apk --release`
与 `flutter build web --release`。

- 公共仓库 Actions 免费（含 artifact 存储）
- 首次构建约 5–10 分钟（含 Flutter SDK 下载缓存）
- 如需 iOS：需要 macOS runner + Apple 开发者账号（workflow 里可扩展）

## 版本组合

| 组件 | 版本 |
|---|---|
| Flutter | 3.47.7 |
| Dart | 3.13.5 |
| Gradle | 9.3.1 |
| Android Gradle Plugin | 9.1.0 |
| Kotlin | 2.4.0 |
| 包名 | com.example.myflutter |
