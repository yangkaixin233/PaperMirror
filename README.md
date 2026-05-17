<p align="center">
  <img src="media/icon.png" alt="PaperMirror logo" width="128">
</p>

# PaperMirror: VS Code / Cursor LaTeX Chinese Mirror Preview

> Write English LaTeX. Read a Chinese mirror beside it.

PaperMirror is a lightweight Chinese mirror preview for researchers writing English LaTeX papers in VS Code or Cursor. You keep editing the English `.tex` source; PaperMirror shows a Simplified Chinese reading view beside it. The English source remains the single source of truth.

**Language / 语言**: [中文](#中文) | [English](#english)

**Keywords**: LaTeX Chinese preview, TeX Chinese mirror, VS Code LaTeX translator, Cursor LaTeX translator, AI paper writing assistant, academic writing, technical paper writing, OpenAI-compatible LaTeX translation, DeepSeek LaTeX translation, SiliconFlow LaTeX translation, MathJax LaTeX preview.

<a id="中文"></a>

## 中文

[English](#english)

### PaperMirror 是什么

PaperMirror 是一个面向科研写作者的 **LaTeX 中文镜像预览工具**。你继续写英文 `.tex` 源码，右侧同步阅读中文 preview，用来快速检查论文逻辑、术语一致性、段落表达和公式上下文。

一句话概括：

> 写英文 TeX，右侧读中文镜像。公式和引用尽量不乱，只更新改过的段落。

PaperMirror 不是通用翻译器，也不是 PDF 翻译器。它更像一个安静的论文阅读侧栏。英文 `.tex` 始终是唯一真实稿件，中文 preview 只是写作时的辅助视图。

### 适合什么场景

PaperMirror 适合正在用 VS Code 或 Cursor 写英文 LaTeX 论文的研究者，尤其适合：

- 一边写英文 `.tex`，一边读中文镜像。
- 不想每改一句话都重新编译 PDF。
- 不想把整篇论文复制到外部翻译工具。
- 希望公式、引用、标签、URL 和 LaTeX 命令尽量保持安全。
- 希望长文档只翻译新增或修改过的段落。
- 希望继续使用 LaTeX Workshop、Git、Copilot、GitLens 等原有编辑器工作流。
- 希望接入自己的 DeepSeek、SiliconFlow、OpenAI、Ollama 或自定义 OpenAI-compatible endpoint。

### 核心特性

- **右侧中文镜像预览**：为当前活动 `.tex` 文件生成中文 reading preview。
- **源码安全**：PaperMirror 不写入、不修改你的 `.tex` 源文件。
- **保存刷新**：默认保存 `.tex` 后刷新，也支持手动 `Refresh`。
- **只翻译变动内容**：新增或修改过的 block 会被翻译，未变化 block 尽量复用本地缓存。
- **长文档渐进式预览**：先显示文档结构和已缓存内容，未完成段落显示 loading，后续按 batch 更新右侧 preview。
- **公式和引用保护**：尽量保护 inline math、display math、`cite`、`ref`、`label`、`url` 等内容。
- **MathJax 本地资源**：常见公式通过本地 MathJax 资源渲染；复杂宏包和自定义宏属于 best-effort。
- **论文结构解析**：支持 heading、paragraph、item、caption、abstract、keywords、theorem-like environments 等常见论文结构。
- **列表与伪代码**：`itemize`、`enumerate`、`highlights` 渲染为列表；常见 `algorithm` + `algorithmic` 伪代码会尽量渲染为可读列表。
- **复杂环境占位**：`figure`、`table`、`algorithm2e`、`tikzpicture` 等复杂环境显示为轻量占位，而不是错误框。
- **注释默认跳过**：默认不翻译 `%` 注释掉的 LaTeX 内容；需要时可打开 `paperMirror.translateComments`。
- **安全失败回退**：如果模型返回非法 JSON、遗漏 block id 或丢失 placeholder，PaperMirror 会保留英文原文并显示 warning。
- **Provider 可替换**：通过 OpenAI-compatible Chat Completions 接口支持 SiliconFlow、DeepSeek、OpenAI、Ollama 和 Custom endpoint。

### 安装

#### 方式 1：VS Code Marketplace

在 VS Code 或 Cursor 扩展面板中搜索：

```text
PaperMirror
```

如果刚发布后还搜不到，请等待 Marketplace 同步，或先使用 GitHub Release 里的 `.vsix`。

#### 方式 2：GitHub Release / VSIX

1. 打开 [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases)。
2. 下载 `papermirror-vscode-0.1.2.vsix`。
3. 在 VS Code 或 Cursor 中打开 Extensions 面板。
4. 选择 `Install from VSIX...`。
5. 打开一个 `.tex` 文件，运行 `PaperMirror: Open Preview`。

### 快速使用

1. 打开一篇英文 `.tex` 文件。
2. 运行 `PaperMirror: Set API Key`。
3. 运行 `PaperMirror: Validate API Connection`。
4. 运行 `PaperMirror: Open Preview`。
5. 修改并保存 `.tex` 文件，右侧 preview 会更新。

SiliconFlow 示例设置：

```json
{
  "paperMirror.provider": "siliconflow",
  "paperMirror.baseUrl": "https://api.siliconflow.cn/v1",
  "paperMirror.model": "Pro/zai-org/GLM-4.7",
  "paperMirror.updateMode": "onSave",
  "paperMirror.translateComments": false,
  "paperMirror.showReferenceMarkersInTranslations": false,
  "paperMirror.previewFontFamily": "Microsoft YaHei",
  "paperMirror.previewFontSize": 15
}
```

`paperMirror.baseUrl` 只填 base URL，不要填完整的 `/chat/completions`，PaperMirror 会自动拼接。

### API 兼容性说明

PaperMirror 通过 OpenAI-compatible Chat Completions 接口调用模型。作者当前主要使用并测试的是 **SiliconFlow**，因为作者日常可用的是硅基流动 API。

DeepSeek、OpenAI、Ollama 和自定义 OpenAI-compatible endpoint 已在配置层支持，但不同平台的响应格式、JSON 稳定性、限流策略和模型名称可能存在差异。如果你在其他平台上遇到 bug，或希望增加新的 provider / model 预设，欢迎提交 issue。

### 常用配置

- `paperMirror.provider`: `deepseek`、`siliconflow`、`openai`、`ollama` 或 `custom`。
- `paperMirror.apiKey`: 可选 settings key；更推荐使用 `PaperMirror: Set API Key` 存到 SecretStorage。
- `paperMirror.baseUrl`: OpenAI-compatible base URL。
- `paperMirror.model`: 模型名。
- `paperMirror.updateMode`: `onSave` 或 `manual`。
- `paperMirror.translateComments`: 是否翻译 LaTeX 注释，默认 `false`。
- `paperMirror.showReferenceMarkersInTranslations`: 是否在中文译文中显示轻量 `[cite]`、`[ref]`、`[url]` 标记，默认 `false`，优先保证译文流畅度。
- `paperMirror.previewFontFamily` / `paperMirror.previewFontSize`: 中文预览字体和字号。
- `paperMirror.glossary`: 术语表，例如 `{ "graph-level prediction": "图级预测" }`。
- `paperMirror.maxBlocksPerRefresh`: 每次 refresh 最多翻译多少个 block，`0` 表示不限制。
- `paperMirror.maxBlocksPerRequest` / `paperMirror.maxRequestChars`: 控制每次 provider 请求大小。
- `paperMirror.requestTimeoutMs`: 单次 provider 请求超时时间。
- `paperMirror.enableJsonMode`: 是否请求 JSON 输出；如果某些兼容接口不支持 `response_format`，可以关闭。

### 长文档建议

第一次测试 20-30 页论文时，可以先设置：

```json
{
  "paperMirror.updateMode": "manual",
  "paperMirror.maxBlocksPerRefresh": 60
}
```

确认可用后，再改回：

```json
{
  "paperMirror.updateMode": "onSave",
  "paperMirror.maxBlocksPerRefresh": 0
}
```

第一次完整翻译可能需要几分钟；后续保存小改动时，PaperMirror 会优先更新新增或修改过的 block，并复用已有缓存。

### 当前边界

- v0.1.2 只支持当前活动 `.tex` 单文件，不递归解析完整 `\input` / `\include` 项目。
- PaperMirror 不是 TeX 编译器，不保证最终 PDF 排版一致。
- `algorithm2e`、复杂表格、TikZ 和自定义宏目前主要按占位处理。
- 公式预览依赖 MathJax，支持常见 inline/display math，但不承诺覆盖所有 TeX 宏包语法。
- 中文 preview 是写作辅助视图，不应作为正式译文或投稿材料。

### 隐私

PaperMirror 不会修改你的 `.tex` 源文件。翻译缓存保存在 VS Code extension storage 中，不写入你的论文仓库。API key 默认保存在 VS Code SecretStorage。

需要注意的是，待翻译的英文文本块会发送给你选择的 provider。使用第三方 API 时，请确认你的论文内容是否适合发送到对应服务。

<a id="english"></a>

## English

[中文](#中文)

### What Is PaperMirror?

PaperMirror is a **Chinese mirror preview tool for academic LaTeX writing**. You keep editing the English `.tex` source on the left, while PaperMirror shows a Simplified Chinese reading preview on the right. It never edits your source file.

In one sentence:

> Write English TeX as usual. Read a Chinese mirror beside it. Keep equations and references safe, and update only what changed.

PaperMirror is not a full document translator. It is a reading-first writing companion. Your English `.tex` file remains the single source of truth, and the Chinese preview is a disposable assistant view.

### Who Is It For?

PaperMirror is designed for researchers who write English LaTeX papers in VS Code or Cursor. It is useful when you want to:

- read a Chinese mirror while drafting an English paper;
- check logic and terminology without compiling a PDF after every sentence;
- avoid copying the whole paper into an external translator;
- keep equations, citations, labels, URLs, and LaTeX commands as safe as possible;
- translate only new or changed paragraphs in long documents;
- keep your existing LaTeX Workshop, Git, Copilot, and GitLens workflow;
- bring your own DeepSeek, SiliconFlow, OpenAI, Ollama, or custom OpenAI-compatible endpoint.

### Features

- **Chinese side preview** for the active `.tex` file.
- **Source-safe workflow**: PaperMirror never writes to your `.tex` source.
- **Save-triggered refresh** by default, with manual Refresh available.
- **Changed-block translation** with local cache reuse.
- **Progressive rendering for long documents**: cached blocks appear first, and pending blocks are patched chunk by chunk.
- **Placeholder protection** for inline math, display math, `cite`, `ref`, `label`, `url`, and related commands.
- **Local MathJax resources** for common equation rendering; custom macros are best-effort.
- **Paper-aware parsing** for headings, paragraphs, items, captions, abstracts, keywords, theorem-like environments, and common paper structures.
- **List and pseudocode support** for `itemize`, `enumerate`, `highlights`, and common `algorithm` + `algorithmic` environments.
- **Readable placeholders** for `figure`, `table`, `algorithm2e`, `tikzpicture`, and other complex environments.
- **Safe fallback**: if the provider returns invalid JSON, omits a block id, or drops a placeholder, PaperMirror keeps the original English block and shows a warning.
- **OpenAI-compatible providers** including SiliconFlow, DeepSeek, OpenAI, Ollama, and Custom endpoints.

### Install

#### Option 1: VS Code Marketplace

Search for:

```text
PaperMirror
```

If the extension was just published and does not appear yet, wait for Marketplace indexing or install the VSIX from GitHub Releases.

#### Option 2: GitHub Release / VSIX

1. Open [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases).
2. Download `papermirror-vscode-0.1.2.vsix`.
3. In VS Code or Cursor, choose `Extensions: Install from VSIX...`.
4. Open a `.tex` file.
5. Run `PaperMirror: Open Preview`.

### Quick Start

1. Open an English `.tex` file.
2. Run `PaperMirror: Set API Key`.
3. Run `PaperMirror: Validate API Connection`.
4. Run `PaperMirror: Open Preview`.
5. Edit and save the `.tex` file to refresh the preview.

SiliconFlow example:

```json
{
  "paperMirror.provider": "siliconflow",
  "paperMirror.baseUrl": "https://api.siliconflow.cn/v1",
  "paperMirror.model": "Pro/zai-org/GLM-4.7",
  "paperMirror.updateMode": "onSave",
  "paperMirror.translateComments": false,
  "paperMirror.showReferenceMarkersInTranslations": false,
  "paperMirror.previewFontFamily": "Microsoft YaHei",
  "paperMirror.previewFontSize": 15
}
```

Use the base URL only. Do not include `/chat/completions`; PaperMirror appends it automatically.

### API Compatibility

PaperMirror talks to models through an OpenAI-compatible Chat Completions interface. The author currently uses and tests PaperMirror mainly with **SiliconFlow**, because SiliconFlow is the provider available in the author's daily workflow.

DeepSeek, OpenAI, Ollama, and custom OpenAI-compatible endpoints are configurable, but different platforms may vary in response format, JSON stability, rate limits, and model names. If you encounter provider-specific bugs or want support for another provider/model preset, please open an issue.

### Common Settings

- `paperMirror.provider`: `deepseek`, `siliconflow`, `openai`, `ollama`, or `custom`.
- `paperMirror.apiKey`: optional settings key. SecretStorage through `PaperMirror: Set API Key` is recommended.
- `paperMirror.baseUrl`: OpenAI-compatible base URL.
- `paperMirror.model`: model name.
- `paperMirror.updateMode`: `onSave` or `manual`.
- `paperMirror.translateComments`: whether to translate LaTeX comments. Default: `false`.
- `paperMirror.showReferenceMarkersInTranslations`: whether translated Chinese text shows lightweight `[cite]`, `[ref]`, and `[url]` markers. Default: `false` for smoother translation quality.
- `paperMirror.previewFontFamily` / `paperMirror.previewFontSize`: Chinese preview font and size.
- `paperMirror.glossary`: optional terminology map, for example `{ "graph-level prediction": "图级预测" }`.
- `paperMirror.maxBlocksPerRefresh`: maximum translated blocks per refresh. `0` means unlimited.
- `paperMirror.maxBlocksPerRequest` / `paperMirror.maxRequestChars`: provider request size controls.
- `paperMirror.requestTimeoutMs`: per-request provider timeout.
- `paperMirror.enableJsonMode`: request JSON output when supported.

### Long Documents

For the first smoke test on a 20 to 30 page paper:

```json
{
  "paperMirror.updateMode": "manual",
  "paperMirror.maxBlocksPerRefresh": 60
}
```

After the smoke test works, switch back to progressive full preview:

```json
{
  "paperMirror.updateMode": "onSave",
  "paperMirror.maxBlocksPerRefresh": 0
}
```

The first full translation can take several minutes. Later saves should prioritize new or changed blocks and reuse cached translations.

### Limitations

- v0.1.2 supports the active `.tex` file only and does not recursively resolve a full `\input` / `\include` project.
- PaperMirror is not a TeX compiler and does not guarantee final PDF layout.
- `algorithm2e`, complex tables, TikZ, and custom macros are mostly represented with placeholders.
- Formula rendering depends on MathJax and is best-effort for complex TeX package syntax.
- The Chinese preview is a writing aid, not a formal translation for submission.

### Privacy

PaperMirror never modifies your `.tex` source file. The translation cache lives in VS Code extension storage, not in your paper repository. API keys are stored in SecretStorage by default.

Text blocks that need translation are sent to your selected provider. Please choose a provider that matches your paper confidentiality requirements.

## License

MIT.
