# PentAGI Lab Setup and Infrastructure

## 1. Host Environment
The lab was deployed on a Kali Linux host with the following specifications:
- **RAM:** 12 GB (Shared between the PentAGI Docker stack, local models, and target VMs).
- **GPU:** None (This was a critical constraint, leading to the Ollama latency failure documented in the trials).
- **Storage:** 300 GB.
- **Network:** Isolated Docker bridge networks (`pentagi-network`, `docker_default`) and libvirt bridge (`virbr0`).

## 2. Core Stack Deployment
PentAGI was deployed using Docker Compose. The core stack consists of four services:
- **pentagi:** The main application server (Go backend + React frontend) listening on port `8443`. This orchestrates the agents and tools.
- **pgvector:** PostgreSQL with the vector extension. This is the memory system (long-term, working, and episodic) [citation:1][citation:2].
- **scraper:** An isolated Chromium browser service for safe web reconnaissance.
- **pgexporter:** Prometheus exporter for database metrics.

*(Note: Optional stacks like Langfuse, Graphiti, and Grafana were disabled to conserve RAM on the 12GB machine.)*

## 3. Model Providers (The "Brain")
The agent is model-agnostic. The lab experimented with more than three providers:
- **Ollama (Local):** Failed due to CPU latency (58 seconds Time-to-First-Token). See `trials/kirubel/unsuccessful` for the post-mortem.
- **Groq (Free Tier):** Failed due to an 8,000 Token-Per-Minute limit that multi-agent prompts easily exceed.
- **Google Gemini (Free Tier):** The successful provider. Models used were `gemini-3.1-flash-lite` and `gemini-3.5-flash-lite`. The native integration required only the `GEMINI_API_KEY` in the `.env` file [citation:8].
