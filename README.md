<p align="center">
  <img src="media/icon.png" alt="PaperMirror logo" width="128">
</p>

# PaperMirror VS Code/Cursor Extension

Write English LaTeX, read a Chinese mirror beside it.

PaperMirror is a lightweight LaTeX translator preview for non-native English researchers. It keeps your `.tex` source unchanged, translates prose blocks into Simplified Chinese, preserves equations and LaTeX commands as much as possible, and refreshes a side preview while you write in VS Code or Cursor.

> Current release: **VSIX preview only**. Source code will be published after repository cleanup and privacy review.

[中文说明](#中文说明) | [English](#english)

## 中文说明

### 适合谁

PaperMirror 面向正在写英文科技论文、但希望在编辑 LaTeX 时同步阅读中文镜像的科研作者。它不是 TeX 编译器，也不替代 LaTeX Workshop、Overleaf 或 PDF 预览。

### 当前效果

- 支持 VS Code 和 Cursor。
- 在右侧显示中文论文镜像预览。
- 保存 `.tex` 后刷新，也支持手动刷新。
- 按 LaTeX block 增量翻译，尽量复用缓存。
- 支持长文档渐进式预览，未完成内容会显示 loading。
- 默认不翻译被 `%` 注释掉的段落。
- 尽量保护公式、引用、label、ref、url 等 LaTeX 结构。
- 图、表、算法等复杂环境先以占位形式显示。
- 支持 OpenAI-compatible Chat Completions provider。

### 安装

1. 下载 Release 里的 `papermirror-vscode-0.1.2.vsix`。
2. 在 VS Code 或 Cursor 中打开 Extensions 面板。
3. 选择 `Install from VSIX...`。
4. 安装后打开一个 `.tex` 文件。
5. 运行命令 `PaperMirror: Open Preview`。

### API 配置

PaperMirror 支持这些 provider 预设：

- SiliconFlow
- DeepSeek
- OpenAI
- Ollama
- Custom OpenAI-compatible endpoint

作者当前主要使用 **SiliconFlow** 测试。如果你使用 DeepSeek、OpenAI、Ollama 或自定义 OpenAI-compatible API 遇到问题，欢迎在 GitHub Issues 反馈。

推荐使用命令：

- `PaperMirror: Set API Key`
- `PaperMirror: Validate API Connection`
- `PaperMirror: Open Settings`

API key 默认存入 VS Code SecretStorage。也可以在 Settings 中配置，但这种方式安全性更低。

### 重要设置

- `paperMirror.provider`
- `paperMirror.baseUrl`
- `paperMirror.model`
- `paperMirror.updateMode`
- `paperMirror.translateComments`
- `paperMirror.showReferenceMarkersInTranslations`
- `paperMirror.previewFontFamily`
- `paperMirror.previewFontSize`
- `paperMirror.maxBlocksPerRefresh`

### 已知边界

- 当前版本不是完整 TeX 编译器。
- 复杂公式和自定义宏可能无法完全渲染。
- 图、表、TikZ、算法等复杂环境目前主要以占位展示。
- 第一次翻译长文档可能较慢，后续保存会尽量复用缓存。
- 除 SiliconFlow 外的 provider 兼容性仍需要更多用户反馈。

## English

### What It Is

PaperMirror is a lightweight VS Code/Cursor extension for researchers who write English LaTeX papers and want a readable Chinese mirror preview while editing. It does not modify your source file and is not a replacement for a TeX compiler, LaTeX Workshop, Overleaf, or final PDF review.

### Features

- Works in VS Code and Cursor.
- Shows a Chinese mirror preview beside your `.tex` editor.
- Refreshes on save or manually.
- Translates changed LaTeX blocks incrementally.
- Reuses local cache when possible.
- Supports progressive preview for long documents.
- Skips commented-out LaTeX paragraphs by default.
- Preserves formulas, citations, labels, refs, and URLs as much as possible.
- Shows placeholders for figures, tables, algorithms, and other complex environments.
- Uses OpenAI-compatible Chat Completions providers.

### Install

1. Download `papermirror-vscode-0.1.2.vsix` from GitHub Releases.
2. Open the Extensions panel in VS Code or Cursor.
3. Choose `Install from VSIX...`.
4. Open a `.tex` file.
5. Run `PaperMirror: Open Preview`.

### Providers

Supported presets:

- SiliconFlow
- DeepSeek
- OpenAI
- Ollama
- Custom OpenAI-compatible endpoint

This preview release has mainly been tested with **SiliconFlow**. If you use another provider and find a bug or need a new provider preset, please open an issue.

### Privacy Note

PaperMirror sends selected LaTeX prose blocks to the configured API provider for translation. Your `.tex` source is not modified. API keys are stored in VS Code SecretStorage by default when using `PaperMirror: Set API Key`.

## License

MIT.
