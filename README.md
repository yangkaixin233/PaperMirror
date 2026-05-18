<p align="center">
  <img src="media/icon.png" alt="PaperMirror logo" width="120">
</p>

# PaperMirror

**面向英文 LaTeX 科研论文写作的增量中文审阅预览插件。**

[VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=papermirror.papermirror-vscode) ·
[GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) ·
[English](#english)

PaperMirror 是一个 VS Code / Cursor 插件，用于把当前英文 `.tex` 文件渲染成右侧中文镜像预览。它面向科研论文写作过程：写完一段、修改一段，或让大模型润色英文后，可以快速用中文检查含义、逻辑、术语和公式上下文。

PaperMirror 的核心不是整篇翻译，而是 **只更新新增或修改过的 LaTeX block**。未变化内容会尽量复用本地缓存，因此更适合在写作期间持续打开使用。

## 核心特性

- **增量更新**：只翻译新增或修改过的 LaTeX block，未变化内容尽量复用缓存。
- **源码安全**：只读取 `.tex` 文件，不写入、不修改论文源码。
- **写作友好**：默认保存后刷新，也支持手动刷新。
- **长文档支持**：优先显示文档结构和缓存内容，未完成部分逐步更新。
- **LaTeX-aware**：尽量保护公式、引用、ref、label、url 和常见 LaTeX 命令。
- **安全回退**：单个 block 翻译失败时保留英文原文，不影响整篇预览。
- **接口灵活**：支持 OpenAI、DeepSeek、Ollama、SiliconFlow，以及自定义 OpenAI-compatible Chat Completions endpoint。

## 安装

### VS Code Marketplace

在 VS Code 或 Cursor 扩展面板中搜索：

```text
PaperMirror
```

### VSIX

也可以从 [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) 下载 `papermirror-vscode-0.1.2.vsix`，然后通过以下命令安装：

```text
Extensions: Install from VSIX...
```

Cursor 也支持相同的 VSIX 安装方式。

## 快速开始

1. 打开一篇英文 `.tex` 文件。
2. 运行 `PaperMirror: Set API Key`。
3. 运行 `PaperMirror: Validate API Connection`。
4. 运行 `PaperMirror: Open Preview`。
5. 修改并保存 `.tex` 文件。

保存后，PaperMirror 会刷新右侧中文预览。命中缓存时，只会请求改动过的 block。

## Provider 配置

PaperMirror 使用 OpenAI-compatible Chat Completions 接口。

SiliconFlow 示例：

```json
{
  "paperMirror.provider": "siliconflow",
  "paperMirror.baseUrl": "https://api.siliconflow.cn/v1",
  "paperMirror.model": "Pro/zai-org/GLM-4.7",
  "paperMirror.updateMode": "onSave",
  "paperMirror.translateComments": false,
  "paperMirror.showReferenceMarkersInTranslations": false
}
```

`paperMirror.baseUrl` 只填写 base URL，不要包含 `/chat/completions`，PaperMirror 会自动拼接。

当前支持的 provider 预设：

- OpenAI
- DeepSeek
- Ollama
- SiliconFlow
- Custom OpenAI-compatible endpoint

当前版本主要使用 **SiliconFlow** 测试。其他 provider 如有兼容性问题，欢迎反馈。

## 重要设置

| 设置项 | 说明 |
| --- | --- |
| `paperMirror.provider` | Provider 预设：`openai`、`deepseek`、`ollama`、`siliconflow` 或 `custom`。 |
| `paperMirror.baseUrl` | OpenAI-compatible base URL。 |
| `paperMirror.model` | 模型名。 |
| `paperMirror.updateMode` | `onSave` 或 `manual`。 |
| `paperMirror.translateComments` | 是否翻译 LaTeX 注释，默认 `false`。 |
| `paperMirror.showReferenceMarkersInTranslations` | 是否在译文中显示 `[cite]`、`[ref]`、`[url]`，默认 `false`。 |
| `paperMirror.previewFontFamily` / `paperMirror.previewFontSize` | 中文预览字体和字号。 |
| `paperMirror.maxBlocksPerRefresh` | 每次刷新最多翻译多少个 block，`0` 表示不限制。 |

API key 推荐通过 `PaperMirror: Set API Key` 存入 VS Code SecretStorage。也可以写入 settings，但安全性更低。

## 当前边界

- v0.1.2 只支持当前活动 `.tex` 单文件。
- 暂不递归解析完整 `\input` / `\include` LaTeX 项目。
- PaperMirror 不是 TeX 编译器，不保证最终 PDF 排版一致。
- 复杂表格、TikZ、`algorithm2e` 和自定义宏目前主要以占位显示。
- 公式渲染依赖 MathJax，对复杂 TeX 宏包语法仅做 best-effort 支持。
- 中文预览是写作辅助视图，不应作为正式译文或投稿材料。

## 隐私

PaperMirror 不修改 `.tex` 源文件。需要翻译的文本 block 会发送给你配置的 provider。使用 `PaperMirror: Set API Key` 时，API key 默认存储在 VS Code SecretStorage 中。

## English

PaperMirror is an **incremental Chinese review preview for English LaTeX research writing**. It is a VS Code / Cursor extension that renders the active `.tex` file into a Simplified Chinese mirror preview. It is designed for writing-time review: after drafting, editing, or LLM-assisted polishing, you can quickly check meaning, logic, terminology, and formula context in Chinese.

PaperMirror is not a whole-document translator. Its core behavior is **changed-block translation**: it re-translates only newly added or modified LaTeX blocks and reuses local cache for unchanged content whenever possible.

### Features

- **Incremental updates**: translate only newly added or modified LaTeX blocks, with local cache reuse for unchanged content.
- **Source-safe**: reads `.tex` files but does not write to or modify your paper source.
- **Writing-friendly**: refreshes on save by default, with manual refresh also available.
- **Long-document support**: shows document structure and cached content first, then updates unfinished parts progressively.
- **LaTeX-aware**: tries to protect math, citations, refs, labels, URLs, and common LaTeX commands.
- **Safe fallback**: keeps the original English text when an individual block fails, instead of breaking the whole preview.
- **Flexible providers**: supports OpenAI, DeepSeek, Ollama, SiliconFlow, and custom OpenAI-compatible Chat Completions endpoints.

### Installation

Search **PaperMirror** in the VS Code or Cursor extension panel.

You can also download `papermirror-vscode-0.1.2.vsix` from [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases), then install it with:

```text
Extensions: Install from VSIX...
```

### Quick Start

1. Open an English `.tex` file.
2. Run `PaperMirror: Set API Key`.
3. Run `PaperMirror: Validate API Connection`.
4. Run `PaperMirror: Open Preview`.
5. Edit and save the `.tex` file.

After saving, PaperMirror refreshes the Chinese preview. When cache is available, only changed blocks are sent to the provider.

### Provider Configuration

PaperMirror uses OpenAI-compatible Chat Completions APIs.

SiliconFlow example:

```json
{
  "paperMirror.provider": "siliconflow",
  "paperMirror.baseUrl": "https://api.siliconflow.cn/v1",
  "paperMirror.model": "Pro/zai-org/GLM-4.7",
  "paperMirror.updateMode": "onSave",
  "paperMirror.translateComments": false,
  "paperMirror.showReferenceMarkersInTranslations": false
}
```

`paperMirror.baseUrl` should be the base URL only. Do not include `/chat/completions`; PaperMirror appends it automatically.

Supported provider presets:

- OpenAI
- DeepSeek
- Ollama
- SiliconFlow
- Custom OpenAI-compatible endpoint

This version has mainly been tested with **SiliconFlow**. If you find compatibility issues with other providers, feedback is welcome.

### Key Settings

| Setting | Description |
| --- | --- |
| `paperMirror.provider` | Provider preset: `openai`, `deepseek`, `ollama`, `siliconflow`, or `custom`. |
| `paperMirror.baseUrl` | OpenAI-compatible base URL. |
| `paperMirror.model` | Model name. |
| `paperMirror.updateMode` | `onSave` or `manual`. |
| `paperMirror.translateComments` | Whether to translate LaTeX comments. Default: `false`. |
| `paperMirror.showReferenceMarkersInTranslations` | Whether to show `[cite]`, `[ref]`, and `[url]` in translations. Default: `false`. |
| `paperMirror.previewFontFamily` / `paperMirror.previewFontSize` | Font family and font size for the Chinese preview. |
| `paperMirror.maxBlocksPerRefresh` | Maximum blocks to translate per refresh. `0` means unlimited. |

API keys should preferably be stored through `PaperMirror: Set API Key`, which uses VS Code SecretStorage. You can also put keys in settings, but that is less secure.

### Current Scope

- v0.1.2 supports the active `.tex` file only.
- Full recursive `\input` / `\include` project parsing is not supported yet.
- PaperMirror is not a TeX compiler and does not guarantee final PDF layout fidelity.
- Complex tables, TikZ, `algorithm2e`, and custom macros are mostly shown as placeholders.
- Math rendering uses MathJax and supports complex TeX package syntax on a best-effort basis.
- The Chinese preview is a writing aid, not a formal translation or submission artifact.

### Privacy

PaperMirror does not modify your `.tex` source file. Text blocks that need translation are sent to the provider you configure. When using `PaperMirror: Set API Key`, the API key is stored in VS Code SecretStorage by default.

## License

MIT.
