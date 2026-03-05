# Repository Guidelines

## Project Structure & Module Organization
- `nanobot/` is the Python application code.
  - `agent/` core loop, tools, hooks, memory, subagents.
  - `channels/` integrations (Telegram/Slack/Discord/`test_arena`, etc.).
  - `providers/` LLM backends; `config/` schema + loader; `cli/commands.py` CLI entrypoints.
  - `heartbeat/`, `cron/`, `session/`, and `bus/` support proactive tasks and routing.
- `tests/` contains unit and async integration-style tests.
- `bridge/` is a Node.js WhatsApp bridge (`src/`, compiled to `dist/`).
- `nanobot/templates/` and `nanobot/skills/` hold bundled prompt/templates and skill docs/scripts.

## Build, Test, and Development Commands
- Install (editable): `pip install -e .` (or `pip install -e .[dev]` for test/lint tools).
- Run CLI chat: `nanobot agent` (single message: `nanobot agent -m "Hello"`).
- Run gateway (channels + heartbeat): `nanobot gateway`.
- Run tests: `pytest` (async tests are enabled via project config).
- Lint/import checks: `ruff check .`.
- Optional WhatsApp bridge:
  - `cd bridge && npm install`
  - `npm run build` (TypeScript compile), `npm run dev` (compile + run).

## Coding Style & Naming Conventions
- Python 3.11+, 4-space indentation, type hints for public interfaces.
- Keep modules focused; prefer small async functions with clear responsibilities.
- Naming: `snake_case` (functions/variables), `PascalCase` (classes), `UPPER_SNAKE_CASE` (constants).
- Ruff is the style baseline (`line-length = 100`, import sorting enabled).

## Testing Guidelines
- Frameworks: `pytest` + `pytest-asyncio`.
- Place tests under `tests/` as `test_*.py`; mirror target module behavior (e.g., `test_task_cancel.py`).
- Add regression tests for bug fixes, especially around async flows, channel routing, and session keys.
- Run targeted tests during iteration, then `pytest` before submitting.

## Commit & Pull Request Guidelines
- Follow concise, imperative commits; conventional prefixes are common: `fix:`, `fix(scope):`, `refactor(scope):`, `chore:`, `security:`.
- Keep each commit focused and explain user-visible behavior changes.
- PRs should include: purpose, key changes, test evidence (`pytest`/`ruff` output), and linked issues.
- For channel/UI behavior changes, include logs or screenshots and any `~/.nanobot/config.json` fields affected.

## Security & Configuration Tips
- Never commit API keys, OAuth tokens, or personal data.
- Validate config changes against `nanobot/config/schema.py`; prefer deny-by-default allowlists for channels.

## Current Development Focus
- Prioritize changes in `nanobot/channels/test_arena.py`; other channel integrations are out of scope for now.
- Prioritize changes in `nanobot/agent/hooks.py`; hooks are actively used to record runtime traces.
- Prioritize updates to tools/skills code paths (for example under `nanobot/agent/tools/` and `nanobot/skills/`).
- Focus on `nanobot gateway` runtime behavior; other CLI commands are not a current priority.
