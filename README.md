# 小呆象棋 3.0

**一款完全离线运行的中国象棋 Android 应用 · 人机对弈 / 双人对弈 · 本地 AI 引擎 · 无广告、无网络依赖**

![Platform](https://img.shields.io/badge/Platform-Android%207.0%2B-3DDC84?logo=android&logoColor=white)
![Language](https://img.shields.io/badge/Kotlin-2.0.21-7F52FF?logo=kotlin&logoColor=white)
![MinSdk](https://img.shields.io/badge/minSdk-24-blue)
![TargetSdk](https://img.shields.io/badge/targetSdk-34-blue)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 目录

- [项目简介](#项目简介)
- [功能特性](#功能特性)
- [应用信息](#应用信息)
- [快速开始](#快速开始)
- [项目结构](#项目结构)
- [技术架构](#技术架构)
- [可选二进制资源](#可选二进制资源)
- [签名](#签名)
- [版本说明](#版本说明)
- [常见问题](#常见问题)
- [参与贡献](#参与贡献)
- [许可证](#许可证)
- [免责声明](#免责声明)

---

## 项目简介

小呆象棋是一款面向 Android 平台的中国象棋应用。棋局判定、走子规则、AI 搜索与语音播报全部在设备本地完成，应用不依赖任何在线服务，也不采集用户数据。

本仓库为**纯净源码仓库**：仅包含 Kotlin / Java / XML / Gradle 脚本等纯文本文件，**不包含 `.so`、`.onnx`、`.ttf`、`.wav`、`.png`、`.apk`、`.keystore` 等任何二进制产物**，便于代码审阅、二次开发与持续集成。

已编译、已签名的安装包不在仓库内，请前往本仓库的 **Releases** 页面下载。

## 功能特性

### 对弈模式

| 模式 | 说明 |
| --- | --- |
| 人机对弈 | 由本地 Pikafish 引擎（UCI 协议）计算着法，支持难度分级，思考时间随局面复杂度自适应 |
| 双人对弈 | 同一设备上红黑双方轮流走子，适合面对面对弈 |

### 棋局能力

| 能力 | 说明 |
| --- | --- |
| 完整走子规则 | 马腿、象眼、炮架、将帅照面、过河兵等规则逐项实现 |
| 规则判定 | 将军、将死、困毙、长将判负、长捉判负、重复局面判和 |
| 悔棋 | 按回合回退；人机模式下同时撤销 AI 着法 |
| 提示 | 调用引擎给出推荐着法 |
| 棋钟 | 双方独立计时，切换走子方自动交接 |
| 中文记谱 | 以标准中文记谱法记录每一步，对局中可随时查看 |
| 棋盘翻转 | 一键切换视角，适配执黑方视角 |
| 语音播报 | 基于 sherpa-onnx 的离线中文语音，播报走子、将军、胜负等状态 |
| 音效与语音开关 | 落子、吃子音效可静音；语音播报可独立开关 |

> 本版本已**移除全部背景音乐**及相关音乐菜单，应用内不再包含任何音乐资源与音乐播放逻辑。

## 应用信息

| 项目 | 值 |
| --- | --- |
| 应用名称 | 小呆象棋 |
| 版本 | 3.0（versionCode 3） |
| 包名 | `xiaodaixiangqi.zhongjiban` |
| 最低系统版本 | Android 7.0（API 24） |
| 目标系统版本 | Android 14（API 34） |
| 支持架构 | `arm64-v8a` |
| 语言 | Kotlin + Java |
| 网络权限 | 无 |

## 快速开始

### 环境要求

| 组件 | 版本 |
| --- | --- |
| JDK | 17 |
| Android SDK Platform | 34 |
| Android Build-Tools | 34.0.0 |
| Gradle | 8.9 |
| Android Gradle Plugin | 8.7.3 |
| Kotlin | 2.0.21 |

### 从源码构建

```bash
# 1. 克隆仓库
git clone https://github.com/xiaoxu-tongxue/xiaoxu-xiangqi.git
cd xiaoxu-xiangqi

# 2. 配置 SDK 路径（或直接用 Android Studio 打开工程）
echo "sdk.dir=/path/to/android-sdk" > local.properties

# 3. 使用本机 Gradle 8.9 构建
gradle assembleDebug          # 调试包
gradle assembleRelease        # 发布包（未签名，需自行配置 signingConfigs）
```

Windows 环境把 `gradle` 换成 `gradle.bat`，参数相同。

> 为避免向仓库引入 `gradle-wrapper.jar`（二进制文件），本仓库未附带 wrapper 的可执行 jar。请使用本机已安装的 Gradle 8.9；若坚持用 `./gradlew`，先在本地执行一次 `gradle wrapper --gradle-version 8.9` 生成 wrapper，或用 Android Studio 打开工程让其自动补全。

### 直接安装

前往 [Releases](../../releases) 下载 `xiaodai-xiangqi-3.0-nomusic.apk`，在 Android 7.0 及以上、arm64 架构的设备上安装即可。

## 项目结构

```
xiaoxu-xiangqi/
├── app/
│   ├── build.gradle.kts                 # 模块构建脚本（未配置签名）
│   ├── proguard-rules.pro               # 混淆规则
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/
│       │   ├── daidaixiangqi/app/
│       │   │   ├── MainActivity.kt       # 入口 Activity、界面与交互
│       │   │   ├── ChessApp.kt           # Application 初始化
│       │   │   ├── GameController.kt     # 对局状态机：走子、悔棋、提示、终局判定
│       │   │   ├── GameClock.kt          # 双方棋钟
│       │   │   ├── ai/
│       │   │   │   ├── ChessEngine.kt    # UCI 引擎进程管理与着法解析
│       │   │   │   └── ZobristHash.kt    # 局面哈希（重复局面检测）
│       │   │   ├── model/
│       │   │   │   ├── Board.kt          # 棋盘表示
│       │   │   │   ├── Move.kt           # 着法定义
│       │   │   │   ├── MoveRules.kt      # 合法着法生成与限制判定
│       │   │   │   └── MatingPattern.kt  # 杀法与局面判定
│       │   │   ├── ui/
│       │   │   │   └── BoardView.kt      # 自定义棋盘绘制与手势
│       │   │   └── audio/
│       │   │       ├── GameAudioManager.kt   # 落子 / 吃子音效
│       │   │       └── OfflineTtsSpeaker.kt  # 离线中文语音播报
│       │   └── com/k2fsa/sherpa/onnx/    # sherpa-onnx 的 Java 接口封装（纯源码）
│       └── res/
│           ├── layout/activity_main.xml
│           └── values/ (colors / strings / styles) + 纯 vector 图标
├── build.gradle.kts                     # 顶层构建脚本
├── settings.gradle.kts
├── gradle.properties
├── gradlew / gradlew.bat
├── LICENSE
└── README.md
```

## 技术架构

**棋局内核**：`Board` 描述局面状态，`MoveRules` 负责合法着法生成与各类限制判定，`ZobristHash` 为每个局面维护增量哈希，用于长将、长捉与重复局面识别；`GameController` 作为唯一状态机，统一调度走子、计时、悔棋与终局判定。

**AI 引擎**：以 UCI 协议与 Pikafish 引擎通信，通过标准输入输出交换 `position` / `go` / `bestmove` 指令；NNUE 网络文件在首次启动时复制到应用私有目录，避免每次冷启动重复解包。思考时间依局面复杂度动态采样，兼顾棋力与响应速度。

**语音播报**：通过 sherpa-onnx 的 JNI 接口加载本地语音模型，在设备端完成中文语音合成，全程无网络请求。

**棋盘渲染**：`BoardView` 为自定义 View，以 Canvas 逐帧绘制棋盘、棋子、选中态与最后一步标记，支持点击选子、拖动走子与视角翻转。

**构建体系**：Kotlin DSL（`build.gradle.kts`）+ AGP 8.7.3 + Kotlin 2.0.21；release 构建开启代码压缩与资源裁剪。

## 可选二进制资源

为保证仓库纯净，以下二进制资源未包含在源码中。缺少它们时应用仍可编译、可开局，双人对弈完全可用，仅对应增强功能不可用；如需完整功能，请自行获取并放置到指定位置：

| 功能 | 所需文件 | 放置位置 |
| --- | --- | --- |
| 人机对弈（Pikafish 引擎） | `libpikafish.so`、`pikafish.nnue` | `app/src/main/jniLibs/arm64-v8a/`、`app/src/main/assets/` |
| 离线语音播报 | `zh_CN-chaowen-medium.onnx`、`phone.fst`、`tokens.txt`、`lexicon.txt`、`*.onnx.json`、`libsherpa-onnx-jni.so` 等 | `app/src/main/assets/tts/`、`app/src/main/jniLibs/arm64-v8a/` |
| 落子 / 吃子音效 | `move_piece.wav`、`capture_piece.wav` | `app/src/main/res/raw/`（并在 `GameAudioManager.kt` 对应 TODO 处接回 SoundPool） |
| 楷体字体 | `app_kai.ttf` | `app/src/main/res/font/`（再将 `serif` 改为 `@font/app_kai`） |

## 签名

本仓库不包含任何 keystore。发布构建时请在 `app/build.gradle.kts` 中补充 `signingConfigs`，并在 `buildTypes.release` 内启用对应配置，具体位置见文件内注释。

## 版本说明

### v3.0

- **界面**：顶栏标题居中显示，翻转按钮移至左上角，避免与右侧功能按钮拥挤
- **音频**：移除全部内置背景音乐、音乐菜单与音乐播放逻辑，仅保留落子 / 吃子音效与语音播报
- **资源**：字体改用系统 `serif`，启动图标改为纯 vector 绘制，工程不再依赖 png / ttf 二进制
- **规则**：完善长将、长捉、困毙与重复局面的终局判定

## 常见问题

**Q：人机模式点了没反应？**
A：人机对弈依赖 `libpikafish.so` 与 `pikafish.nnue`。纯净源码未内置该二进制，请按[可选二进制资源](#可选二进制资源)补齐；或直接使用 Releases 中的完整安装包。

**Q：`gradle assembleRelease` 报签名错误？**
A：工程默认未配置签名。可先用 `assembleDebug` 验证构建，或按[签名](#签名)一节补上 keystore 配置。

**Q：能否在模拟器上运行？**
A：工程仅打包 `arm64-v8a`，请使用 arm64 真机或 arm64 系统镜像。

## 参与贡献

欢迎提交 Issue 与 Pull Request。提交前请确保：

1. `gradle assembleDebug` 可正常通过；
2. 不引入二进制文件（`.so`、`.apk`、图片、字体、音频等），保持仓库为纯源码；
3. 代码风格与现有模块保持一致，新增逻辑附带必要注释。

## 许可证

本项目基于 [MIT License](LICENSE) 开源。

第三方组件说明：[Pikafish](https://github.com/official-pikafish/Pikafish) 与 [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) 均为独立开源项目，其许可证以各自仓库为准；本仓库仅包含与它们对接的源码，不包含其二进制与模型文件。

## 免责声明

本项目为个人学习与技术实践作品，按现状（AS IS）提供，不构成任何形式的明示或默示担保。因使用本软件产生的任何直接或间接损失，作者不承担责任。请勿将本软件用于涉及利益的正式赛事判定场合。
