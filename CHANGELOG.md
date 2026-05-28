# Changelog

## 0.1.7

Focused release for long-paper usability, recovery controls, and safer translation output.

Added:

- Added `PaperMirror: Full Retranslate Current File (Costly)` and the preview menu action `Full Retranslate (costs tokens)` for cache recovery and large English fallback areas.
- Added a right-click preview-block menu with `Retranslate block`, `Copy text`, and `Reveal source`.
- Added cursor-near priority for the first translation of long documents, with sticky progress visible while translation is running.
- Added safer provider validation so `PaperMirror: Validate API Connection` checks whether the configured provider can actually produce Chinese.
- Added translation-quality guards that avoid caching likely untranslated English provider output and ignore stale English cache entries.
- Added configurable `paperMirror.maxBlocksPerRefresh` behavior for very large papers, with `0` as the default full missing-block queue.

Changed:

- Save-triggered refresh now fills all missing cache-cleared blocks by default instead of leaving most of the document as source fallback.
- `Stop Translating` now has clearer status feedback and keeps already visible preview content intact.
- Placeholder repair is more tolerant of duplicated math/display placeholders while still falling back when required placeholders are missing.
- README and Marketplace-facing text now explain the long-document workflow, provider validation, full retranslation, and current v0.1.7 boundaries.

Known limitations:

- v0.1.7 still supports the active `.tex` file only and does not recursively parse full `\input` / `\include` projects.
- Full retranslation clears the current-file cache and consumes provider tokens; it is intended as a recovery action, not the normal save workflow.
- Complex tables, TikZ, `algorithm2e`, and custom macros are still handled on a best-effort basis.

Verification:

- `npm test`: 47 tests passed.
- `npm run build`: core and VS Code extension builds passed.
- VSIX generated: `papermirror-vscode-0.1.7.vsix`.

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
