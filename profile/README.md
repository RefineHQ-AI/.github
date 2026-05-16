# Maida.AI

**Don't let broken agent changes merge.**

Maida is the pre-merge behavioral regression gate for AI agents. It records
agent runs, compares current behavior against a known-good baseline, and fails
CI when structural behavior regresses: more steps, unexpected tool calls, loops,
latency spikes, or cost blowups.

Maida sits earlier than production tools and complements output evals. Output
evals ask whether the answer was good. Maida asks whether this PR changed how
the agent behaves.

No cloud account required. No telemetry by default. Runs stay in your
environment unless you explicitly export them.

---

## Repositories

| Repo | What it is |
|------|------------|
| [maida](https://github.com/maida-ai/maida) | Python package, `maida` CLI, SDK, local run storage, and timeline viewer |
| [maida-assert](https://github.com/maida-ai/maida-assert) | GitHub Action for running Maida behavioral checks on pull requests |
| [maida-tutorials](https://github.com/maida-ai/maida-tutorials) | Example workflows and walkthroughs for trying Maida on agent code |
| [maida-ts](https://github.com/maida-ai/maida-ts) | TypeScript compatibility layer for integrations and plugin authors |
| [opencode-plugin](https://github.com/maida-ai/opencode-plugin) | Maida plugin for OpenCode |

---

## Quick Start

Install the `maida-ai` package from PyPI. It provides the `maida` CLI and the
`maida` Python module.

```bash
pip install maida-ai
```

Instrument one agent entrypoint:

```python
from maida import trace

@trace
def run_agent(input: str):
    # Your existing agent code
    ...
```

Capture a known-good baseline:

```bash
python my_agent.py
maida list --json
maida baseline <RUN_ID> --out baselines/my_agent.json
```

Assert a future run against that baseline:

```bash
python my_agent.py
maida assert <RUN_ID> --baseline baselines/my_agent.json --policy .maida/policy.yaml --format markdown
```

---

## GitHub Action

Use `maida-ai/maida-assert@v2` to block behavioral regressions before merge.

```yaml
name: Agent Regression Check

on: [pull_request]

jobs:
  agent-check:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
      - uses: maida-ai/maida-assert@v2
        with:
          agent-script: my_agent.py
          baseline: baselines/my_agent.json
          policy: .maida/policy.yaml
          python-version: '3.12'
```

Your `agent-script` must use `@trace` or `traced_run()` so Maida can record a
run.

---

## What Maida Flags

- Step-count regressions
- Tool-call count regressions
- Unexpected tool paths
- Loop risk
- Guardrail events
- Latency envelope changes
- Cost envelope changes
- Missing stop conditions

These are behavioral regression signals. They do not prove output quality. They
tell you when an agent's execution behavior changed relative to a baseline.

---

Website: [maida.ai](https://maida.ai)
PyPI: [`maida-ai`](https://pypi.org/project/maida-ai/)
Contact: [contact@maida.ai](mailto:contact@maida.ai)
