# Troubleshooting: Real-World Issues Encountered

## 1. "Failed to select primary docker image via llm call" (Context Canceled)
**Symptom:** The flow stuck on "The flow is starting..." and died with a `context canceled` error.
**Root Cause:** This is almost always a model backend issue, not Docker [citation:4]. The model (Ollama) took too long to respond to the initial "image selection" prompt, exceeding the timeout.
**Fix:** Switch to a faster provider (Gemini) or increase GPU acceleration for local models.

## 2. Groq 413 "Request Too Large" / Token Limits
**Symptom:** The agent failed mid-task with `413 Request too large` and a mention of `tokens per minute (TPM): Limit 8000`.
**Root Cause:** PentAGI is verbose. It sends the system prompt, tool definitions, and full history on every turn. A single request can exceed 8K tokens.
**Fix:** Reduce `max_tokens` in the provider YAML to 1024, and use a provider with higher TPM limits (Gemini).

## 3. Ollama Latency (58 Seconds for One Word)
**Symptom:** The model "works" via `ollama run`, but fails in PentAGI.
**Root Cause:** CPU inference requires reading the entire model weights from RAM for every token. On a 12GB CPU-only machine, Time-to-First-Token (TTFT) is 58+ seconds. PentAGI's orchestration timeout is shorter than this.
**Fix:** Agentic AI requires GPU acceleration or a cloud API. Local CPU is not viable for multi-agent frameworks.

## 4. Network Isolation and Target Reachability
**Symptom:** The agent could not reach the target VMs or API services despite them being active on the host.
**Root Cause:** Docker's default security posture isolates bridge networks from each other and from the host's loopback interface. Services bound to the host's localhost are invisible to the container network.
**Troubleshooting Approach:**
- **Verify the target is listening on all interfaces.** If a service is bound only to loopback, the container cannot see it. Bind it to `0.0.0.0` or route the agent through the Docker host gateway.
- **Check the agent's routing table.** Ensure the sandbox container has a valid route to the target's subnet. If the route is missing, the container will silently drop the packets.
- **Review firewall policies.** Docker's `FORWARD` chain defaults to `DROP`. If the target is on a separate virtual bridge (like a VM network), explicit allow rules are required.
- **Connect the right container.** The framework spawns a new, temporary sandbox container for every flow. Attaching the main application container to the target network does not guarantee the sandbox can reach it.
