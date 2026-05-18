<p align="center">
  <img src="media/icon.png" alt="PaperMirror logo" width="128">
</p>

# PaperMirror: Incremental Chinese Preview for LaTeX Paper Writing

> Write English LaTeX. Review a Chinese mirror beside it. Only changed blocks are retranslated.

PaperMirror is a VS Code / Cursor extension for researchers writing English LaTeX papers. It turns your active `.tex` file into a Chinese mirror preview for drafting and review. When you edit a paragraph, or ask an LLM to polish your English, PaperMirror updates only the changed LaTeX blocks, so you can quickly check meaning, logic, terminology, and formula context without recompiling a PDF or retranslating the whole paper.

**Install**: search **PaperMirror** in the VS Code Marketplace, or download the VSIX from [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases).

**Language / 语言**: [中文](#中文) | [English](#english)

<a id="中文"></a>

## 中文

PaperMirror 是一个面向英文 LaTeX 科研论文写作的 **增量中文审阅预览工具**。你继续在 VS Code 或 Cursor 里写英文 `.tex`，右侧显示中文镜像侧栏。每次保存后，它会优先更新新增或修改过的段落，而不是整篇重翻译，所以更适合写作期间一直开着用。

它不是通用翻译器，也不是 PDF 翻译器。英文 `.tex` 始终是唯一真实稿件，中文 preview 只是用来辅助审阅：看意思有没有跑偏、逻辑是否连贯、术语是否一致、公式上下文是否能读懂。

### 适合什么场景

- 写英文论文时，想在旁边快速读中文镜像。
- 让大模型润色英文段落后，想马上检查中文含义是否符合原意。
- 不想每改一句话都重新编译 PDF 或整篇重翻译。
- 论文里有公式、引用、ref、label、url，希望这些结构尽量不要被翻乱。
- 长文档写作时，希望只更新改过的部分，并复用缓存。

### 核心特性

- **VS Code / Cursor 可用**：从插件商店搜索 `PaperMirror` 安装，也可以下载 VSIX。
- **源码安全**：只读取 `.tex`，不写入、不修改你的论文源码。
- **增量翻译**：按 LaTeX block 翻译，未变化内容尽量复用本地缓存。
- **保存刷新**：默认保存 `.tex` 后更新右侧 preview，也支持手动 `Refresh`。
- **长文档渐进式显示**：先显示结构和已缓存内容，未完成段落显示 loading，后续分批补上。
- **LaTeX-aware**：尽量保护公式、`cite`、`ref`、`label`、`url` 和常见 LaTeX 命令。
- **本地 MathJax**：常见公式会尝试渲染；复杂宏包和自定义宏属于 best-effort。
- **安全回退**：单个 block 失败时保留英文原文，不让整篇 preview 崩掉。

### 快速开始

1. 在 VS Code / Cursor 扩展面板搜索 **PaperMirror** 并安装。
2. 打开一篇英文 `.tex` 文件。
3. 运行 `PaperMirror: Set API Key`。
4. 运行 `PaperMirror: Validate API Connection`。
5. 运行 `PaperMirror: Open Preview`。
6. 修改并保存 `.tex`，右侧中文 preview 会更新。

SiliconFlow 示例设置：

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

`paperMirror.baseUrl` 只填 base URL，不要填完整的 `/chat/completions`，PaperMirror 会自动拼接。

### API 说明

PaperMirror 使用 OpenAI-compatible Chat Completions 接口。作者目前主要用 **SiliconFlow** 测试；DeepSeek、OpenAI、Ollama 和自定义 OpenAI-compatible endpoint 也可以配置，但其他平台还需要更多真实反馈。

API key 推荐通过 `PaperMirror: Set API Key` 存入 VS Code SecretStorage。也可以写入 settings，但安全性更低。

### 重要设置

- `paperMirror.provider`: `deepseek`、`siliconflow`、`openai`、`ollama` 或 `custom`。
- `paperMirror.baseUrl`: OpenAI-compatible base URL。
- `paperMirror.model`: 模型名。
- `paperMirror.updateMode`: `onSave` 或 `manual`。
- `paperMirror.translateComments`: 是否翻译 LaTeX 注释，默认 `false`。
- `paperMirror.showReferenceMarkersInTranslations`: 是否在中文译文中显示 `[cite]`、`[ref]`、`[url]` 标记，默认 `false`。
- `paperMirror.previewFontFamily` / `paperMirror.previewFontSize`: 中文预览字体和字号。
- `paperMirror.maxBlocksPerRefresh`: 每次 refresh 最多翻译多少个 block，`0` 表示不限制。

### 当前边界

- v0.1.2 只支持当前活动 `.tex` 单文件，不递归解析完整 `\input` / `\include` 项目。
- PaperMirror 不是 TeX 编译器，不保证最终 PDF 排版一致。
- `algorithm2e`、复杂表格、TikZ 和自定义宏目前主要按占位处理。
- 公式预览依赖 MathJax，常见语法可用，复杂宏包不保证。
- 中文 preview 是写作辅助视图，不应作为正式译文或投稿材料。

<a id="english"></a>

## English

PaperMirror is an **incremental Chinese review preview for English LaTeX paper writing**. It is designed to stay open while you draft: keep editing your `.tex` file, read a Chinese mirror beside it, and retranslate only the parts you changed.

It is not a TeX compiler or a PDF translator. The English `.tex` file remains the single source of truth; the Chinese preview is a temporary reading aid for checking meaning, argument flow, terminology, and formula context.

### When It Helps

- You write English LaTeX papers but think through drafts in Chinese.
- You ask an LLM to polish a paragraph and want to review the meaning immediately.
- You do not want to recompile a PDF or retranslate the whole paper after small edits.
- Your paper contains formulas, citations, refs, labels, and URLs that should not be casually rewritten.
- You work with long `.tex` files and want changed-block updates with cache reuse.

### Features

- Works in VS Code and Cursor.
- Source-safe: PaperMirror reads your `.tex` file but never modifies it.
- Changed-block translation with local cache reuse.
- Save-triggered refresh by default, with manual Refresh available.
- Progressive preview for long documents.
- LaTeX-aware protection for math, citations, refs, labels, URLs, and common commands.
- Local MathJax resources for common equation rendering.
- Safe fallback: failed blocks keep the original English instead of breaking the preview.

### Quick Start

1. Search **PaperMirror** in the VS Code Marketplace and install it.
2. Open an English `.tex` file.
3. Run `PaperMirror: Set API Key`.
4. Run `PaperMirror: Validate API Connection`.
5. Run `PaperMirror: Open Preview`.
6. Edit and save the `.tex` file to refresh the Chinese preview.

You can also download `papermirror-vscode-0.1.2.vsix` from [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) and install it through `Extensions: Install from VSIX...`.

### Providers

PaperMirror uses an OpenAI-compatible Chat Completions interface. It has mainly been tested with **SiliconFlow**. DeepSeek, OpenAI, Ollama, and custom OpenAI-compatible endpoints are configurable, but provider-specific feedback is welcome.

### Limitations

- v0.1.2 supports the active `.tex` file only.
- It does not recursively resolve a full `\input` / `\include` LaTeX project.
- It is not a TeX compiler and does not guarantee final PDF layout.
- Complex tables, TikZ, `algorithm2e`, and custom macros are mostly represented with placeholders.
- Formula rendering is best-effort for complex TeX package syntax.

### Privacy

PaperMirror does not modify your `.tex` source. Text blocks that need translation are sent to your selected provider. API keys are stored in VS Code SecretStorage by default when using `PaperMirror: Set API Key`.

## License

MIT.
