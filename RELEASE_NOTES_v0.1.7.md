# PaperMirror v0.1.7

This release focuses on long-paper usability, recovery controls, and safer translation output for the VS Code / Cursor LaTeX Chinese mirror preview workflow.

## 新增功能

- Added `PaperMirror: Full Retranslate Current File (Costly)` and the preview menu action `Full Retranslate (costs tokens)`.
- Added a right-click preview-block menu with `Retranslate block`, `Copy text`, and `Reveal source`.
- Added cursor-near priority for first-run translation of long `.tex` documents.
- Added sticky translation progress so long-paper refreshes do not look stalled.
- Added provider validation that checks whether the configured model actually returns Chinese.

## 改进与修复

- Save refresh now fills all missing cache-cleared blocks by default.
- Likely untranslated English provider output is no longer cached.
- Stale English cache entries are ignored and re-enter the translation queue.
- Duplicated math/display placeholder tokens are repaired more gracefully.
- `Stop Translating` now makes clear that it only stops the in-flight request and keeps visible preview content.
- README and Marketplace-facing copy now document v0.1.7 behavior, full retranslation, single-block retry, and long-document boundaries.

## 使用提示

- Normal writing flow: edit `.tex`, press `Ctrl+S`, and let PaperMirror translate changed or missing blocks.
- For a long paper, place the TeX cursor near the section you want to read first before opening or refreshing the preview.
- Use `Full Retranslate (costs tokens)` only when cache state or provider fallback leaves a large preview mostly English.
- Use right-click `Retranslate block` when only one paragraph needs another provider request.

## 当前边界

- v0.1.7 supports the active `.tex` file only.
- Full recursive `\input` / `\include` project parsing is not supported yet.
- Complex tables, TikZ, `algorithm2e`, and custom macros are still handled on a best-effort basis.
- The Chinese preview is a writing aid, not a formal translation or submission artifact.

## Verification

- `npm test`: 47 tests passed.
- `npm run build`: core and VS Code extension builds passed.
- VSIX generated: `papermirror-vscode-0.1.7.vsix`.
