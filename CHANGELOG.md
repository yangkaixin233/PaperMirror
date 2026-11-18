# Changelog

## 1.0.0

First stable release, bringing together the changes developed after 0.1.11.

- Chinese comparison for English LaTeX and Markdown, rendered formulas, and cursor-linked navigation.
- Persistent change marks with earlier Chinese and formula comparisons, plus fixes for block matching, external refresh and Clear.
- More reliable selected-block translation, cancellation, retries and MathJax rendering.
- A Chinese setup page with required-field hints, editable models, official API-key links, secure key storage and one-click connection testing. Advanced options stay collapsed.
- Chinese store name, description and search keywords, while keeping the existing extension ID.
- A rewritten README with an eight-second demo, everyday usage first and setup/reference details afterwards.

## 0.1.16

- Rewrite the README around Chinese comparison, persistent change marks and formula preview, with an eight-second VS Code demo and shorter setup instructions. Document cursor-linked navigation from the English source to the Chinese preview.
- Fix incorrect “Show before” pairings when a revision deletes, edits, and adds paragraphs together. Uncertain matches stay as additions and deletions; very large document reorders are no longer silently ignored.
- Retain local change-detection source for table contents, figure paths, standalone labels, Markdown links and code, and compare heading/list structure without changing translation cache keys or sending this extra source to the model.
- Show the earlier rendered formula for edited display equations, with controls outside the nodes MathJax replaces.
- Refresh an already tracked preview when an external tool rewrites its clean, open source document in `onSave` mode. Keep unsaved typing and manual mode out of automatic translation.
- Establish a baseline immediately when change marks are re-enabled, so the next edit is marked.
- Keep late translation updates from restoring marks after Clear, while preserving translation text and later edits.

## 0.1.15

- Mark changed blocks in the preview so AI-assisted revisions are easy to review. After each refresh, blocks edited or added since the preview first opened the file (or since the marks were last cleared) get a colored bar and an `Edited` / `New` tag, and deleted blocks leave a `Deleted` marker in place. Colors follow the editor's git gutter colors.
- `Show before` expands the earlier Chinese of an edited block (or its earlier source if it was never translated); deleted markers expand to the removed text.
- A change bar under the status line summarizes the marks (`Changes since 10:32: 2 edited · 1 new · 1 deleted`) with `Prev` / `Next` navigation that also reveals the source, and `Clear marks` to accept the current text as the new baseline. `PaperMirror: Clear Change Marks` does the same from the Command Palette.
- Changes are detected on the raw source, so edits inside formulas, citations, or labels are marked even though they reuse cached translations. Marks accumulate across several saves until cleared, so an assistant that writes the file more than once cannot hide an earlier edit.
- New setting `paperMirror.showChangeMarkers` (default on) turns the marks off.

## 0.1.14

- Restore formula rendering in VS Code webviews when MathJax's speech worker cannot load local resources. Disable generated speech and braille for the current preview, including when saved MathJax menu preferences enable them; preserve SVG math, semantic enrichment, and saved preferences.
- Fix a selected-block retry started while the document is idle getting stuck at `Retranslating...` because the preview dropped its result.
- Stop endlessly re-requesting blocks whose correct translation contains no Chinese (author lists, proper nouns); after two attempts the latest output is accepted and cached, and explicit retry still forces a new request.
- Parse nested lists with the same environment name and single-line `\begin{itemize} \item ... \end{itemize}` lists correctly, without leaking `\begin`/`\item` markers into the preview.
- Keep math inside Markdown inline code spans rendered as the original text instead of a literal placeholder token.
- Retry transient provider failures (429/5xx/network) with backoff, and split a batch that hits the model's output token limit instead of dropping the whole batch to English fallback.
- Anchor selected-block retry to the block's content hash so retrying after unsaved edits above the block no longer targets the wrong block.

## 0.1.13

- Unify full-document translation and selected-block retries under one cancellable operation per document.
- Make `Stop Translating` cancel queued document and block work, restore unfinished blocks, and return to `Refresh` without a sticky `Stopping...` state.
- Ignore late results and delayed Stop commands from superseded operations.
- Show selected-block retry feedback immediately and keep its request at urgent queue priority.
- Let an explicit block retry use the current unsaved editor text for that block without saving the source or translating other dirty blocks.
- Serialize webview updates and restore lifecycle state authoritatively after preview reloads or style changes.
- Allow a save-triggered document refresh to join an active selected-block retry for the same source.

## 0.1.12

- Allow a selected block to be retried while the document is still translating.
- Show the selected block's retranslating state immediately and prioritize its request after the active network call.
- Prevent document progress updates from overwriting a selected-block retry result.
- Reject English-only provider responses for ordinary prose so they remain visible safe fallbacks instead of entering the translation cache.
- Mark blocks deferred by an explicit per-refresh limit and provide a clear single-block translation action.

## 0.1.11

Multi-document background translation and scheduling release.

- Continue unfinished long-document translations in the background after switching to another open LaTeX or Markdown file.
- Keep provider concurrency at one request while giving the visible document bounded foreground priority.
- Preserve an independent preview snapshot for each open document so background results cannot replace the visible preview.
- Keep save debounce and cancellation scoped to the affected document.
- Rename the clickable English fallback action to `Retry translation`.
- Rewrite the Chinese-first README around the normal research-writing workflow.
- Add scheduling regression coverage and complete a real two-long-document provider smoke test.
- VSIX generated: `papermirror-vscode-0.1.11.vsix`.

## 0.1.10

LaTeX and Markdown mirror-preview release.

- Added Chinese mirror translation for active `.md` and `.markdown` files.
- Added Markdown-aware blocks for headings, paragraphs, lists, inline/display math, inline code, and fenced code.
- Improved active-document switching across multiple LaTeX and Markdown editors.
- Added safer recovery for cache-missing blocks and per-block retranslation.
- Kept incremental save refreshes: only new, changed, or cache-missing prose blocks are requested.
- Updated the bilingual README and provider setup guidance.
- VSIX generated: `papermirror-vscode-0.1.10.vsix`.

## 0.1.9

Settings and validation diagnostics release.

Added:

- Added clearer first-time setup guidance in the README for provider, API key, base URL, and model.
- Added more actionable `PaperMirror: Validate API Connection` diagnostics for missing settings, API key source, base URL/model mismatch, JSON Mode incompatibility, timeout, network/proxy failures, and provider-side errors.
- Added sanitized validation logs that include provider, endpoint, model, key source, JSON Mode state, timeout, and VS Code proxy setting without exposing API keys.

Changed:

- Provider fetch failures now include useful low-level cause details when available, such as network code, address, and port.
- README and VSIX references now point to `papermirror-vscode-0.1.9.vsix`.

Verification:

- `npm run build`
- `npm test`
- VSIX generated: `papermirror-vscode-0.1.9.vsix`.

## 0.1.8

Maintenance release.

- Fixed known issues.
- Corrected Marketplace category metadata by removing mismatched categories.
- VSIX generated: `papermirror-vscode-0.1.8.vsix`.

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
