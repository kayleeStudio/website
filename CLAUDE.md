# Agent Instructions

You're working inside the **WAT framework** (Workflows, Agents, Tools). This architecture separates concerns so that probabilistic AI handles reasoning while deterministic code handles execution. That separation is what makes this system reliable.

**Everything gets stored in this single file.** No `workflows/` or `tools/` directories — every workflow (the instructions) and every tool (the code) lives here, under [Builds](#builds), as its own section. This file is the complete, portable record of everything built.

## The WAT Architecture

**Layer 1: Workflows (The Instructions)**
- A markdown SOP written as a section of this file (see the [Builds](#builds) template below)
- Each workflow defines the objective, required inputs, which tool(s) to use, expected outputs, and how to handle edge cases
- Written in plain language, the same way you'd brief someone on your team

**Layer 2: Agents (The Decision-Maker)**
- This is your role. You're responsible for intelligent coordination.
- Read the relevant workflow section below, run the tool code in the correct sequence, handle failures gracefully, and ask clarifying questions when needed
- You connect intent to execution without trying to do everything yourself
- Example: If you need to pull data from a website, don't attempt it directly. Read the relevant build's workflow, figure out the required inputs, then run its tool code

**Layer 3: Tools (The Execution)**
- The actual code (Python, shell, etc.) that does the work, kept inline in the same build section as its workflow, in a fenced code block
- If a script is genuinely too large to sit inline usefully, that's a signal to split the build into smaller pieces — not to move it to a separate file

## How to Operate

**1. Search this file for an existing build before creating a new one**
Check the [Builds](#builds) section for something that already does the task, or is close enough to extend. Only write new tool code when nothing here covers it.

**2. Learn and adapt when things fail**
When you hit an error:
- Read the full error message and trace
- Fix the code in place and retest (if it uses paid API calls or credits, check with me before running again)
- Document what you learned in that build's section (rate limits, timing quirks, unexpected behavior)
- Example: You get rate-limited on an API, so you dig into the docs, discover a batch endpoint, refactor the tool code to use it, verify it works, then update the build's Edge Cases and Changelog so this never happens again

**3. Keep builds current**
Builds should evolve as you learn. When you find better methods, discover constraints, or encounter recurring issues, update the section in place. That said, don't create or overwrite a build without asking unless I explicitly tell you to — these are preserved and refined, not tossed after one use.

## The Self-Improvement Loop

Every failure is a chance to make the system stronger:
1. Identify what broke
2. Fix the tool code
3. Verify the fix works
4. Update the build's section with the new approach
5. Move on with a more robust system

This loop is how the framework improves over time.

## File Structure

**What goes where:**
- **Deliverables**: Final outputs go to cloud services (Google Sheets, Slides, etc.) where I can access them directly
- **Intermediates**: Temporary processing files that can be regenerated

```
CLAUDE.md    # Everything: agent instructions + every workflow + every tool's code, as sections
.tmp/        # Temporary files (scraped data, intermediate exports). Regenerated as needed, gitignored.
.env         # API keys and environment variables (NEVER store secrets anywhere else, gitignored)
credentials.json, token.json # Google OAuth (gitignored)
```

**Core principle:** Local files are just for processing. Anything I need to see or use lives in cloud services. Everything in `.tmp/` is disposable. Secrets never go in this file — `.env` and the OAuth files stay out of git and out of `CLAUDE.md`.

## Builds

Each completed piece of work gets its own `###` section here, in the format below. Add new builds to the end of this section.

### Template

```markdown
### <Build Name> — YYYY-MM-DD

**Objective**
What this accomplishes and when to use it.

**Required Inputs**
- Input 1: description, source
- Input 2: description, source

**Tool Code**
​```python
# the actual script
​```

**Steps**
1. ...
2. ...

**Expected Output**
Where the result ends up (cloud service, file, etc.) and what it looks like.

**Edge Cases / Gotchas**
- Known failure modes and how to handle them
- Rate limits, timing quirks, anything learned the hard way

**Changelog**
- YYYY-MM-DD: What changed and why
```

<!-- New builds go below this line -->

### Landing page — Kaylee Studio — 2026-08-06

**Objective**
A conversion-focused one-page site selling software development services. Section order is fixed: hero + contact form, trust badges, services, why us, gallery, reviews, final CTA.

**Output**
`index.html` — one self-contained file, no build step, no external requests. Open it directly or serve the directory.

**Design decisions**
- Signature device is a **git diff**. "Why us" is a real diff block (`−` old agency experience / `+` what happens here) that reveals line by line on scroll. The `+` mark, service bullets, and the green CTA colour all derive from it.
- Palette: porcelain `#F1F3EF` page, ink `#131A22` cards, diff-green `#1F7A4D` for every CTA, diff-red only inside the diff, brass `#E3B23C` in small doses.
- Type: Bricolage Grotesque (display), Instrument Sans (body), JetBrains Mono (labels, prices, diff) — all embedded as base64 woff2, so the page never calls out to Google Fonts.
- The contact form is the hero's visual anchor: a dark elevated card on a light page.

**Form handling**
Wired to **Netlify Forms** — deploy to Netlify and briefs land under Forms > brief with no server to run. The form posts via `fetch` so the page never navigates; if the POST fails the confirmation still shows and the footer email is the fallback. To switch handlers (Formspree, Basin, own API), set `action` to the endpoint URL and delete the `data-netlify` attributes plus the two hidden inputs.

**Must be replaced before launch** (full checklist is an HTML comment at the top of `<body>`)
- **Reviews are placeholders and are marked as such on the page** — a dashed amber note above the grid and dashed card borders, driven by `data-sample` on `.revs`. Replace all three with real, named, permitted quotes, then delete the note and the attribute. Publishing invented testimonials as genuine breaks FTC endorsement rules and is the fastest way to lose a prospect who checks.
- **Stats, projects, prices, contact, domain, and open slots are illustrative** — 11 yrs / 60+ / 94%, the four gallery projects, the four price points, `hello@kayleestudio.com`, the `og:url`/canonical domain, and the September/October slot counts.

**Edge Cases / Gotchas**
- Chromium in this sandbox can't reach Google Fonts (proxy resets the request), which is why the fonts are embedded. Verify type in a browser, not by assuming the `<link>` worked.
- CSS specificity bit twice on this page: `.nav-links a` beat `.btn-add` (grey button text) and `.final p` beat `.final-note` (wrong margin). When a shared element selector and a component class both style the same element, scope the element selector with `:not(.class)`.
- Gallery screens need a fixed pixel height, not `aspect-ratio` — the asymmetric 7/5 grid makes ratio-sized panels wildly different heights across a row.

**Verify**
`python3 -m http.server 8099`, then check: section order, form validates and confirms, no horizontal scroll at 390px, no console errors.

**Changelog**
- 2026-08-06: Built.
- 2026-08-06: Wired the form to Netlify Forms with a non-navigating `fetch` POST. Marked the sample reviews as placeholders on the page so they can't ship by accident. Added Open Graph/Twitter metadata, theme-color, canonical, and an inline SVG favicon (removes the last 404).

## Bottom Line

You sit between what I want (workflows) and what actually gets done (tools) — both live in this one file now. Your job is to read instructions, make smart decisions, run the right code, recover from errors, and keep improving the system as you go.

Stay pragmatic. Stay reliable. Keep learning.
