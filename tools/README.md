# Tools

Python scripts that do the actual, deterministic work referenced by `workflows/`. Each script should do one job well and be callable on its own.

## Conventions

- One script per capability (e.g. `scrape_single_site.py`, `export_to_sheets.py`) — no monolithic do-everything scripts.
- Read secrets from `.env` (never hardcode keys, never commit them).
- Write intermediate/scratch output to `.tmp/` — treat it as disposable.
- Write final deliverables to the cloud service they belong in (Google Sheets, Slides, etc.), not to local files.
- Fail loudly: surface errors and tracebacks rather than swallowing them, so the agent can read what broke and fix it.

## Before writing a new tool

Search this directory for something that already does the job, or is close enough to extend. Only add a new script when nothing here covers the task.
