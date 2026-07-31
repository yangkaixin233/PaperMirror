# PaperMirror v0.1.10

PaperMirror v0.1.10 adds Markdown translation while preserving the existing LaTeX writing workflow.

## Highlights

- Translate active `.tex`, `.md`, and `.markdown` files into a Simplified Chinese mirror preview.
- Preserve Markdown headings, lists, inline/display math, inline code, and fenced code.
- Follow the active document when switching between multiple supported files.
- Reuse cached translations and request only new, changed, or cache-missing prose blocks.
- Retry an individual preview block without retranslating the full document.

## Notes

- The source file remains unchanged.
- Markdown tables and deeply nested extension syntax currently use basic rendering.
- Real provider traffic has mainly been tested with SiliconFlow. Compatibility reports for DeepSeek, OpenAI, Ollama, and custom OpenAI-compatible endpoints are welcome.

## Package

- File: `papermirror-vscode-0.1.10.vsix`
- SHA-256: `56CC059708F3E5CC3E7FD6C3E5F7CCE6559600E2F2B053CD9A34127F145DE875`
