# PaperMirror：给中文科研工作者的 LaTeX / Markdown 中文翻译侧栏

<p align="center">
  <img src="media/icon.png" width="128" alt="PaperMirror logo">
</p>

PaperMirror 是给中文科研工作者用的 VS Code / Cursor 插件，适合在 LaTeX 或 Markdown 里写英文科技论文时使用。

你在左边照常编辑英文 `.tex` 或 `.md`，右边会显示对应的中文。常见 LaTeX 公式会按数学排版显示，读中文时仍能顺着公式理解上下文。按 `Ctrl+S` 保存后，它只更新刚修改的段落，已经翻译好的内容继续保留。

写作时，可以用它确认英文有没有准确表达原意。让大模型改完文章后，也可以直接看中文结果，检查逻辑、术语和公式上下文有没有跑偏。

> 源文件始终保持原样。PaperMirror 只生成用于阅读的中文侧栏。

[中文说明](#中文说明) | [English](#english)

## 中文说明

### 安装

在 VS Code 或 Cursor 的扩展商店中搜索 **PaperMirror**，安装：

[PaperMirror: Chinese Preview for LaTeX & Markdown](https://marketplace.visualstudio.com/items?itemName=papermirror.papermirror-vscode)

也可以从 [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) 下载 `.vsix`，然后运行 `Extensions: Install from VSIX...`。

### 首次配置

PaperMirror 使用你自己的大模型接口。只需要聊天模型，不需要 embedding 模型。

1. 打开 Settings，搜索 `PaperMirror`，在 `Provider`（`paperMirror.provider`）中选择接口。
2. 按 `Ctrl+Shift+P`，运行 `PaperMirror: Set API Key`。
3. 在 Settings 中检查 `Base URL`（`paperMirror.baseUrl`）和 `Model`（`paperMirror.model`）。
4. 运行 `PaperMirror: Validate API Connection`。看到连接成功后再开始翻译长文档。

不同接口需要填写的内容如下：

| 接口 | Base URL | Model | API key |
| --- | --- | --- | --- |
| DeepSeek | 可留空，默认 `https://api.deepseek.com` | 可留空，默认 `deepseek-v4-flash` | 必填 |
| OpenAI | 可留空，默认 `https://api.openai.com/v1` | 必填 | 必填 |
| Ollama | 可留空，默认 `http://localhost:11434/v1` | 必填，例如 `qwen3:8b` | 通常不需要 |
| SiliconFlow | 可留空，默认 `https://api.siliconflow.cn/v1` | 必填，填写账户当前可用的聊天模型 ID | 必填 |
| Custom | 必填，只填写 base URL | 必填 | 由接口决定 |

SiliconFlow 的模型列表会变化，建议从控制台或官方 [`GET /v1/models`](https://docs.siliconflow.cn/en/api-reference/models/get-model-list) 查询当前模型 ID。

API key 默认保存在 VS Code SecretStorage。Settings 中的 `paperMirror.apiKey` 只适合临时测试。

### 日常使用

1. 打开一个 `.tex`、`.md` 或 `.markdown` 文件。
2. 按 `Ctrl+Shift+P`，运行 `PaperMirror: Open Preview`。
3. 在左侧修改英文，按 `Ctrl+S` 保存。
4. 在右侧检查更新后的中文。

正常写作时只需要保存文件。没有修改的段落会继续使用本地译文，不会整篇重新请求。

如果一篇长文档是第一次翻译，把光标停在最想先看的段落附近。PaperMirror 会先处理附近内容，再逐步补全文档。翻译过程中可以切换到另一篇文章，原来的任务会在后台继续；切回来时可以看到已经完成的部分。

用大模型修改论文后，也可以直接保存 `.tex` 或 `.md`。PaperMirror 会更新发生变化的段落，方便你用中文重新审阅改写结果。

### 预览中会显示什么

- 标题、正文、列表和 caption 会翻译成中文。
- 行内公式和行间公式由内置 MathJax 渲染，无需安装 TeX。
- 引用 key、ref key、label 和 URL 不会发送给模型。你可以在 Settings 中选择是否在译文里显示 `[cite]`、`[ref]` 和 `[url]`。
- 图、表、TikZ 和暂不支持的算法环境会显示简洁占位，能够安全提取的 caption 会翻译。
- LaTeX `%` 注释和 Markdown HTML 注释默认不翻译。

PaperMirror 用来辅助写作和阅读，不负责最终排版。编译和检查提交版 PDF 时，请继续使用 LaTeX Workshop、Overleaf 或原有 TeX 工具链。

### 某一段没有翻译时

- 点击该段旁边的 `Retry translation`，只重新翻译这一段。
- 点击预览顶部的 Refresh，重新检查当前文件中修改或缺失的译文。
- 使用 `Full Retranslate Current File (Costly)` 可以忽略当前文件的旧译文并整篇重译。这个操作会消耗较多 token。
- `Stop Translating` 只停止当前文档，已经完成的内容会保留。
- `Show Logs` 可以查看模型名、超时、网络、代理和接口返回错误。
- `Clear Cache` 会删除所有文档的本地译文，通常无需使用。

### 常用命令

| 命令 | 用途 |
| --- | --- |
| `PaperMirror: Open Preview` | 打开中文侧栏 |
| `PaperMirror: Refresh Preview` | 更新当前文件中修改或缺失的译文 |
| `PaperMirror: Full Retranslate Current File (Costly)` | 忽略当前文件缓存并整篇重译 |
| `PaperMirror: Set API Key` | 安全保存 API key |
| `PaperMirror: Validate API Connection` | 检查接口、模型、密钥和中文输出 |
| `PaperMirror: Open Settings` | 打开插件设置 |
| `PaperMirror: Show Logs` | 查看诊断日志 |
| `PaperMirror: Clear Cache` | 删除全部本地翻译缓存 |

### 常用设置

| 设置 | 默认值 | 用途 |
| --- | ---: | --- |
| `paperMirror.updateMode` | `onSave` | 保存时更新，或改为只手动更新 |
| `paperMirror.showReferenceMarkersInTranslations` | `false` | 在译文中显示 `[cite]`、`[ref]`、`[url]` |
| `paperMirror.translateComments` | `false` | 翻译源码注释 |
| `paperMirror.requestTimeoutMs` | `60000` | 单次接口请求的超时时间 |
| `paperMirror.previewFontFamily` | `system` | 中文预览字体 |
| `paperMirror.previewFontSize` | `14.5` | 中文预览字号 |

PaperMirror 会读取 VS Code / Cursor 的 `http.proxy`，只在插件请求中使用。它不会修改系统环境变量或用户代理设置。

### 当前支持范围

- 当前按单个活动文件预览，尚未递归读取完整的 `\input` / `\include` 项目。
- 常见 LaTeX 结构和公式可以正常显示。复杂自定义宏、特殊宏包环境仍可能显示原文或占位。
- Markdown 支持标题、段落、列表、行内/行间公式、行内代码和 fenced code。复杂表格和深层嵌套语法仍按基础形式处理。
- 翻译速度取决于接口、模型、网络和代理。PaperMirror 默认不自动重试，避免重复等待和额外 token 消耗。

### 隐私和接口测试

- PaperMirror 不修改源文件。
- 翻译缓存保存在 VS Code extension storage，不会写入论文目录。
- 只有需要翻译的文字会发送到你配置的接口。
- 作者主要使用 SiliconFlow 做过真实接口测试。DeepSeek、OpenAI、Ollama 和 Custom 使用相同的 OpenAI-compatible 调用方式并有自动化测试。如遇兼容问题，请在 [GitHub Issues](https://github.com/yangkaixin233/PaperMirror/issues) 中提供接口名称、Base URL、模型 ID 和脱敏后的错误日志。

## English

PaperMirror is a VS Code and Cursor extension for Chinese-speaking researchers who write English papers in LaTeX or Markdown.

Keep editing the English `.tex` or `.md` file on the left and read a Chinese translation on the right. Common LaTeX equations remain mathematically typeset in the preview, so the surrounding Chinese text and formulas can be read together. When you save with `Ctrl+S`, PaperMirror updates the paragraphs you changed and keeps the rest of the preview. This is useful for checking whether the English still matches your intended meaning, reviewing text revised by an LLM, and reading sections with many equations or references.

Your source file stays unchanged. PaperMirror only creates a Chinese reading view.

### Install

Search for **PaperMirror** in the VS Code or Cursor extension marketplace, or open:

[PaperMirror: Chinese Preview for LaTeX & Markdown](https://marketplace.visualstudio.com/items?itemName=papermirror.papermirror-vscode)

You can also download a `.vsix` from [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) and run `Extensions: Install from VSIX...`.

### First setup

PaperMirror uses your own model endpoint. It needs a chat model and does not need an embedding model.

1. Open Settings, search for `PaperMirror`, and select a provider.
2. Run `PaperMirror: Set API Key` from the Command Palette.
3. Check the Base URL and Model fields.
4. Run `PaperMirror: Validate API Connection` before translating a long document.

| Provider | Base URL | Model | API key |
| --- | --- | --- | --- |
| DeepSeek | Preset: `https://api.deepseek.com` | Preset: `deepseek-v4-flash` | Required |
| OpenAI | Preset: `https://api.openai.com/v1` | Required | Required |
| Ollama | Preset: `http://localhost:11434/v1` | Required | Usually not required |
| SiliconFlow | Preset: `https://api.siliconflow.cn/v1` | Required, use a currently available chat model ID | Required |
| Custom | Required | Required | Endpoint-dependent |

SiliconFlow model availability changes. Check its console or the official [`GET /v1/models`](https://docs.siliconflow.cn/en/api-reference/models/get-model-list) endpoint for a current model ID.

API keys are stored in VS Code SecretStorage by default. The `paperMirror.apiKey` setting is intended for temporary testing.

### Daily workflow

1. Open a `.tex`, `.md`, or `.markdown` file.
2. Run `PaperMirror: Open Preview`.
3. Edit the English source and save with `Ctrl+S`.
4. Review the updated Chinese text on the right.

For a large first translation, leave the cursor near the section you want to read first. PaperMirror translates that area first and fills in the rest progressively. You can switch to another supported document while the first one continues in the background.

After an LLM revises your paper, save the updated source. PaperMirror refreshes the changed paragraphs so you can review the revision in Chinese.

### What appears in the preview

- Headings, prose, lists, and captions are translated.
- Inline and display math are rendered with the bundled MathJax.
- Citation keys, reference keys, labels, and URLs are removed before model requests. Optional `[cite]`, `[ref]`, and `[url]` markers can be enabled in Settings.
- Figures, tables, TikZ, and unsupported algorithm environments use lightweight placeholders.
- LaTeX `%` comments and Markdown HTML comments are skipped by default.

PaperMirror is a writing and reading aid. Continue using LaTeX Workshop, Overleaf, or your existing TeX toolchain for final PDF compilation.

### If a paragraph stays in English

- Click `Retry translation` to request that paragraph again.
- Use Refresh to check changed or missing translations in the current file.
- Use `Full Retranslate Current File (Costly)` only when the whole file needs to be translated again.
- Use `Show Logs` for model, timeout, network, proxy, and endpoint errors.
- `Clear Cache` removes local translations for every document and is rarely needed.

### Current scope

- Preview works on one active source file at a time. Recursive `\input` and `\include` projects are not supported yet.
- Common LaTeX structures and formulas are supported. Complex custom macros and package-specific environments may fall back to source text or placeholders.
- Markdown support covers headings, paragraphs, lists, inline and display math, inline code, and fenced code.
- Translation time depends on the endpoint, model, network, and proxy. Automatic provider retries are disabled by default to avoid extra waiting and token use.

### Privacy and provider testing

PaperMirror keeps source files unchanged and stores translations in VS Code extension storage. Only text that needs translation is sent to the endpoint you configure.

The author has mainly tested real API traffic with SiliconFlow. DeepSeek, OpenAI, Ollama, and Custom use the same OpenAI-compatible request format and have automated coverage. Please report compatibility problems in [GitHub Issues](https://github.com/yangkaixin233/PaperMirror/issues) with the provider, Base URL, model ID, and redacted logs.

## License

[MIT](LICENSE)
