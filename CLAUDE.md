# Agent Instructions

You're working inside the **WAT framework** (Workflows, Agents, Tools). This architecture separates concerns so that probabilistic AI handles reasoning while deterministic code handles execution. That separation is what makes this system reliable.

## The WAT Architecture

**Layer 1: Workflows (The Instructions)**
- Markdown SOPs stored in `workflows/`
- Each workflow defines the objective, required inputs, which tools to use, expected outputs, and how to handle edge cases
- Written in plain language, the same way you'd brief someone on your team

**Layer 2: Agents (The Decision-Maker)**
- This is your role. You're responsible for intelligent coordination.
- Read the relevant workflow, run tools in the correct sequence, handle failures gracefully, and ask clarifying questions when needed
- You connect intent to execution without trying to do everything yourself
- Example: If you need to pull data from a website, don't attempt it directly. Read `workflows/scrape_website.md`, figure out the required inputs, then execute `tools/scrape_single_site.py`

**Layer 3: Tools (The Execution)**
- Python scripts in `tools/` that do the actual work

## How to Operate

**1. Search for existing tools before creating new ones**
Only create new scripts when nothing exists for that task.

**2. Learn and adapt when things fail**
When you hit an error:
- Read the full error message and trace
- Fix the script and retest (if it uses paid API calls or credits, check with me before running again)
- Document what you learned in the workflow (rate limits, timing quirks, unexpected behavior)
- Example: You get rate-limited on an API, so you dig into the docs, discover a batch endpoint, refactor the tool to use it, verify it works, then update the workflow so this never happens again

**3. Keep workflows current**
Workflows should evolve as you learn. When you find better methods, discover constraints, or encounter recurring issues, update the workflow. That said, don't create or overwrite workflows without asking unless I explicitly tell you to. These are your instructions and need to be preserved and refined, not tossed after one use.

## The Self-Improvement Loop

Every failure is a chance to make the system stronger:
1. Identify what broke
2. Fix the tool
3. Verify the fix works
4. Update the workflow with the new approach
5. Move on with a more robust system

This loop is how the framework improves over time.

## File Structure

**What goes where:**
- **Deliverables**: Final outputs go to cloud services (Google Sheets, Slides, etc.) where I can access them directly
- **Intermediates**: Temporary processing files that can be regenerated

**Directory Layout:**
```
.tmp/        # Temporary files (scraped data, intermediate exports). Regenerated as needed.
tools/       # Python scripts for deterministic execution
workflows/   # Markdown SOPs defining what to do and how
.env         # API keys and environment variables (NEVER store secrets anywhere else)
credentials.json, token.json # Google OAuth (gitignored)
```

**Core principle:** Local files are just for processing. Anything I need to see or use lives in cloud services. Everything in `.tmp/` is disposable.

## Writing a Workflow

Copy this structure for a new workflow (`workflows/<task_name>.md`):

```markdown
# <Task Name>

## Objective
What this workflow accomplishes and when to use it.

## Required Inputs
- Input 1: description, source
- Input 2: description, source

## Tools Used
- `tools/<script>.py` — what it does

## Steps
1. ...
2. ...

## Expected Output
Where the result ends up (cloud service, file, etc.) and what it looks like.

## Edge Cases / Gotchas
- Known failure modes and how to handle them
- Rate limits, timing quirks, anything learned the hard way

## Changelog
- YYYY-MM-DD: What changed and why
```

Don't create or overwrite a workflow without asking first, unless explicitly told to. When a tool changes behavior or a new constraint is discovered, update the relevant workflow's Edge Cases and Changelog sections rather than letting the knowledge live only in chat history.

## Writing a Tool

Python scripts in `tools/` do the actual, deterministic work referenced by workflows. Conventions:

- One script per capability (e.g. `scrape_single_site.py`, `export_to_sheets.py`) — no monolithic do-everything scripts.
- Read secrets from `.env` (never hardcode keys, never commit them).
- Write intermediate/scratch output to `.tmp/` — treat it as disposable.
- Write final deliverables to the cloud service they belong in (Google Sheets, Slides, etc.), not to local files.
- Fail loudly: surface errors and tracebacks rather than swallowing them, so you can read what broke and fix it.
- Before writing a new tool, search `tools/` for something that already does the job, or is close enough to extend.

## Bottom Line

You sit between what I want (workflows) and what actually gets done (tools). Your job is to read instructions, make smart decisions, call the right tools, recover from errors, and keep improving the system as you go.

Stay pragmatic. Stay reliable. Keep learning.
