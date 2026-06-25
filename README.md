<p align="center">
  <img src="assets/icon.png" alt="Winko 图标" width="128" height="128">
</p>

<h1 align="center">Winko</h1>

<p align="center">
  <strong>把 AI 编程助手的沉默，翻译成一盏会呼吸的信号灯。</strong>
</p>

<p align="center">
  <a href="README.en.md">English</a>
  ·
  <a href="https://github.com/zhc93design/winko/releases">下载最新版</a>
  ·
  <a href="#快速开始">快速开始</a>
  ·
  <a href="#灯语">灯语</a>
  ·
  <a href="#开发">开发</a>
</p>

<p align="center">
  <a href="https://github.com/zhc93design/winko/releases">
    <img alt="最新版本" src="https://img.shields.io/github/v/release/zhc93design/winko?label=%E7%89%88%E6%9C%AC&labelColor=111827&color=22c55e&style=flat-square">
  </a>
  <img alt="MIT 许可证" src="https://img.shields.io/badge/%E8%AE%B8%E5%8F%AF%E8%AF%81-MIT-facc15?labelColor=111827&style=flat-square">
  <img alt="macOS 桌面端" src="https://img.shields.io/badge/macOS-%E6%A1%8C%E9%9D%A2%E7%AB%AF-ef4444?labelColor=111827&style=flat-square">
</p>

![Winko 概览](assets/readme-hero.svg)

## 它是什么

Winko 是一个 macOS 桌面悬浮信号灯，专门给同时使用多个 AI 编程助手的人看状态。它监听 CatDesk、Claude Code、Codex、Cursor、OpenClaw、Z Code 和 OpenCode，把分散在终端、编辑器、本地钩子、日志和本机状态流里的事件，聚合成一盏一眼能读懂的灯。

你不用反复切回每个编程助手，看它是不是还在跑、是不是等你确认计划、是不是卡在权限或错误上。Winko 会安静地待在屏幕角落：绿色表示可以放心，黄色表示值得看一眼，红色表示需要处理。

它不是新的工作台，也不是需要盯着看的仪表盘。它更像一个环境提示物，让余光知道你的编程助手队列正在推进、等待，还是已经需要你接手。

## 为什么需要它

AI 编程助手的工作经常发生在后台：模型在思考、工具在执行、本地钩子在等待，编辑器里还有另一个助手正在继续。单个工具还能切回去看，多助手并行时，人很快就会失去“现在到底谁需要我”的感觉。

Winko 做的事很小，但很关键：

- 把多个助手来源压缩成一个统一灯态。
- 区分“正在处理”“需要关注”“权限/错误”等不同紧急程度。
- 完成后短暂保留最近会话，点击灯就能回到更可能需要继续布置任务的来源。
- 支持屏幕悬浮灯，也支持通过 CH552 桥接到实体桌面信号灯。

## 核心能力

| 能力 | 说明 |
| --- | --- |
| 多来源监控 | 覆盖 CatDesk、Claude Code、Codex、Cursor、OpenClaw、Z Code 和 OpenCode。 |
| 统一灯语 | 原始事件先转成阶段，再转成信号，最后按当前灯具样式渲染。 |
| 多会话聚合 | 多个会话同时运行时，自动展示优先级最高的状态。 |
| Claude Code 权限弹窗 | Claude Code 的提问、计划确认和权限请求可以由 Winko 弹窗承接。 |
| 可调显示形态 | 支持三灯、单灯变色、单灯单色、红绿双灯和 CH552 硬件桥。 |
| 本地隐私边界 | 只用本地钩子、日志、文件或状态流；只读集成避免读取提示词和模型正文。 |

## 灯语

Winko 的状态模型是分层的：

```text
软件事件 -> 工作阶段 -> 信号状态 -> 具体灯效
```

这意味着你可以调整“灯怎么亮”，而不破坏“事件怎么判断”。

| 信号 | 含义 | 默认感觉 |
| --- | --- | --- |
| `idle` | 空闲、完成、可以放心 | 绿灯常亮 |
| `working` | 正在思考、执行工具或编辑 | 默认安静/熄灭 |
| `attention` | 可能需要补充输入或看一眼 | 黄灯或红灯闪烁，取决于灯具样式 |
| `error` | 权限、计划确认、失败或阻塞 | 强提醒 |
| `silent` | 手动关闭或静默 | 熄灭 |

## 支持的数据源

| 来源 | 监听方式 | 边界 |
| --- | --- | --- |
| CatDesk | `agent.log` 文件监听 + 会话轮询 | 读取状态事件和会话列表 |
| Claude Code | 本地钩子服务 | 支持状态监控与 Claude Code 权限弹窗 |
| Codex | 本地钩子服务 | 监听钩子事件，计划回合会提示继续输入 |
| Cursor | 本地钩子服务 | 只做状态观察，不接管原生审批 |
| OpenClaw | 只读会话文件适配器 | 不安装钩子，不接管审批 |
| Z Code | 只读 JSONL 日志监听 | 不读取正文，不猜测不可观测的权限状态 |
| OpenCode | 服务端 SSE，必要时降级到结构化日志 | 只使用结构化状态字段，不读取消息正文 |

## 显示形态

同一套信号可以映射到不同灯具形态：

- 三灯泡红黄绿信号灯
- 单灯泡变色
- 单灯单色，适合极简硬件
- 红绿双灯，支持横排和竖排
- CH552 USB 硬件信号灯桥

## 快速开始

从 [zhc93design/winko 发行页](https://github.com/zhc93design/winko/releases) 下载最新版 macOS 安装包，打开 Winko 后按引导安装本地钩子。

Codex 首次发现新钩子时，可能需要在 Codex 中运行一次 `/hooks` 并信任它。

运行时配置文件位于：

```text
~/.catdesk-signal-light/config.json
```

## 隐私边界

Winko 是本地状态翻译器。它监听的是本地钩子、本地日志、本地会话文件或本机状态流，不把这些状态发送到外部服务。

对于 OpenClaw、Z Code、OpenCode 日志降级等只读集成，Winko 只使用结构化状态元数据判断灯态，避免读取提示词、模型正文或完整消息内容。

## 开发

```bash
npm install
npm test
npm start
```

本项目是 Electron 桌面应用。开发模式会跳过在线更新检查。

## 构建

```bash
npm run pack
npm run dist
```

`npm run pack` 会在 `dist/mac-universal/` 生成未打包的通用 macOS 应用。

`npm run dist` 会构建签名并公证过的发行 DMG，同时运行构建后校验。运行前需要设置 [`docs/macos-signing.md`](docs/macos-signing.md) 中说明的本机 Apple 公证环境变量，API key 文件不能放进仓库。

预期发行产物：

```text
dist/Winko-X.Y.Z-universal.dmg
dist/Winko-X.Y.Z-universal.dmg.blockmap
dist/latest-mac.yml
```

## 仓库结构

| 路径 | 说明 |
| --- | --- |
| `src/` | Electron 主进程、渲染层、钩子服务、信号聚合、更新与硬件桥。 |
| `hardware/` | CH552 硬件草图、接线和固件说明。 |
| `tests/` | 桌面应用的 Vitest 单元测试。 |
| `scripts/` | 钩子脚本、发布检查、构建校验和开发辅助脚本。 |
| `docs/` | 设计说明、接入研究和生成的监控总表。 |
| `assets/` | 图标与 README 视觉素材。 |

## 发布维护

发布前需要确保这些版本一致：

```text
package.json version = X.Y.Z
src/renderer/settings.html about-version-badge = vX.Y.Z
src/renderer/settings.html changelog top entry = vX.Y.Z
dist/latest-mac.yml version = X.Y.Z
```

检查源码侧一致性：

```bash
npm run release:check -- X.Y.Z
```

构建并检查产物侧一致性：

```bash
npm run dist
npm run release:check -- X.Y.Z --after-dist
```

发布辅助脚本可以运行测试、构建、提交、推送、创建公开 GitHub 发行版，并上传三个必要产物：

```bash
npm run release:github -- X.Y.Z
```

公开发行包会检查 `zhc93design/winko` 的更新；开发模式仍会跳过在线更新检查。

## 维护说明

修改项目规则或文档职责前，先读 `SOURCE_OF_TRUTH.md`。修改 `src/`、`scripts/`、配置结构、事件映射、发布流程或测试前，先读 `AGENTS.md`。

`docs/AI_CONTEXT.generated.md` 是给 AI 快速上岗的生成索引；当来源注册表、IPC、脚本、测试、默认配置、渲染层文件或护栏范围变化时，用 `npm run ai-context:update` 重新生成，不要手改。
