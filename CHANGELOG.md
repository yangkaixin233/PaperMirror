# Changelog

## 0.1.2

Initial VSIX preview release.

- Added VS Code/Cursor LaTeX Chinese mirror preview.
- Added OpenAI-compatible provider presets.
- Added API key setup and connection validation commands.
- Added progressive preview for long documents.
- Added local caching for unchanged blocks.
- Added default skipping for commented-out LaTeX text.
- Added optional reference markers in translated preview.
- Added preview font family and font size settings.
- Added local MathJax resources for formula rendering.

Known limitations:

- Formula rendering is best-effort and may not cover all custom LaTeX macros.
- Complex environments such as figures, tables, algorithms, and TikZ are represented with placeholders.
- Provider compatibility beyond SiliconFlow needs more user feedback.
