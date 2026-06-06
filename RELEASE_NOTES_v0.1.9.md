# PaperMirror v0.1.9

Settings and validation diagnostics release.

This release focuses on making first-time setup easier to diagnose. `PaperMirror: Validate API Connection` now gives more specific feedback for missing required settings, API key source and format, base URL/model mismatch, JSON Mode incompatibility, timeout, network/proxy failures, and provider-side errors.

User-facing changes:

- README now puts the required setup fields earlier: provider, API key, base URL, and model.
- Validation logs now show sanitized setup context, including endpoint, model, key source, timeout, JSON Mode state, and VS Code proxy setting.
- Network failures now include more useful cause details when available.

Package:

- VSIX generated: `papermirror-vscode-0.1.9.vsix`.

Verification:

- `npm run build`
- `npm test`
