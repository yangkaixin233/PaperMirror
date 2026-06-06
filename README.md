<p align="center">
  <img src="media/icon.png" alt="PaperMirror logo" width="120">
</p>

# PaperMirror: VS Code / Cursor LaTeX 中文镜像预览插件

**写英文 TeX，右侧读中文镜像：面向论文作者的 LaTeX 中文预览、TeX AI 翻译与写作审阅工具。**

[VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=papermirror.papermirror-vscode) ·
[GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) ·
[English](#english)

如果 PaperMirror 对你的写作流程有帮助，欢迎在 VS Code Marketplace 评分或给 GitHub 仓库点 Star，这会帮助更多需要 LaTeX 中文预览的人找到它。

PaperMirror 是一个面向中文科研作者的 VS Code / Cursor 插件，用于把当前英文 `.tex` 文件渲染成右侧中文镜像预览。它适合英文 LaTeX 论文写作过程：写完一段、修改一段，或让大模型润色英文后，用中文快速检查含义、逻辑、术语和公式上下文。

常见搜索词：VS Code LaTeX 翻译、Cursor LaTeX preview、TeX 中文预览、LaTeX Chinese preview、LaTeX AI translation、OpenAI-compatible paper translation。

PaperMirror 不是 PDF 翻译器，也不是 TeX 编译器。它的核心是 **只翻译新增或修改过的 LaTeX block**，未变化内容尽量复用本地缓存，因此更适合在写作期间持续打开使用。

## 适合这些场景

- 在 VS Code 或 Cursor 里写英文 LaTeX，希望旁边有中文预览。
- 让大模型改完英文段落后，用中文镜像快速审阅是否偏离原意。
- 想用 OpenAI、DeepSeek、Ollama、SiliconFlow 或自定义接口翻译 `.tex`，但不想破坏公式和引用。
- 写 20 到 30 页论文时，希望只更新改过的段落，而不是每次重翻全文。
- 想找 LaTeX translation、Chinese preview、academic writing、MathJax preview 这一类 VS Code 插件。

## 核心特性

- **增量更新**：只翻译新增或修改过的 LaTeX block，未变化内容尽量复用缓存。
- **可强制全篇重译**：更多菜单中的 `Full Retranslate (costs tokens)` 会清空缓存并重译当前文件，适合缓存异常或预览大面积英文时使用。
- **翻译质量保护**：明显仍是英文的 provider 输出不会写入缓存；`Validate API Connection` 会检查 provider 是否真的输出中文。
- **长文档优先可用**：首次翻译长文档时优先处理当前 TeX 光标附近内容，并在 sticky header 显示进度。
- **可中止请求**：翻译中按钮显示 `Stop Translating`；停止后保留当前源码和已完成预览，未完成内容可再次刷新。
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

如果按功能搜索，也可以尝试：`LaTeX Chinese preview`、`LaTeX translation`、`TeX Chinese preview`。

### VSIX

也可以从 [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases) 下载 `papermirror-vscode-0.1.9.vsix`，然后通过以下命令安装：

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

保存后，PaperMirror 会刷新右侧中文预览。命中缓存时，只会请求改动过的 block。如果验证命令提示 provider 没有真正输出中文，请先更换模型或 provider，再清空 cache 重刷整篇论文。

### 首次配置四步

在 VS Code / Cursor Settings 里搜索 `PaperMirror` 后，先看前四项即可：

| 顺序 | 设置项 | 是否必须 | 说明 |
| --- | --- | --- | --- |
| 1 | `paperMirror.provider` | 必填 | 选择接口类型：OpenAI、DeepSeek、Ollama、SiliconFlow 或 Custom。 |
| 2 | API key | 云端 provider 必填 | 推荐运行 `PaperMirror: Set API Key`，密钥会存入 VS Code SecretStorage；也可以填写 `paperMirror.apiKey`，但安全性更低。Ollama 通常不需要。 |
| 3 | `paperMirror.baseUrl` | Custom 必填 | DeepSeek、SiliconFlow、OpenAI、Ollama 可留空使用预设；Custom 必须填写 base URL。不要包含 `/chat/completions`。 |
| 4 | `paperMirror.model` | 视 provider 而定 | DeepSeek、SiliconFlow 可留空使用默认模型；OpenAI、Ollama、Custom 通常必须填写模型名。 |

填完后运行 `PaperMirror: Validate API Connection`。VS Code 原生 Settings 页面目前没有内置测试按钮，所以验证需要通过命令面板（`Ctrl+Shift+P`）或 PaperMirror preview 右上角菜单执行。验证失败时，PaperMirror 会尽量指出具体原因：必填项缺失、API key 来源与格式、base URL、model、JSON Mode、网络超时或服务商返回错误。

### 使用建议

- **首次翻译长文档**：把光标停在最想先看的段落或章节附近，再打开预览或刷新。PaperMirror 会捕捉刷新开始时的 TeX 光标位置，优先翻译附近内容。
- **日常写作**：正常修改 `.tex` 后直接按 `Ctrl+S` 保存即可。默认保存刷新只会请求新增或修改过的 block，未改动内容会尽量复用本地缓存。
- **谨慎使用全篇重译**：预览右上角 `...` 菜单里的 `Full Retranslate (costs tokens)` 会清空缓存并把当前文件作为新文档重新翻译，通常只在缓存异常、预览大面积英文或当前翻译状态明显错误时使用。

## Provider 配置

PaperMirror 使用 OpenAI-compatible Chat Completions 接口。当前支持的 provider 预设：

- OpenAI
- DeepSeek
- Ollama
- SiliconFlow
- Custom OpenAI-compatible endpoint

DeepSeek 示例：

```json
{
  "paperMirror.provider": "deepseek",
  "paperMirror.baseUrl": "https://api.deepseek.com",
  "paperMirror.model": "deepseek-v4-flash",
  "paperMirror.updateMode": "onSave",
  "paperMirror.translateComments": false,
  "paperMirror.showReferenceMarkersInTranslations": false
}
```

SiliconFlow 常用设置：

```json
{
  "paperMirror.provider": "siliconflow",
  "paperMirror.baseUrl": "https://api.siliconflow.cn/v1",
  "paperMirror.model": "Pro/zai-org/GLM-4.7"
}
```

`paperMirror.baseUrl` 只填写 base URL，不要包含 `/chat/completions`，PaperMirror 会自动拼接。不同模型的翻译服从度差异很大，建议每次更换 provider、base URL 或 model 后先运行 `PaperMirror: Validate API Connection`。

## Marketplace 分类与关键词

扩展分类：

- Other

关键词覆盖：`latex`、`tex`、`latex translation`、`tex translation`、`latex preview`、`chinese preview`、`AI translation`、`academic writing`、`paper writing`、`research writing`、`MathJax`、`OpenAI`、`DeepSeek`、`Ollama`、`SiliconFlow`、`OpenAI-compatible`、`Cursor`。

## 重要设置

| 设置项 | 类型 | 说明 |
| --- | --- | --- |
| `paperMirror.provider` | 必填 | Provider 预设：`openai`、`deepseek`、`ollama`、`siliconflow` 或 `custom`。 |
| `paperMirror.apiKey` | 可选替代 | 不推荐长期使用；更安全的方式是运行 `PaperMirror: Set API Key`。 |
| `paperMirror.baseUrl` | 端点配置 | 预设 provider 可留空；Custom 必填。只填写 base URL。 |
| `paperMirror.model` | 模型配置 | DeepSeek、SiliconFlow 有默认值；OpenAI、Ollama、Custom 通常要自己填。 |
| `paperMirror.updateMode` | 常用 | `onSave` 或 `manual`。默认 `onSave`。 |
| `paperMirror.maxBlocksPerRequest` | 高级 | 单次 provider 请求最多包含多少个 block。 |
| `paperMirror.maxBlocksPerRefresh` | 高级 | 每次刷新最多翻译多少个 block，`0` 表示不限制。 |
| `paperMirror.translateComments` | 可选 | 是否翻译 LaTeX 注释，默认 `false`。 |
| `paperMirror.showReferenceMarkersInTranslations` | 可选 | 是否在译文中显示 `[cite]`、`[ref]`、`[url]`，默认 `false`。 |
| `paperMirror.previewFontFamily` / `paperMirror.previewFontSize` | 外观 | 中文预览字体和字号。 |
| `paperMirror.requestTimeoutMs` | 高级 | 单次 provider 请求超时时间。 |

每次修改 provider、base URL、model 或 API key 后，都建议先运行 `PaperMirror: Validate API Connection`，确认连接可用且模型会输出中文。失败提示会标明当前使用的 key 来源是 SecretStorage 还是 Settings；如果 Settings 看起来正确但仍然 401，优先重新运行 `PaperMirror: Set API Key`。

## 当前边界

- v0.1.9 只支持当前活动 `.tex` 单文件。
- 暂不递归解析完整 `\input` / `\include` LaTeX 项目。
- 长文档预览会复用缓存、渐进补全，并优先处理当前光标附近或缺失的 block；但实际完成速度仍取决于 provider 响应。
- 如果 `paperMirror.maxBlocksPerRefresh` 设置为大于 `0`，一次刷新可能只处理部分缺失 block，剩余内容需要再次刷新。
- 右键 `Retranslate block` 只重译选中的预览 block；如果 provider 仍返回英文或非法结果，该 block 会保留英文回退并显示 warning。
- `Full Retranslate (costs tokens)` 是恢复性操作，会清空当前文件缓存并重译当前 `.tex`，不适合日常保存刷新。
- PaperMirror 不是 TeX 编译器，不保证最终 PDF 排版一致。
- 复杂表格、TikZ、`algorithm2e` 和自定义宏目前主要以占位显示。
- 公式渲染依赖 MathJax，对复杂 TeX 宏包语法仅做 best-effort 支持。
- 中文预览是写作辅助视图，不应作为正式译文或投稿材料。

## 隐私

PaperMirror 不修改 `.tex` 源文件。需要翻译的文本 block 会发送给你配置的 provider。使用 `PaperMirror: Set API Key` 时，API key 默认存储在 VS Code SecretStorage 中。

## 常见问题

### PaperMirror 会修改我的 `.tex` 文件吗？

不会。PaperMirror 只读取当前活动 `.tex` 文件，并在右侧生成中文 preview。

### PaperMirror 是 PDF 翻译器吗？

不是。PaperMirror 面向 `.tex` 写作过程，用轻量 HTML preview 帮你在写作时阅读和审阅内容。

### 公式、引用和 label 会被翻译坏吗？

PaperMirror 会尽量保护公式、`\cite`、`\ref`、`\label`、URL 和常见 LaTeX 命令。复杂宏包语法仍按 best-effort 处理，失败时保留英文原文。

### 长文档会每次全部重翻吗？

不会。PaperMirror 按 block 缓存翻译结果，保存后优先翻译新增或修改过的内容。

### 清空 cache 后预览变英文怎么办？

先运行 `PaperMirror: Validate API Connection`。PaperMirror 会尽量识别 provider 返回的未翻译英文输出，并避免把这类结果写入缓存；如果验证失败，通常需要更换模型、provider 或 prompt 服从度更好的 endpoint。若缓存或局部预览状态异常，可以在预览右上角 `...` 菜单运行 `Full Retranslate (costs tokens)`，它会清空缓存并重新翻译当前文件。

### `Stop Translating` 会停止什么？

它只停止当前正在进行的翻译刷新请求，不会修改 `.tex` 源文件，也不会清空已经显示的预览。停止后可以再次点击 Refresh 继续翻译未完成 block。

### 支持哪些 provider？

当前支持 OpenAI、DeepSeek、Ollama、SiliconFlow，以及自定义 OpenAI-compatible Chat Completions endpoint。

## English

PaperMirror is a **VS Code / Cursor extension for LaTeX Chinese mirror preview**. It renders the active English `.tex` file into a Simplified Chinese preview so researchers can check meaning, logic, terminology, and formula context while drafting.

If PaperMirror helps your writing workflow, a VS Code Marketplace rating or GitHub Star helps more LaTeX writers discover it.

Search terms: VS Code LaTeX translation, Cursor LaTeX preview, TeX Chinese preview, LaTeX Chinese preview, AI translation for academic writing, OpenAI-compatible paper translation.

PaperMirror is not a whole-document translator. Its core behavior is **changed-block translation**: it re-translates only newly added or modified LaTeX blocks and reuses local cache for unchanged content whenever possible.

### Features

- **Incremental updates**: translate only newly added or modified LaTeX blocks, with local cache reuse for unchanged content.
- **Force full retranslation**: `Full Retranslate (costs tokens)` in the preview menu clears cache and retranslates the current file, useful when cache or preview state looks wrong.
- **Translation quality guard**: likely untranslated English provider output is not cached; `Validate API Connection` checks whether the provider can actually produce Chinese.
- **Long-document first use**: first-run translation starts near the current TeX cursor and keeps progress visible in the sticky header.
- **Cancelable refresh**: `Stop Translating` stops the in-flight translation request while keeping existing source and preview visible.
- **Source-safe**: reads `.tex` files but does not write to or modify your paper source.
- **Writing-friendly**: refreshes on save by default, with manual refresh also available.
- **Long-document support**: shows document structure and cached content first, then updates unfinished parts progressively.
- **LaTeX-aware**: tries to protect math, citations, refs, labels, URLs, and common LaTeX commands.
- **Safe fallback**: keeps the original English text when an individual block fails, instead of breaking the whole preview.
- **Flexible providers**: supports OpenAI, DeepSeek, Ollama, SiliconFlow, and custom OpenAI-compatible Chat Completions endpoints.

### Installation

Search **PaperMirror** in the VS Code or Cursor extension panel. Functional search terms include `LaTeX Chinese preview`, `LaTeX translation`, and `TeX Chinese preview`.

You can also download `papermirror-vscode-0.1.9.vsix` from [GitHub Releases](https://github.com/yangkaixin233/PaperMirror/releases), then install it with:

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

### First-Time Setup: Four Fields

Search `PaperMirror` in VS Code / Cursor Settings and check the first four settings first:

| Order | Setting | Required? | Notes |
| --- | --- | --- | --- |
| 1 | `paperMirror.provider` | Required | Choose OpenAI, DeepSeek, Ollama, SiliconFlow, or Custom. |
| 2 | API key | Required for cloud providers | Prefer `PaperMirror: Set API Key`, which stores the key in VS Code SecretStorage. `paperMirror.apiKey` also works but is less secure. Ollama usually does not need a key. |
| 3 | `paperMirror.baseUrl` | Required for Custom | DeepSeek, SiliconFlow, OpenAI, and Ollama can use preset defaults. Custom endpoints must set this. Do not include `/chat/completions`. |
| 4 | `paperMirror.model` | Depends on provider | DeepSeek and SiliconFlow have defaults. OpenAI, Ollama, and Custom usually need an explicit model name. |

After changing these fields, run `PaperMirror: Validate API Connection`. Native VS Code Settings does not provide an embedded test button, so validation is available from the Command Palette (`Ctrl+Shift+P`) or the PaperMirror preview menu. When validation fails, PaperMirror tries to explain the exact layer: missing required fields, API key source and format, base URL, model, JSON Mode, timeout, or provider-side errors.

### Usage Tips

- **First translation of a long document**: place the TeX cursor near the paragraph or section you want to read first before opening or refreshing the preview. PaperMirror captures the cursor position at refresh start and prioritizes nearby blocks.
- **Normal writing flow**: edit the `.tex` file and press `Ctrl+S`. Save-triggered refreshes send only newly added or modified blocks when cache is available.
- **Use full retranslation carefully**: `Full Retranslate (costs tokens)` in the preview `...` menu clears cache and retranslates the current file as a fresh document. Use it mainly for cache problems, large English fallback areas, or clearly broken preview state.

### Provider Configuration

PaperMirror uses OpenAI-compatible Chat Completions APIs. Supported provider presets:

- OpenAI
- DeepSeek
- Ollama
- SiliconFlow
- Custom OpenAI-compatible endpoint

DeepSeek example:

```json
{
  "paperMirror.provider": "deepseek",
  "paperMirror.baseUrl": "https://api.deepseek.com",
  "paperMirror.model": "deepseek-v4-flash",
  "paperMirror.updateMode": "onSave",
  "paperMirror.translateComments": false,
  "paperMirror.showReferenceMarkersInTranslations": false
}
```

Common SiliconFlow settings:

```json
{
  "paperMirror.provider": "siliconflow",
  "paperMirror.baseUrl": "https://api.siliconflow.cn/v1",
  "paperMirror.model": "Pro/zai-org/GLM-4.7"
}
```

`paperMirror.baseUrl` should be the base URL only. Do not include `/chat/completions`; PaperMirror appends it automatically. Run `PaperMirror: Validate API Connection` after changing provider, base URL, or model.

### Marketplace Categories And Keywords

Categories:

- Other

Keywords include `latex`, `tex`, `latex translation`, `tex translation`, `latex preview`, `chinese preview`, `AI translation`, `academic writing`, `paper writing`, `research writing`, `MathJax`, `OpenAI`, `DeepSeek`, `Ollama`, `SiliconFlow`, `OpenAI-compatible`, and `Cursor`.

### Key Settings

| Setting | Type | Description |
| --- | --- | --- |
| `paperMirror.provider` | Required | Provider preset: `openai`, `deepseek`, `ollama`, `siliconflow`, or `custom`. |
| `paperMirror.apiKey` | Optional alternative | Not recommended for long-term use; `PaperMirror: Set API Key` is safer. |
| `paperMirror.baseUrl` | Endpoint | Preset providers can leave this empty; Custom must set it. Use the base URL only. |
| `paperMirror.model` | Model | DeepSeek and SiliconFlow have defaults; OpenAI, Ollama, and Custom usually need an explicit model. |
| `paperMirror.updateMode` | Common | `onSave` or `manual`. Default: `onSave`. |
| `paperMirror.maxBlocksPerRequest` | Advanced | Maximum blocks per provider request. |
| `paperMirror.maxBlocksPerRefresh` | Advanced | Maximum blocks to translate per refresh. `0` means unlimited. |
| `paperMirror.translateComments` | Optional | Whether to translate LaTeX comments. Default: `false`. |
| `paperMirror.showReferenceMarkersInTranslations` | Optional | Whether to show `[cite]`, `[ref]`, and `[url]` in translations. Default: `false`. |
| `paperMirror.previewFontFamily` / `paperMirror.previewFontSize` | Appearance | Font family and font size for the Chinese preview. |
| `paperMirror.requestTimeoutMs` | Advanced | Timeout for each provider request. |

After changing provider, base URL, model, or API key, run `PaperMirror: Validate API Connection` to confirm that the endpoint works and the selected model actually returns Chinese. The failure message reports whether the key came from SecretStorage or Settings; if Settings looks correct but validation returns 401, rerun `PaperMirror: Set API Key` first.

### Current Scope

- v0.1.9 supports the active `.tex` file only.
- Full recursive `\input` / `\include` project parsing is not supported yet.
- Long-document preview reuses cache, fills content progressively, and prioritizes blocks near the current cursor or missing from cache; actual completion speed still depends on provider latency.
- If `paperMirror.maxBlocksPerRefresh` is set above `0`, one refresh may process only part of the missing queue, and remaining content needs another refresh.
- `Retranslate block` retries only the selected preview block; if the provider still returns English or invalid output, that block stays as English fallback with a warning.
- `Full Retranslate (costs tokens)` is a recovery action: it clears the current-file cache and retranslates the current `.tex`, so it is not intended for normal save refreshes.
- PaperMirror is not a TeX compiler and does not guarantee final PDF layout fidelity.
- Complex tables, TikZ, `algorithm2e`, and custom macros are mostly shown as placeholders.
- Math rendering uses MathJax and supports complex TeX package syntax on a best-effort basis.
- The Chinese preview is a writing aid, not a formal translation or submission artifact.

### Privacy

PaperMirror does not modify your `.tex` source file. Text blocks that need translation are sent to the provider you configure. When using `PaperMirror: Set API Key`, the API key is stored in VS Code SecretStorage by default.

### FAQ

#### Does PaperMirror modify my `.tex` file?

No. It reads the active `.tex` file and renders a Chinese preview beside it.

#### Is PaperMirror a PDF translator?

No. PaperMirror is designed for `.tex` drafting and lightweight writing-time review.

#### Can it protect formulas and citations?

PaperMirror tries to protect equations, citations, labels, URLs, and common LaTeX commands. Complex package-specific syntax is handled on a best-effort basis.

#### Does it re-translate the whole paper after every save?

No. It caches translations by block and prioritizes newly added or modified content.

#### What if the preview turns English after clearing cache?

Run `PaperMirror: Validate API Connection` first. PaperMirror tries to detect provider output that is still English and avoids caching it. If validation fails, switch to a model, provider, or endpoint that follows the Chinese translation prompt more reliably. If cache or preview state looks wrong, use `Full Retranslate (costs tokens)` from the preview `...` menu to clear cache and retranslate the current file.

#### What does `Stop Translating` stop?

It stops only the current translation refresh request. It does not modify the `.tex` source file and does not clear the already visible preview. Click Refresh again to continue unfinished blocks.

## License

MIT.
