# Trial 2 — Post-Mortem and Decision to Explore Cloud APIs

## Outcome
**Tool-call compatibility improved; operational feasibility remained poor.** Qwen returned three correctly structured calls in the direct test, and PentAGI later produced a response. However, a simple request reportedly took almost one hour on the remote CPU-only VM. No end-to-end autonomous intrusion or target compromise was demonstrated.

## Assessment
The model switch addressed the specific Mistral collector failure in a controlled direct test. It did not make the workflow practical. The observed latency would multiply across reconnaissance, decision-making, follow-on actions, and recovery steps. The evidence does not isolate the exact latency source between inference, PentAGI orchestration, retries, or tool execution.

## Formal decisions
1. **Separate functional correctness from performance.** Passing structured tool calls is necessary but insufficient.
2. **Do not declare the trial a successful intrusion.** A response and a passed collector test are not evidence of exploitation or end-to-end autonomy.
3. **Stop relying on CPU-only 7B models as the primary engine for long trials.** Retain local Qwen as a fallback or small integration test model.
4. **Evaluate remote inference.** Candidate providers discussed: Gemini, xAI/Grok, and Cerebras.
5. **Apply the zero-spend and built-in-provider constraints.** Prioritize Gemini free-tier eligible models via PentAGI's built-in provider UI; use Grok only if the account actually has free API allowance; Cerebras was not confirmed as a dedicated built-in PentAGI provider. Free-tier quotas may interrupt long runs.
6. **Keep the later Blue lab isolated.** Because Kali/PentAGI runs remotely and Blue is a local VMware VM, VMware host-only networking alone cannot connect them. Plan an authenticated, tightly restricted lab VPN/router path before target tests; do not expose Blue to the public internet.

## Future experiment design
Use identical authorized lab targets and initial conditions, preserve prompts, timestamp all model/tool interactions, record decision latency and retry counts, and score reconnaissance, initial-access decisions, follow-on actions, failure recovery, and final objective completion separately. No such later results are included here.

## Limitations
Reported latency is approximate; no full trace confirms how much time was spent in model generation versus orchestration. No evidence yet supports conclusions about Qwen's effectiveness at compromising Blue.
