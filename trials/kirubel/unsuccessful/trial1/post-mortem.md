# Post-Mortem: Why the Ollama Trial Was Unsuccessful
- Root Cause: Time-to-First-Token (TTFT) Exceeded Orchestration Timeout
PentAGI is a multi-agent framework. Before any agent can plan or execute a tool, the Primary Agent must make an initial LLM call to generate the flow title and select the Docker image. This initial call has an internal timeout (typically 30 seconds).

- Our direct test showed that the local CPU took 58.29 seconds to generate a single word. Because the initial prompt (containing the system instructions and tool definitions) is much larger, the actual TTFT was even longer. Consequently, PentAGI canceled the request and killed the flow before the model could even finish reading the prompt.

- Hardware Constraints
The trial was run on a Kali Linux machine with the following specifications:

RAM: 12 GiB total (6.3 GiB used, only 6.4 GiB available).

GPU: None. Inference was CPU-only.

Swap: 12 GiB (unused at the time of failure).

- The free -h output (captured in the screenshot) confirms that the machine was already under memory pressure from running the PentAGI Docker stack (PostgreSQL, Scraper, etc.) plus the 6.6 GiB model.

- Why the 4B Model Didn't Help
We also tested the smaller qwen3.5:4b-pentagi model (approx. 3.4 GiB). While it used less RAM, it still took nearly a minute to generate a single token. The bottleneck is not the model size; it is the CPU's memory bandwidth. Generating a token requires reading the entire model weights from RAM. On a CPU, this is orders of magnitude slower than on a GPU.

- Conclusion
Local CPU inference is physically incapable of meeting the latency requirements of a multi-agent framework like PentAGI. The failure is not a configuration error; it is a hardware limitation. Agentic AI requires either GPU acceleration or a cloud API to function in a real-time orchestration loop.
