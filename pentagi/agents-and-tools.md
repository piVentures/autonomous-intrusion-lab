# PentAGI Architecture: Agents, Tools, and Safeguards

## 1. The Agent Roster (A Virtual Red Team)
PentAGI is not a single chatbot; it is a multi-agent system where a Primary Agent orchestrates a team of specialists [citation:2][citation:7].

| Agent Type | Role in the Lab |
| :--- | :--- |
| **Primary Agent** | The Orchestrator. It received your objective, planned the attack chain, and delegated tasks to the specialists. |
| **Pentester** | The Executor. This agent ran the actual `nmap`, `curl`, `sqlmap`, and Metasploit commands inside the sandbox. It handled the tactical exploitation. |
| **Searcher** | The Researcher. When the agent needed to know the crAPI API routes, the Searcher queried the web (or parsed the JS bundle) to find intelligence. |
| **Coder** | The Developer. When the Pentester needed a custom Python socket script to connect to the Metasploitable2 bindshell, the Coder wrote and executed it. |
| **Installer** | The Mechanic. Ensured tools like `nmap` or `metasploit` were present in the Kali sandbox. |
| **Adviser** | The Mentor. If the Primary Agent got stuck, the Adviser provided strategic guidance (especially when execution monitoring was enabled). |
| **Reflector** | The Disciplinarian. If an agent started generating prose instead of tool calls, the Reflector forced it back to action [citation:7]. |
| **Memorist** | The Archivist. Queried the pgvector memory to recall what worked in previous steps (e.g., "Did we already try port 80?"). |

## 2. The Tools (The Execution Layer)
Agents act through tools. In PentAGI, the most important tools for the trials were:
- **Terminal:** The ability to run arbitrary shell commands (`nmap`, `curl`, `msfconsole`, `python3`) inside the isolated Docker sandbox [citation:2].
- **Browser:** An isolated Chromium instance for web scraping and reconnaissance 
- **Web Search:** Integrated search engines (DuckDuckGo, Tavily, etc.) for intelligence gathering [citation:2].

## 3. Safeguards (Why It Doesn't Run Forever)
PentAGI implements strict guards to prevent the agent from burning tokens indefinitely [citation:7]:
- **Tool Call Limits:** The Primary Agent is capped at 100 model turns. Limited agents (like Searcher) are capped at 20.
- **Repeat Detection:** If an agent calls the same tool with the same arguments 3 times in a row, it is blocked. At 7 times, the chain is terminated.
- **Reflector:** Intervenes if the model stops producing tool calls.

## 4. Memory (Context Management)
PentAGI uses **Chain Summarization** to manage the growing context window . Because the agent sends the full conversation history on every turn, the context can explode. The summarizer compresses older messages while preserving critical tool call results. This is why the 3.6M token Mr. Robot trial did not immediately hit a context limit, but it is also why the cost scales exponentially.
