# AgentDbg

**Local-first debugger for AI agents. (v0.2.4)**

When your agent does something unexpected -- calls the wrong tool, loops on the same step, burns through API budget -- you need a timeline, not a dashboard. AgentDbg captures a structured, chronological record of a single agent run: LLM calls, tool calls, state updates, errors, and loop warnings. Then it shows you that timeline in a clean local UI so you can understand what actually happened and fix it.

No accounts. No cloud. No telemetry. Traces live on your machine.

---

## Repositories

| Repo | What it is |
|------|------------|
| [agentdbg](https://github.com/AgentDbg/agentdbg) | The debugger -- core library, CLI, UI |
| [agentdbg-ts](https://github.com/AgentDbg/agentdbg-ts) | TypeScript compatibility layer for plugin authors |
| [opencode-plugin](https://github.com/AgentDbg/opencode-plugin) | AgentDbg plugin for OpenCode |
| [tutorials](https://github.com/AgentDbg/tutorials) | Jupyter notebook walkthroughs (LangChain, OpenAI Agents SDK, runaway loop debugging) |

---

## Quick start

```bash
pip install agentdbg
```

```python
from agentdbg import trace

@trace
def run_my_agent():
    # your agent code here
    ...

run_my_agent()
```

```bash
agentdbg view
```

That's it. A timeline of every LLM call, tool call, and error -- locally, in your browser, in under 10 minutes.

---

## Works with

- **LangChain / LangGraph** -- drop-in callback handler
- **OpenAI Agents SDK** -- tracing processor adapter
- **CrewAI** -- execution hook integration
- **Any Python agent** -- manual `record_llm_call` / `record_tool_call` API

All integrations are optional. Import to enable. No lock-in.

---

## Stop runaway loops

```python
@trace(stop_on_loop=True, max_llm_calls=50)
def run_my_agent():
    ...
```

AgentDbg detects repeated call patterns and -- if you opt in -- aborts before they drain your budget. The abort is recorded in the timeline so you know exactly where and why it stopped.

---

## Design principles

- **Debugger, not observability.** This is for development-time clarity, not production monitoring.
- **Local-first.** Traces are plain files on disk (`run.json` + `events.jsonl`). No accounts, no setup, no data leaving your machine.
- **Framework-agnostic core.** The core library has no framework dependencies. Integrations are thin, optional adapters.
- **Fixed event taxonomy.** Seven event types, clearly defined. No surprise schema drift.

---

PyPI: [`agentdbg`](https://pypi.org/project/agentdbg/) -- Issues and PRs welcome.
