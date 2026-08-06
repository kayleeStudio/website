# Workflows

Each file in this directory is a markdown SOP (standard operating procedure) for one recurring task. Workflows are the instructions layer of the WAT framework — plain-language briefs the agent reads before acting.

## Template

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

## Rules

- Don't create or overwrite a workflow without asking first, unless explicitly told to.
- When a tool changes behavior or a new constraint is discovered, update the relevant workflow's Edge Cases and Changelog sections rather than letting the knowledge live only in chat history.
