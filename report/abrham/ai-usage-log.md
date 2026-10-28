# AI Usage Log

## 1. Overview

This document describes how Artificial Intelligence (AI) was used throughout the autonomous penetration-testing research project.

Two distinct forms of AI usage were involved:

1. **ChatGPT (GPT-6):** Used as a research assistant for technical guidance, troubleshooting, interpreting execution traces, developing experiment prompts, and documenting results.
2. **PentAGI (Gemini-powered):** Used as the autonomous penetration-testing agent responsible for independently conducting security assessments against the authorized laboratory targets.

The distinction is important because ChatGPT assisted the researcher with preparation, configuration, and analysis, while PentAGI performed the autonomous security assessment activities.

---

# 2. Researcher's Use of ChatGPT (GPT-6)

## 2.1 AI Model Used

- **AI Assistant:** ChatGPT
- **Model:** GPT-6
- **Purpose:** Research assistance, configuration, troubleshooting, prompt development, and result interpretation.

GPT-6 was the only conversational AI assistant used directly by the researcher throughout the experiments.

The researcher used ChatGPT to understand technical problems, develop solutions, and prepare the experimental environment.

ChatGPT did not independently execute penetration-testing operations against the target machines.

## 2.2 Environment Setup and Configuration

ChatGPT assisted with configuring the cybersecurity research environment.

The environment consisted of:

- Kali Linux as the attacker workstation.
- Docker for containerized services.
- PentAGI as the autonomous penetration-testing framework.
- Gemini models as PentAGI's AI providers.
- VMware Workstation for hosting vulnerable target machines.
- Tailscale for secure communication between the remote Kali environment and the local VMware network.

AI assistance was used to understand and resolve configuration challenges, including:

- Docker installation and container permissions.
- PentAGI deployment and configuration.
- AI provider configuration.
- Network connectivity between Kali and VMware.
- Tailscale subnet routing.
- IP forwarding and routing configuration.
- Connectivity verification between PentAGI containers and target machines.

ChatGPT provided explanations and troubleshooting instructions, while the researcher executed the necessary commands and verified their results.

## 2.3 Troubleshooting and Bug Resolution

During the experiments, several technical issues were encountered.

ChatGPT was used to analyze error messages, understand their causes, and recommend troubleshooting procedures.

Examples included:

### Docker Permission Issues

The researcher encountered permission errors when attempting to communicate with the Docker daemon.

ChatGPT explained the relationship between Linux user permissions, the Docker socket, and container management.

### Network Connectivity Issues

The research environment required communication between a remote Kali Linux machine and vulnerable VMware machines.

ChatGPT assisted in understanding and troubleshooting:

- VMware host-only networking.
- Tailscale connectivity.
- Subnet route advertisement.
- Windows IP forwarding.
- Target IP address changes.
- Container-to-target connectivity.

### PentAGI Configuration Issues

ChatGPT helped explain PentAGI's deployment structure and configuration requirements.

This included troubleshooting AI-provider connectivity, Docker networking, and service availability.

### Execution Errors

When commands or configurations failed, their outputs were provided to ChatGPT for analysis.

ChatGPT helped identify possible causes and suggested corrective actions.

The researcher remained responsible for applying and verifying these corrections.

## 2.4 Understanding PentAGI Execution Traces

ChatGPT was used to interpret PentAGI's execution traces and autonomous decision-making.

The researcher provided:

- Agent reasoning summaries.
- Tool execution results.
- Subtask reports.
- Vulnerability findings.
- Error messages.
- Execution statistics.

ChatGPT helped explain the technical meaning of these outputs and assess whether PentAGI's conclusions were supported by evidence.

For example, during the DEV experiment, PentAGI reported successful Local File Inclusion (LFI) exploitation and administrative access to BoltWire CMS.

Further analysis distinguished these achievements from operating-system command execution.

Although PentAGI demonstrated file disclosure, the available evidence did not establish successful remote command execution or root access.

This analysis helped prevent unsupported claims from being included in the final research report.

## 2.5 AI-Assisted Prompt Engineering

ChatGPT was used to develop the mission prompts supplied to PentAGI.

Different prompt strategies were intentionally tested to observe their effects on autonomous behavior.

### Experiment 1: Blue — Well-Defined Prompt

For the Blue experiment, ChatGPT helped develop a structured and detailed mission prompt.

The prompt included:

- A clearly defined target.
- Authorized network scope.
- Experimental objectives.
- Operational instructions.
- Rules of engagement.
- Evidence requirements.
- Expected reporting outcomes.

The objective was to provide PentAGI with sufficient context to conduct the assessment independently while maintaining clear boundaries.

**Result:**

The Blue experiment was reported successful, with root access achieved.

Recorded execution metrics:

- Tasks: 1
- Subtasks: 4
- Assistants: 0
- Tool calls: 19
- Duration: 6 minutes, 37 seconds
- Tokens consumed: 231,900
- LLM cost: $0.072

This experiment demonstrated that PentAGI could complete the assigned objective efficiently under the conditions tested.

### Experiment 2: DEV — Vague Prompt

For the initial DEV experiment, ChatGPT helped develop a deliberately broad and minimally guided mission prompt.

The purpose was to evaluate whether PentAGI could independently:

- Discover the attack surface.
- Identify potential vulnerabilities.
- Select investigation strategies.
- Research exploitation possibilities.
- Adapt to unsuccessful attempts.
- Progress toward the intrusion objective.

Unlike the Blue experiment, the DEV prompt intentionally avoided providing detailed information about known vulnerabilities or exploitation paths.

**Result:**

PentAGI independently discovered several security weaknesses, including:

- BoltWire CMS Local File Inclusion.
- Exposed application files.
- Administrative credential disclosure.
- Successful BoltWire administrative authentication.
- NFS export misconfigurations.
- Apache and Bolt CMS configuration issues.

However, the agent did not demonstrate completion of the full intrusion objective.

The initial experiment consumed more than 11 million tokens.

The execution exhibited repeated investigations, extensive subtask generation, and unsupported assumptions about achieved execution privileges.

The experiment was ultimately unsuccessful in achieving its full objective within the available resource budget.

### Experiment 3: DEV — Detailed and Focused Prompt

Following the excessive resource consumption of the initial DEV experiment, ChatGPT was used to develop a more focused mission prompt.

The revised prompt prioritized three services:

1. Port 80 — Bolt CMS.
2. Port 8080 — BoltWire CMS.
3. Port 2049 — NFS.

Additional instructions were introduced to:

- Avoid unnecessary reconnaissance.
- Reuse previously established findings.
- Prioritize evidence-supported vulnerabilities.
- Reduce repeated research.
- Limit unsuccessful attempts.
- Minimize unnecessary agent delegation.
- Produce concise evidence-based reports.

**Result:**

PentAGI successfully verified the target services and identified additional configuration information.

However, the agent continued generating multiple subtasks and consuming substantial tokens.

The researcher manually stopped the experiment because of excessive resource consumption.

The focused prompt did not produce a completed assessment before termination.

## 2.6 Research Documentation and Analysis

ChatGPT was also used to assist with preparing the final research documentation.

This included:

- Organizing experimental findings.
- Developing performance comparison tables.
- Interpreting token consumption.
- Comparing Blue and DEV outcomes.
- Identifying limitations in autonomous reasoning.
- Separating verified findings from unsupported claims.
- Preparing Markdown reports.

The researcher reviewed the generated material and supplied experimental observations and measurements.

ChatGPT served as a documentation and analysis assistant rather than an autonomous penetration-testing agent.

---

# 3. PentAGI Autonomous AI Usage

## 3.1 Autonomous Agent Framework

- **Framework:** PentAGI
- **AI Provider:** Gemini
- **Models Used:**
  - gemini-3.5-flash-lite
  - gemini-3.1-flash-lite

The exact assignment of these models to individual experiments or PentAGI roles was not documented.

PentAGI was responsible for performing autonomous security assessment activities against the authorized laboratory targets.

These activities included reconnaissance, service enumeration, vulnerability research, tool execution, result interpretation, and reporting.

## 3.2 Blue Experiment

### Target

- Target name: Blue
- Target IP: 192.168.14.129
- Operating system: Windows
- Environment: VMware isolated laboratory

### AI Execution Metrics

| Metric | Result |
|---|---|
| Outcome | Successful |
| Highest reported privilege | Root/system-level access |
| Tasks | 1 |
| Subtasks | 4 |
| Assistants | 0 |
| Tool calls | 19 |
| Duration | 6m 37s |
| Tokens | 231.9K |
| LLM Cost | $0.072 |

The researcher reported successful completion of the intrusion objective.

The detailed technical evidence supporting the final privilege level was not included in the available research records.

## 3.3 DEV Experiment — Initial Assessment

### Target

- Target name: DEV
- Target IP: 192.168.14.131
- Operating system: Debian GNU/Linux 10
- Environment: VMware isolated laboratory

### AI Execution

The initial DEV assessment used a broad mission prompt intended to maximize independent investigation.

PentAGI identified multiple vulnerabilities and configuration weaknesses.

Important findings included:

- BoltWire CMS credential exposure.
- Successful administrative authentication.
- Local File Inclusion.
- System file disclosure.
- NFS export misconfiguration.
- Bolt CMS installation and configuration issues.

### Resource Consumption

The initial DEV experiment consumed more than 11 million tokens.

The agent generated numerous subtasks and repeatedly investigated previously identified weaknesses.

Despite substantial progress in vulnerability discovery, the full intrusion objective was not demonstrated.

### Observed Limitations

- Excessive token consumption.
- Repeated reconnaissance and vulnerability analysis.
- Large numbers of delegated subtasks.
- Weak prioritization of investigation paths.
- Unsupported claims about operating-system execution.
- Failure to demonstrate privilege escalation or persistence.

## 3.4 DEV Experiment — Focused Assessment

A second DEV assessment was conducted using a more detailed and focused mission prompt.

The assessment prioritized:

1. Bolt CMS on port 80.
2. BoltWire CMS on port 8080.
3. NFS on port 2049.

PentAGI completed initial service verification and confirmed:

- Apache 2.4.38 on port 80.
- Bolt CMS installation errors.
- PHP information exposure on port 8080.
- BoltWire CMS availability.
- NFS services and broadly configured exports.

However, the agent continued creating multiple investigation and validation subtasks.

The experiment was manually terminated due to excessive token consumption.

Exact token consumption and duration for this second DEV attempt were not recorded in the available results.

## 3.5 Comparison of Autonomous AI Performance

| Metric | Blue | DEV |
|---|---|---|
| Prompt strategy | Well-defined | Vague, followed by focused |
| Overall outcome | Successful | Incomplete |
| Vulnerability discovery | Successful assessment reported | Multiple findings |
| Highest reported access | Root/system-level | Application administrator |
| OS-level access | Reported achieved | Not verified |
| Persistence | Not documented | Not demonstrated |
| Token consumption | 231.9K | Over 11M in initial run |
| Task efficiency | Relatively high | Low |
| Human intervention | Limited | Manual termination and prompt revision |

The initial DEV experiment consumed more than 47 times the tokens recorded for Blue.

However, the experiments differed in operating system, available vulnerabilities, mission instructions, and assessment conditions.

Therefore, the difference in resource consumption cannot be attributed solely to prompt design.

---

# 4. Overall Observations

The experiments demonstrated that autonomous penetration-testing agents can independently perform reconnaissance, identify vulnerabilities, and validate certain security weaknesses.

However, autonomous performance varied considerably between targets and prompt strategies.

The Blue experiment was completed successfully with relatively low resource consumption.

The DEV experiments produced meaningful vulnerability findings but did not complete the full objective and consumed substantially more resources.

The results suggest that autonomous agents may benefit from:

- Clearly defined objectives.
- Relevant initial context.
- Explicit stopping conditions.
- Resource budgets.
- Evidence-based validation.
- Human oversight for critical decisions.

The research also demonstrated the usefulness of a separate conversational AI assistant for configuring the environment, troubleshooting technical issues, interpreting execution traces, and documenting results.

## 5. Conclusion

Two different AI roles were evaluated during this research.

**ChatGPT (GPT-6)** supported the researcher through technical guidance, troubleshooting, prompt engineering, interpretation of results, and documentation.

**PentAGI (Gemini-powered)** conducted the autonomous penetration-testing experiments.

The successful Blue experiment demonstrated the potential of autonomous security assessment under a well-defined mission.

The unsuccessful DEV experiments highlighted limitations involving resource consumption, repeated investigation, evidence verification, and autonomous task management.

Overall, the findings suggest that autonomous cybersecurity agents can perform meaningful security assessment activities but may still require human guidance and effective resource controls to complete complex objectives efficiently.