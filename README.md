# IDEswitcher

<p align="center">
  <img src="https://img.shields.io/badge/version-1.2.0-blue" alt="Version">
  <img src="https://img.shields.io/badge/platform-macOS-000000" alt="Platform">
  <img src="https://img.shields.io/badge/language-Kotlin%20%2B%20TypeScript-0095D5" alt="Language">
</p>

## 项目简介

agentic coding 时代，开发者双开IntelliJ IDEA和agentic coding平台已成为日常。

**IDEswitcher** 提供 IDEA 与 Qoder / CodeFuse 之间的**双向快速跳转**，精确保留文件路径和光标位置（行 + 列）。

### 功能特性

- **双向跳转**：IDEA ↔ Qoder / IDEA ↔ CodeFuse，统一快捷键 `⌥⇧O`
- **精确定位**：跳转后光标停在同一文件的同一行、同一列
- **可配置目标**：Settings → Tools → IDEswitcher 中选择跳转到 Qoder 还是 CodeFuse
- **智能默认值**：首次启动时自动检测已安装的编辑器作为默认目标


---

## 快速开始

### 1. 安装 IntelliJ IDEA 插件

**方式一：直接安装**

下载 [Releases](../../releases) 中的 `IDEswitcher-1.2.0.zip`，在 IDEA 中：Settings → Plugins → ⚙️ → Install Plugin from Disk...

**方式二：从源码构建**

```bash
cd IDEswitcher-main
./gradlew build
# 产物在 build/distributions/IDEswitcher-1.2.0.zip
```

安装后在 Settings → Tools → IDEswitcher 中选择跳转目标（Qoder 或 CodeFuse）。

### 2. 安装编辑器扩展（Qoder / CodeFuse）

```bash
cd IDEswitcher-main/agentic-ide-extension
npm install && npm run compile
```

将整个 `agentic-ide-extension/` 目录拷贝到对应位置：

| 编辑器 | 安装路径 | 目录命名 |
|--------|---------|---------|
| Qoder | `~/.qoder/extensions/` | `ide-switcher` |
| CodeFuse | `~/.codefuse/extensions/` | `ali-team.ide-switcher-1.2.0` |

> **注意**：CodeFuse 要求扩展目录名遵循 `publisher.name-version` 格式，否则不会识别。

### 3. 使用

在任一 IDE 中按 `⌥⇧O`（Option+Shift+O），即可跳转到另一个 IDE 的相同文件和光标位置。

也可以通过右键菜单 → **Jump to Editor** / **Jump to IntelliJ IDEA** 触发。

---

## 工作原理

```
IDEA → Qoder/CodeFuse:  ⌥⇧O → 读取 Settings.target → code --goto file:line:col
Qoder/CodeFuse → IDEA:  ⌥⇧O → 检测 IDEA 版本 → idea --line L --column C file
```

### 项目结构

```
IDEswitcher-main/
├── src/main/kotlin/com/ali/ideswitcher/
│   ├── action/JumpAction.kt              # IDEA 端主 Action
│   ├── target/
│   │   ├── Target.kt                     # 枚举：QODER / CODEFUSE
│   │   ├── Jumper.kt                     # 跳转接口（含默认实现）
│   │   ├── QoderJumper.kt               # Qoder 跳转常量
│   │   └── CodeFuseJumper.kt            # CodeFuse 跳转常量
│   └── settings/
│       ├── IdeSwitcherSettings.kt        # 持久化配置
│       └── IdeSwitcherConfigurable.kt    # Settings 页 UI
├── src/main/resources/META-INF/
│   └── plugin.xml                        # IntelliJ 插件描述
├── src/test/                             # 单元测试
├── agentic-ide-extension/                # VS Code 扩展（通用）
│   ├── src/extension.ts                  # 核心逻辑
│   └── out/extension.js                  # 编译产物
└── build.gradle.kts                      # Gradle 构建配置
```

---

## 开发

```bash
cd IDEswitcher-main

# 启动沙箱 IDE（插件自动加载）
./gradlew runIde

# 运行测试
./gradlew test

# 清理
./gradlew clean
```

### 技术栈

- **IDEA 插件**：Kotlin 1.9 + IntelliJ Platform SDK 2024.1
- **编辑器扩展**：TypeScript + VS Code Extension API
- **构建**：Gradle (IDEA) / npm (扩展)
- **无运行时外部依赖**

---

## 已知限制

- 仅支持 macOS
- IntelliJ IDEA 路径检测仅覆盖 `/Applications` 下的标准安装，JetBrains Toolbox 自定义路径未自动检测
- Qoder / CodeFuse 路径硬编码为 `/Applications/Qoder.app` 和 `/Applications/CodeFuse.app`

---

## License

MIT
