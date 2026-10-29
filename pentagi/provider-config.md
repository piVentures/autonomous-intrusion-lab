# Provider and Agent Configuration

## 1. The Multi-Agent Model Strategy
PentAGI allows assigning different LLMs (or different parameters) to each agent type. This is crucial for balancing cost, speed, and reasoning capability [citation:5][citation:7].
- **Primary Agent / Pentester:** Needs the strongest model (highest reasoning).
- **Searcher / Enricher:** Can use a cheaper/faster model.
- **Summarizer:** Can use a lightweight model to compress context.

## 2. Gemini Configuration
Since Gemini was a native provider, the `.env` configuration was minimal:
```bash
GEMINI_API_KEY=your_api_key_here
GEMINI_SERVER_URL=https://generativelanguage.googleapis.com
The agent-specific model mapping was controlled internally by PentAGI's default Gemini profile, which routed heavy tasks to gemini-3.5-flash-lite and lighter tasks to gemini-3.1-flash-lite.

3. Custom Provider (Groq) and Token Limits
For Groq, a custom provider YAML was required. The critical fix was reducing max_tokens to avoid the 8,000 TPM free-tier limit.
The YAML file (example.custom.provider.yml) was modified so that every agent reserved a smaller output buffer:
```
primary_agent:
  model: "openai/gpt-oss-120b"
  max_tokens: 1024  # Reduced from 4000 to prevent 413 errors
```

4. Ollama Context Window (The Local Failure)
For local Ollama, the default context window is too small. PentAGI requires up to 110K tokens of context for complex flows . A custom Modelfile was created to increase num_ctx:-11

FROM qwen3.5:9b
PARAMETER num_ctx 16384
However, even with this configuration, CPU inference latency exceeded the orchestration timeout, causing the flow to fail before the first agent could even plan.
