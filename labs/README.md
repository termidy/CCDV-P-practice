# Labs

Hands-on labs: a way to practise without an API key.

**You do not need an API key.** Each lab ships with `shopassist_lab.py`, a small
standard-library-only simulator of the `anthropic` SDK. The code you write is exactly the
code you would write against the real API; the replies come from rules, not a real model.
Set `ANTHROPIC_API_KEY` in your environment (or in a `.env` file) and the very same notebook
calls Claude for real.

## How to run a lab

```bash
cd labs/lab_api_errors
jupyter lab            # or open notebook.ipynb in VS Code
```

Open **`notebook.ipynb`** and work through it top to bottom. Every cell already holds the
code, with blanks marked `...` and a comment saying what goes in each one. Each task ends
with a `check(...)` call that prints PASS with an explanation or FAIL with a specific hint.
`START_HERE.md` in the lab folder has the details; `solution.ipynb` is the completed
notebook if you get stuck.

The only requirement is Python 3.10+ and Jupyter (`pip install jupyter`). The other packages
the lectures use are optional - `anthropic`, `python-dotenv`, `fastapi`, `pydantic`, `mcp`
and `claude-agent-sdk`: when one is missing, the simulator stands in for it. An installed
`anthropic` is used only when `ANTHROPIC_API_KEY` is set; without a key the simulator
answers, so no lab fails with an authentication error. The labs that use Claude Code or the
Agent SDK play the agent offline, and run your hooks for real.

## The labs

In course order, one for each practice lecture that has a lab. Each lab mirrors a script in
[`shopassist/`](../shopassist/).

| Lab | Practises | Script |
| --- | --- | --- |
| `lab_supervisor` | A coordinator and subagents: tool scope enforced, facts as data, tokens per agent | `coordinator.py` |
| `lab_api_errors` | The two error families, 401 / 400 / timeout on purpose, retrying only what can change | `errors.py` |
| `lab_regression_gate` | A defensive verdict parser, results per case, a gate that blocks any case that gets worse | `evaluate.py` |
| `lab_prompt_caching` | `cache_control` on the policy, usage priced in code, a changing value that breaks the cache | `caching.py` |
| `lab_injection_defense` | An injected order note: the prompt as layer one, a refund gate in code as layer two | `injection.py` |
| `lab_redact_pii` | Redact personal data before the request, fail closed at the boundary, keep keys out of logs | `redact.py` |
| `lab_claude_code_hook` | A Claude Code hook script that exits with 2, registered in settings.json, tested headless | `.claude/hooks/guard.py` |

## The simulator follows the current API

The labs use `claude-sonnet-5-5` and `output_config={"effort": "low"}`, as the lectures do.
Like the real API, the simulator rejects `temperature`, `top_p`, `top_k`, an assistant
prefill and a forced `tool_choice` on current Opus, Sonnet and Fable models; enforces the
tool protocol (every `tool_use` needs its `tool_result`); checks image blocks; caches
prompts up to `cache_control`; and raises the SDK's error classes. A mistake you make
offline gives the same error the API would.
