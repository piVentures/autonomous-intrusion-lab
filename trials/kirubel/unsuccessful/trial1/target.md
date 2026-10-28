# Target Definition: Juice Shop (Ollama Failure Trial)

- **Target Name:** OWASP Juice Shop
- **IP Address:** 172.17.0.1:3000
- **Objective:** Run a simple `curl -I` command to verify connectivity.
- **Why this target:** This is the simplest possible task. There is no complex reasoning required. If the agent fails here, the failure is purely due to infrastructure (model latency), not task complexity.
- **Model Used:** `qwen3.5:4b-pentagi` and `qwen3.5:9b-pentagi` (Local, via Ollama).
