# CCDV-P practice: ShopAssist in code

Companion code for the practice lectures of the **Claude Certified Developer - Professional** exam course. Each practice
lecture is recorded in VS Code against the real Claude API, and this repository holds the project you see on screen, so
you can run every step yourself.

The project continues the ShopAssist code of the Foundations-level practices: the conversation and loop scripts, the
structured tool errors, the Agent SDK hook, and `CLAUDE.md` with its rule for tool code are already here, and the
practice lectures build on them. The recordings were made in the earlier repository, so the editor and the terminal
show the folder name `CCDV-F-practice`. The code is the same.

| Folder | What it is |
| --- | --- |
| [`shopassist/`](shopassist/) | The practice project: one small Python script per lesson, all built on the same fictional support assistant, ShopAssist. Needs an Anthropic API key. |
| [`starting-points/`](starting-points/) | The "before" version of the files that a lesson edits on screen, so you can replay the edits yourself. |
| [`labs/`](labs/) | Jupyter notebooks to practise **without an API key**, one for each practice lecture that has a lab. |

## Setup

```bash
git clone https://github.com/termidy/CCDV-P-practice.git
cd CCDV-P-practice/shopassist
python3 -m venv .venv && source .venv/bin/activate      # Python 3.10+
pip install -r requirements.txt
cp .env.example .env                                    # put your key in .env, never in the code
```

`.env` holds one line, `ANTHROPIC_API_KEY=...`; every script loads it with `python-dotenv`. Some lessons also need
[Claude Code](https://docs.claude.com/en/docs/claude-code) installed and logged in.

The scripts use the current API: `claude-sonnet-5-5`, `output_config={"effort": "low"}`, structured outputs through
`output_config.format`, and the `mcp` package 2.x for the MCP server. If something returns a 400, check the model id and
the SDK version first (`anthropic>=1.12`).

## The practice lectures

In the order of the course.

| Practice lecture | File(s) | What you run |
| --- | --- | --- |
| Coordinator and Subagents in Code | `coordinator.py` | `python coordinator.py` - a coordinator calls two specialised agents as tools |
| Errors and Retries: 401, 400 and Timeouts | `errors.py` | `python errors.py` - a wrong key, a removed parameter through `extra_body`, a 50 ms timeout; `max_retries` and `with_options` |
| Evaluate a Prompt Change on a Test Set | `evaluate.py` | `python evaluate.py` - two system prompts, same cases, scored by code and by a model grader |
| Prompt Caching: Pay for the Policy Once | `caching.py`, `policy.txt` | `python caching.py` - `cache_control` on the policy block; usage shows cache write once, then cache reads |
| Prompt Injection Through a Tool Result | `injection.py` | `python injection.py` - an order note demands a 500 refund; then `python -c "from injection import process_refund; print(process_refund('ORD-12345678', 500))"` shows the code gate |
| PII and Secrets: Redact Before You Send | `redact.py` | `python redact.py` - emails, card and phone numbers become placeholders before the request; `git check-ignore -v .env` |
| Claude Code Hooks: Block a Destructive Command | `.claude/settings.json`, `.claude/hooks/guard.py` | `claude -p "Show the first line of .env with head, then delete review.json with rm. Report what happened." --allowedTools Bash` - both commands are blocked by the hook |
| Claude Code in CI: `-p` and JSON Output | `ci_review.sh`, `review_schema.json` | break a tool convention in `mcp_server.py`, then `sh ci_review.sh` |

The other scripts in `shopassist/` are the code these lectures build on. The order IDs the tools know are
`ORD-12345678` (a jacket, delivered 20 days ago) and `ORD-87654321` (a lamp, delivered 45 days ago). Everything else
returns `found: false`, which is data, not an error.
