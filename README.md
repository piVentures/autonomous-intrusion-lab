# Governed Autonomous Intrusion

A reproducible laboratory for measuring how autonomous an AI agent actually is when it performs offensive security operations — and where it still needs a human.

## Scope and Problem

Agentic AI frameworks increasingly claim they can perform penetration testing autonomously. Those claims are rarely tested under controlled conditions with verifiable evidence. The gap between vendor claims and operational reality is the problem this repository addresses.

This lab asks one question: **Can an AI agent independently complete a multi-stage intrusion objective, or does it still rely on an experienced human operator for critical decisions?**

To answer it, we deployed a self-hosted agentic framework against intentionally vulnerable targets inside an isolated laboratory, applied explicit governance (target allowlists, network isolation, kill switch), and documented every command, every intervention, and every failure. The result is not a demonstration. It is an empirical record of the autonomy boundary.

## What is PentAGI

PentAGI is an open-source, self-hosted, MIT-licensed penetration testing framework built around a team of specialized AI agents. A Primary Agent receives an objective and delegates to specialists: a Pentester for exploitation, a Searcher for intelligence, a Coder for custom payloads, an Installer for tooling, an Adviser for strategy, a Memorist for recall, and a Reflector that forces the model back to action when it drifts.

Every tool call executes inside an isolated Docker sandbox. The framework is model-agnostic, supporting local inference engines and multiple commercial and free cloud providers through a unified configuration layer. It exposes the full agent decision trace through its UI and API, which makes it auditable in a way that most closed frameworks are not.

## Why PentAGI

Several open-source agentic frameworks were considered for this research. The selection criteria were not benchmark supremacy but **observability of the decision loop**.

The research question requires knowing *who decided what, and why*. A framework that produces results but hides the reasoning chain is unsuitable for this kind of evaluation. PentAGI was selected because its multi-agent delegation model is explicit: an orchestrator, named specialists, and a full log of every delegation and tool call. That transparency is the decisive factor.

The full comparison of candidate frameworks, including their actual levels of autonomy, deployment complexity, and limitations, is documented in the research report.

## Repository Structure

```text
autonomous-intrusion-lab/
├── lab/           Target inventory, allowlist, and network isolation evidence
├── pentagi/       Sanitized framework configuration (Docker Compose, provider YAML, model settings)
├── trials/        Per-trial documentation, including prompts, traces, and verification
│   ├── kirubel/   Individual trial logs and evidence
│   └── abrham/    Individual trial logs and evidence
├── report/        Research report, metrics table, and AI usage log
└── video/         Walkthrough script
Each trial directory contains the exact prompt given to the agent, the target definition, the raw execution trace, and an independent verification of the agent's claims. Trials that did not achieve their objective are documented with a post-mortem rather than discarded.

Trials
The repository documents a representative set of trials spanning multiple attack surfaces: multi-stage web-to-root intrusion, network service exploitation, and API-level authorization flaws. Each trial was conducted under an explicit target allowlist and is supported by raw terminal traces, screenshots, and independent verification.

Additional targets are being documented. The trials included here are examples of the lab's methodology, not an exhaustive list.

Reproduction
Sanitized configuration files for the agentic stack are in pentagi/. The target allowlist and network isolation evidence are in lab/. The full experimental record — prompts, traces, metrics, interventions, and independent verification — is in trials/ and report/.

The complete research report, including the framework comparison, trial analysis, cost measurements, and final operational recommendation, is in report/report.md.
