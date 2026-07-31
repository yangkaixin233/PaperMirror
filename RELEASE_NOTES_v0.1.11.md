# PaperMirror v0.1.11

PaperMirror v0.1.11 improves long-document work when several LaTeX or Markdown files are open.

## Highlights

- An unfinished document can continue translating in the background after you switch files.
- The visible document receives bounded foreground priority while provider concurrency remains one request.
- Each open document keeps an independent preview snapshot; background and stale results cannot replace the visible preview.
- Returning to a document restores its existing progress instead of restarting its provider requests.
- The English fallback action is now labeled `Retry translation`.
- The Chinese-first README now explains installation, required provider settings, normal save-based updates, long-document behavior, and recovery actions from the user's workflow.

## Verification

- Core tests: 64 passed.
- VS Code extension tests: 26 passed.
- Build and TypeScript checks passed.
- Production dependency audit reported 0 vulnerabilities.
- A real provider smoke test used two synthetic long LaTeX documents. Both completed, continued across editor switches, restored their own previews, and rendered inline/display math without stale-result replacement or duplicate provider requests.
- The package contains no test paper, bibliography, PDF, API key, or personal manuscript path.

## Package

- File: `papermirror-vscode-0.1.11.vsix`
- Size: 19,919,400 bytes
- SHA-256: `7ED9426503B28BD7CB2914D0EE2CF6484E928535CAF52F57F03075DED8E86D05`
