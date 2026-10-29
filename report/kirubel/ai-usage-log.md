# AI Usage and Intervention Log (Kirubel)

This log documents the AI models, tools, and human interventions used during the trials.

## Models Used
- **Framework:** PentAGI (v2.2.0-ce)
- **Primary Model:** Google Gemini (3.1 Flash-Lite & 3.5 Flash-Lite) via Free Tier.
- **Failed Model:** Qwen 3.5 (9B & 4B) via local Ollama.

## Interventions
- **Mr. Robot Trial:** 0 human interventions. The agent autonomously chained the WordPress RCE, hash crack, and SUID nmap escalation.
- **Metasploitable2 (Autonomous) Trial:** 0 human interventions. The agent autonomously discovered port 1524 and wrote a Python socket script to connect.
- **crAPI (BOLA) Trial:** 0 human interventions. The agent adapted to 404 errors by parsing the React JS bundle to find the API routes.
- **Ollama Trial:** The flow failed at startup (58s latency). Intervention was required to kill the stuck flow and switch the provider to Gemini.
- **Docker Network Isolation:** Multiple manual interventions were required to connect the `pentagi` container to the `virbr0` and `docker_default` networks to allow the agent to reach the targets.

## Prompt Engineering
- Prompts were governed by explicit scope (target IP), rules of engagement (no destructive commands), and forced verification (provide raw HTTP 200 OK or `uid=0(root)`). This structure was critical to prevent the agent from hallucinating success.
