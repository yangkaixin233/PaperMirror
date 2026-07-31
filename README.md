# PaperMirror: 科研写作 LaTeX / Markdown 中文镜像（VS Code / Cursor）

<p align="center">
  <img src="media/icon.png" width="128" alt="PaperMirror logo">
</p>

**写英文 TeX 或 Markdown，右侧读中文镜像。公式和引用保持安全，只更新改过的段落。**

[中文](#中文) · [English](#english)

---

## 中文

PaperMirror 是面向非英语母语科研作者的轻量写作辅助插件。你继续在 VS Code 或 Cursor 中维护英文 `.tex` / `.md` 源文件，PaperMirror 在右侧生成可随时重建的中文阅读镜像，帮助检查论述逻辑、术语和公式上下文。

PaperMirror 不修改源文件，也不替代 LaTeX Workshop、Overleaf 或最终 PDF 编译。

### 核心能力

- **增量翻译**：保存后只请求新增、修改或缓存缺失的 block，未变化内容复用本地缓存。
- **长文档渐进预览**：先显示文档结构和缓存结果，再逐批填充译文；优先处理当前光标附近的内容。
- **LaTeX-aware**：发送请求前保护公式、代码和脆弱命令；引用 key、ref key、label 和 URL 不发送给模型。
- **本地公式渲染**：内置 MathJax，无需安装 TeX，也不依赖 CDN。
- **安全回退**：请求失败、返回英文或占位符异常时保留英文，并标记 `English fallback`。
- **可恢复操作**：支持停止翻译、右键单块重译和整篇重新翻译。
- **多文档切换**：切换活动 `.tex`、`.md` 或 `.markdown` 文件时，预览跟随当前文档。
- **Provider 可选**：支持 DeepSeek、SiliconFlow、OpenAI、Ollama 和自定义 OpenAI-compatible Chat Completions 接口。

翻译只需要聊天模型，不需要 embedding 模型。

### 安装

- 在 [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=papermirror.papermirror-vscode) 搜索 `PaperMirror` 安装。
- 也可以下载 [PaperMirror v0.1.10 VSIX](papermirror-vscode-0.1.10.vsix)，然后在 VS Code / Cursor 中执行 `Extensions: Install from VSIX...`。

### 首次配置（四步）

打开 VS Code / Cursor Settings，搜索 `PaperMirror`。最前面的 **PaperMirror: 1. Setup (Required)** 包含必填项。

1. 选择 `paperMirror.provider`。
2. 云端接口运行 `PaperMirror: Set API Key`，将密钥保存到 VS Code SecretStorage。
3. 检查 `paperMirror.baseUrl` 和 `paperMirror.model`。
4. 按 `Ctrl+Shift+P`，运行 `PaperMirror: Validate API Connection`。

验证成功后再打开长文档，可避免模型名失效或接口设置错误造成大面积英文回退。

| Provider | Base URL | Model | API key |
| --- | --- | --- | --- |
| DeepSeek | 可留空，默认 `https://api.deepseek.com` | 可留空，默认 `deepseek-v4-flash` | 必填 |
| OpenAI | 可留空，默认 `https://api.openai.com/v1` | 必填 | 必填 |
| SiliconFlow | 可留空，默认 `https://api.siliconflow.cn/v1` | **必填，请填写账户当前可用的 chat 模型 ID** | 必填 |
| Ollama | 可留空，默认 `http://localhost:11434/v1` | 必填，如 `qwen3:8b` | 通常不需要 |
| Custom | 必填，只填 base URL | 必填 | 由接口决定 |

SiliconFlow 模型会调整，PaperMirror 不再内置容易过期的默认模型。可通过 SiliconFlow 控制台或官方 [`GET /v1/models`](https://docs.siliconflow.cn/en/api-reference/models/get-model-list) 查询当前模型 ID。DeepSeek 预设依据其[官方模型说明](https://api-docs.deepseek.com/quick_start/pricing)。

> `paperMirror.apiKey` 设置项只用于便携测试，安全性低于 SecretStorage。日常使用推荐命令 `PaperMirror: Set API Key`。

### 日常使用

1. 打开一个 `.tex`、`.md` 或 `.markdown` 文件。
2. 按 `Ctrl+Shift+P`，运行 `PaperMirror: Open Preview`。
3. 编辑源文件并按 `Ctrl+S`。
4. 右侧只更新发生变化或缓存缺失的部分。

首次翻译大文档时，把光标停在最想先阅读的位置。PaperMirror 会优先翻译光标附近内容，然后继续处理全文。

打开多个支持文件时，点击左侧编辑器中的目标文件即可切换预览。后台文件保存不会抢走当前预览。

### 恢复操作

- **右键预览段落 → Retranslate block**：仅重新请求该段，适合单个英文回退块。
- **`...` → Full Retranslate (costs tokens)**：当前文件忽略旧缓存并重新翻译全部可翻译 block。其他文件的缓存保留。
- **Stop Translating**：停止当前长任务，已经完成的内容继续保留。
- **Clear Cache**：清空整个扩展翻译缓存。该操作影响所有文档，通常无需使用。

整篇重译会消耗较多 token。正常写作只需保存文件。

### 命令

| Command | 用途 |
| --- | --- |
| `PaperMirror: Open Preview` | 打开或定位中文镜像 |
| `PaperMirror: Refresh Preview` | 手动刷新当前文件 |
| `PaperMirror: Full Retranslate Current File (Costly)` | 忽略当前文件缓存，整篇重译 |
| `PaperMirror: Set API Key` | 将密钥写入 SecretStorage |
| `PaperMirror: Validate API Connection` | 验证 base URL、模型、密钥和实际中文输出 |
| `PaperMirror: Open Settings` | 打开插件设置 |
| `PaperMirror: Show Logs` | 查看请求、缓存和解析诊断 |
| `PaperMirror: Clear Cache` | 清空全部翻译缓存 |

### 常用设置

| Setting | 默认值 | 说明 |
| --- | ---: | --- |
| `paperMirror.updateMode` | `onSave` | 保存刷新或手动刷新 |
| `paperMirror.showReferenceMarkersInTranslations` | `false` | 是否在中文译文中补充 `[cite]` / `[ref]` / `[url]` |
| `paperMirror.translateComments` | `false` | 是否翻译 LaTeX `%` 注释和 Markdown HTML 注释 |
| `paperMirror.maxBlocksPerRefresh` | `0` | `0` 表示处理全部缺失/修改块；正数仅用于限额测试 |
| `paperMirror.maxBlocksPerRequest` | `12` | 单次 API 请求的 block 上限 |
| `paperMirror.maxRequestChars` | `9000` | 单次请求的近似字符预算 |
| `paperMirror.requestTimeoutMs` | `60000` | 单次请求超时 |
| `paperMirror.previewFontFamily` | `system` | 中文预览字体 |
| `paperMirror.previewFontSize` | `14.5` | 中文预览字号 |

PaperMirror 会读取 VS Code / Cursor 的 `http.proxy` 并仅在本扩展请求中使用，不修改系统环境变量或用户代理设置。

### 当前边界

- 当前按活动单文件工作，尚未递归解析完整 `\input` / `\include` 项目。
- LaTeX 解析和公式渲染为 best effort；复杂自定义宏、特殊宏包环境可能显示占位或英文回退。
- `figure`、`table`、部分算法和 TikZ 默认显示轻量占位；可安全抽取的 caption 会翻译。
- Markdown 当前覆盖标题、段落、列表、行内/行间公式、行内代码和 fenced code；复杂表格与嵌套扩展语法仍按普通文本处理。
- 翻译速度取决于 provider、模型、网络和代理。PaperMirror 默认不做自动重试，以控制等待时间和 token 消耗。

### 隐私与测试范围

- 源文件不会被 PaperMirror 修改。
- API key 推荐保存在 VS Code SecretStorage。
- 缓存保存在 VS Code extension storage，不写入论文仓库。
- 只有待翻译的文本 block 会发送到用户配置的 provider；citation/ref key、label 和 URL 会在发送前移除。
- 作者主要使用 SiliconFlow 做过真实接口测试。其他 provider 采用相同的 OpenAI-compatible 协议并有自动化测试；如遇兼容问题，请在 [GitHub Issues](https://github.com/yangkaixin233/PaperMirror/issues) 提交 provider、base URL、模型名和脱敏错误日志。

---

## English

PaperMirror is a lightweight Chinese mirror preview for non-native English researchers writing academic LaTeX or Markdown. Keep editing the English `.tex` / `.md` source in VS Code or Cursor; PaperMirror renders a disposable Chinese reading view beside it for checking logic, terminology, and mathematical context.

It never modifies the source and does not replace LaTeX Workshop, Overleaf, or final PDF compilation.

### Key features

- **Incremental translation**: on save, request only new, changed, or cache-missing blocks.
- **Progressive long-document preview**: show structure and cached content first, then patch translations batch by batch, prioritizing the current cursor area.
- **LaTeX-aware protection**: protect math, code, and fragile commands before requests; citation keys, reference keys, labels, and URLs are not sent to the model.
- **Local MathJax rendering**: no TeX installation and no CDN dependency.
- **Safe fallback**: keep English source and label the block when a request, quality check, or placeholder check fails.
- **Recovery actions**: stop translation, retranslate one preview block, or fully retranslate the current file.
- **Document switching**: follow the active `.tex`, `.md`, or `.markdown` file without background saves replacing the preview.
- **Flexible providers**: DeepSeek, SiliconFlow, OpenAI, Ollama, and custom OpenAI-compatible Chat Completions endpoints.

PaperMirror needs a chat model only. It does not require an embedding model.

### Installation

- Install `PaperMirror` from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=papermirror.papermirror-vscode).
- Or download the [PaperMirror v0.1.10 VSIX](papermirror-vscode-0.1.10.vsix), then run `Extensions: Install from VSIX...` in VS Code or Cursor.

### First setup

Open Settings, search for `PaperMirror`, and complete **PaperMirror: 1. Setup (Required)**:

1. Select `paperMirror.provider`.
2. For cloud providers, run `PaperMirror: Set API Key`.
3. Check `paperMirror.baseUrl` and `paperMirror.model`.
4. Run `PaperMirror: Validate API Connection` from the Command Palette.

| Provider | Base URL | Model | API key |
| --- | --- | --- | --- |
| DeepSeek | Preset: `https://api.deepseek.com` | Preset: `deepseek-v4-flash` | Required |
| OpenAI | Preset: `https://api.openai.com/v1` | Required | Required |
| SiliconFlow | Preset: `https://api.siliconflow.cn/v1` | **Required; use a currently available chat model ID** | Required |
| Ollama | Preset: `http://localhost:11434/v1` | Required | Usually not required |
| Custom | Required | Required | Endpoint-dependent |

SiliconFlow model availability changes, so PaperMirror does not hard-code a default. Check the platform console or the official [`GET /v1/models`](https://docs.siliconflow.cn/en/api-reference/models/get-model-list) endpoint.

### Normal workflow

Open a supported source file, run `PaperMirror: Open Preview`, then edit and save with `Ctrl+S`. Unchanged blocks reuse local cache. For a large first translation, place the cursor near the section you want to read first.

When several supported files are open, select a source editor to switch the preview. Saving a background file does not replace the pinned preview document.

### Recovery actions

- Right-click a preview block and choose `Retranslate block` to retry only that block.
- Use `Full Retranslate (costs tokens)` to bypass cached translations for every translatable block in the current file. Other documents keep their cache.
- Use `Stop Translating` to stop the active run while preserving completed blocks.
- Use `Clear Cache` only when you intentionally want to remove the extension-wide translation cache.

### Current scope

- Active single-file preview; recursive `\input` / `\include` projects are not supported yet.
- Best-effort LaTeX parsing and MathJax rendering for complex custom macros and package environments.
- Lightweight placeholders for figures, tables, some algorithms, and TikZ.
- Markdown support covers headings, paragraphs, lists, inline/display math, inline code, and fenced code; complex tables and nested extensions remain basic.
- Provider latency and network/proxy quality determine request time. Automatic retries are disabled by default to avoid extra waiting and token use.

### Privacy and provider testing

PaperMirror keeps source files unchanged, stores cache in VS Code extension storage, and recommends SecretStorage for API keys. Only translation blocks are sent to the configured provider; citation/reference keys, labels, and URLs are removed first.

The author has mainly tested real API traffic with SiliconFlow. Other providers share the OpenAI-compatible protocol and automated tests. Please report compatibility problems through [GitHub Issues](https://github.com/yangkaixin233/PaperMirror/issues) with the provider, base URL, model ID, and redacted logs.

## License

[MIT](LICENSE)
