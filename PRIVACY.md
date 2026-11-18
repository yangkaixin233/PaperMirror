# Privacy

PaperMirror does not modify your LaTeX or Markdown source files (`.tex`, `.md` and `.markdown`).

When translation is requested, PaperMirror sends the prose that needs translation to the API endpoint you configure. This applies to both LaTeX and Markdown. The connection test uses a built-in example sentence. Data handling by the endpoint depends on the service you choose, such as SiliconFlow, DeepSeek, OpenAI, local Ollama, or a custom OpenAI-compatible endpoint.

API keys entered on the PaperMirror setup page are stored in the editor's SecretStorage. The page displays whether a key is saved without returning the saved key to its form. The legacy `PaperMirror: Set API Key` command opens this same page.

Existing `paperMirror.apiKey` settings remain supported for compatibility. An active key from those older settings is migrated to SecretStorage when you save through the setup page. Use the setup page for new keys rather than placing them in settings files.

Local translation cache is stored in the extension storage area rather than in your paper repository.
