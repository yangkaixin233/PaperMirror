# Privacy

PaperMirror does not modify your `.tex` source file.

When translation is requested, PaperMirror sends selected LaTeX prose blocks to the API provider configured by the user. The exact data handling policy depends on the provider you choose, such as SiliconFlow, DeepSeek, OpenAI, Ollama, or a custom OpenAI-compatible endpoint.

PaperMirror stores API keys in VS Code SecretStorage by default when using `PaperMirror: Set API Key`. Users may also place API keys in VS Code settings for testing, but that is less secure.

Local translation cache is stored in the extension storage area rather than in your paper repository.
