<p align="center">
  <img src="media/icon.png" alt="PaperMirror logo" width="120">
</p>

# PaperMirror

**Incremental Chinese review preview for English LaTeX paper writing.**

[VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=papermirror.papermirror-vscode) ·
[GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) ·
[中文简介](#中文简介)

PaperMirror is a VS Code / Cursor extension for researchers writing English LaTeX papers. It renders the active `.tex` file as a Simplified Chinese review preview and re-translates only newly added or modified LaTeX blocks after edits.

It is designed for writing-time review: checking meaning, logic, terminology, and formula context after drafting or LLM-assisted polishing, without recompiling a PDF or re-translating the whole paper.

## Highlights

- **Incremental by design**: changed LaTeX blocks are translated; unchanged blocks reuse local cache when possible.
- **Source-safe**: PaperMirror reads `.tex` files but does not modify them.
- **Writing workflow friendly**: save-triggered refresh by default, with manual refresh available.
- **Long-document aware**: cached content and document structure appear first; pending blocks update progressively.
- **LaTeX-aware preview**: math, citations, refs, labels, URLs, and common commands are protected as much as possible.
- **Safe fallback**: failed blocks keep the original English instead of breaking the preview.
- **Provider flexible**: uses OpenAI-compatible Chat Completions providers.

## Installation

### VS Code Marketplace

Search for **PaperMirror** in the VS Code extension marketplace.

### VSIX

Download `papermirror-vscode-0.1.2.vsix` from [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases), then install it through:

```text
Extensions: Install from VSIX...
```

Cursor supports the same VSIX installation flow.

## Quick Start

1. Open an English `.tex` file.
2. Run `PaperMirror: Set API Key`.
3. Run `PaperMirror: Validate API Connection`.
4. Run `PaperMirror: Open Preview`.
5. Edit and save the `.tex` file.

PaperMirror will update the Chinese preview on save. Only changed blocks are sent for translation when cache is available.

## Provider Configuration

PaperMirror uses an OpenAI-compatible Chat Completions interface.

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

Use the provider base URL only. Do not include `/chat/completions`; PaperMirror appends it automatically.

Supported provider presets:

- SiliconFlow
- DeepSeek
- OpenAI
- Ollama
- Custom OpenAI-compatible endpoint

The current release has mainly been tested with **SiliconFlow**. Feedback for other providers is welcome.

## Key Settings

| Setting | Purpose |
| --- | --- |
| `paperMirror.provider` | Provider preset: `deepseek`, `siliconflow`, `openai`, `ollama`, or `custom`. |
| `paperMirror.baseUrl` | OpenAI-compatible base URL. |
| `paperMirror.model` | Model name. |
| `paperMirror.updateMode` | `onSave` or `manual`. |
| `paperMirror.translateComments` | Translate LaTeX comments. Default: `false`. |
| `paperMirror.showReferenceMarkersInTranslations` | Show `[cite]`, `[ref]`, `[url]` markers in translated text. Default: `false`. |
| `paperMirror.previewFontFamily` / `paperMirror.previewFontSize` | Chinese preview font and size. |
| `paperMirror.maxBlocksPerRefresh` | Maximum translated blocks per refresh. `0` means unlimited. |

API keys should be stored with `PaperMirror: Set API Key`, which uses VS Code SecretStorage. A settings-based key is available for testing but is less secure.

## Limitations

- v0.1.2 supports the active `.tex` file only.
- It does not recursively resolve full `\input` / `\include` LaTeX projects.
- It is not a TeX compiler and does not guarantee final PDF layout.
- Complex tables, TikZ, `algorithm2e`, and custom macros are mostly represented with placeholders.
- Formula rendering uses MathJax and is best-effort for complex TeX package syntax.
- The Chinese preview is a writing aid, not a formal translation for submission.

## Privacy

PaperMirror does not modify your `.tex` source. Text blocks that need translation are sent to the provider you configure. API keys are stored in VS Code SecretStorage by default.

## 中文简介

PaperMirror 是一个面向英文 LaTeX 科研论文写作的 **增量中文审阅预览插件**。你继续在 VS Code 或 Cursor 中编辑英文 `.tex`，PaperMirror 在右侧生成中文镜像预览，并优先更新新增或修改过的 LaTeX block。

它适合写作期间持续打开使用：写完一段、让大模型润色一段、或修改一小段后，可以直接在右侧检查中文含义、逻辑衔接、术语一致性和公式上下文。

核心原则：

- 英文 `.tex` 是唯一真实稿件。
- PaperMirror 不修改论文源码。
- 只翻译必要的新增或修改内容。
- 公式、引用和常见 LaTeX 命令尽量保持安全。
- 单个 block 失败时回退显示英文原文。

## License

MIT.
