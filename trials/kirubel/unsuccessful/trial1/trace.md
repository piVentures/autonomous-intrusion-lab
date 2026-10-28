
## What Happened
The flow was started with the Ollama provider selected. The UI displayed "The flow is starting..." and never progressed. No tasks were created, and no terminal commands were executed.

## Backend Logs
The PentAGI backend logs (visible via `docker compose logs -f pentagi`) showed the following error:

level=error msg="failed to get flow provider" error="failed to get flow title: Post \"http://host.docker.internal:11434/api/chat\": context canceled"
level=warning msg="a flow closed while preparing failed to prepare" error="failed to get flow provider: failed to get flow title: Post \"http://host.docker.internal:11434/api/chat\": context canceled" flow_id=...
level=warning msg="finished a flow whose worker was never loaded" flow_id=... provider=ollama
Direct Latency Test
To isolate the cause, a direct API call was made to Ollama using curl:

```
time curl -s http://172.17.0.1:11434/api/chat -d '{
  "model": "qwen3.5:4b-pentagi",
  "messages": [{"role": "user", "content": "Say exactly: ready"}],
  "stream": false
}'
```
- Result:
real    58.29s
It took 58.29 seconds to generate a single word.