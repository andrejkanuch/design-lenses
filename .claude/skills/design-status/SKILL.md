---
name: design-status
description: Check progress of a running Compound Design Loop. Shows current phase, completed agents, applied fixes, and remaining work. Use anytime during or after a design loop to see where things stand.
user-invokable: true
---

Display the current status of a design loop by reading and parsing the progress tracking file.

## Find Progress File

Search for `design-review-progress.md` in the following locations, in order:

1. Current working directory
2. Project root (look for nearest `.git` directory and check that level)
3. Any subdirectory — use a glob search like `**/design-review-progress.md`

If no progress file is found, output:
> No active design loop found. Start one with `/compound-design-loop:design-loop <file>`

If multiple progress files are found, list all of them with their full paths and ask the user which one they want to inspect. Do not guess.

## Parse Progress

Read the progress file and extract structured data from two sections:

**From YAML frontmatter**, extract these fields:
- `file` — the target file being reviewed
- `domain` — project domain (e.g., motorcycle, e-commerce)
- `platform` — target platform (e.g., Mobile, Desktop, Responsive)
- `theme` — color theme (e.g., Dark, Light)
- `scope` — page scope (e.g., Page, Component, Section)
- `context` — any additional context string
- `status` — one of: `in-progress`, `complete`, `partial`
- `started` — timestamp when the loop began

**From the markdown body**, parse:
- Count all checked `[x]` checkboxes vs unchecked `[ ]` checkboxes to determine agent completion
- Determine the current iteration by finding the first unchecked agent's iteration group
- Extract the list of applied fixes (usually a numbered or bulleted list under a fixes heading)
- Extract the summary section content if the loop is complete
- Identify iteration names/phases (e.g., DIAGNOSE, FOUNDATIONS, ENHANCE, SHIP)

## Display Status

Output a formatted status report based on the parsed data. Follow this structure:

For an **in-progress** loop:

```markdown
## Design Loop Status

**File**: path/to/file.html
**Context**: Mobile | Dark theme | Page | motorcycle domain
**Status**: In Progress (Phase 2 of 4: FOUNDATIONS)
**Started**: 2026-03-17 10:00

### Progress
████████░░░░░░░░ 4/8 agents complete (50%)

### Completed
- [x] Iteration 1 — DIAGNOSE
  - Design Critic: 8 issues found, 6 applied
  - Domain Expert: 5 issues found, 4 applied

### In Progress
- [ ] Iteration 2 — FOUNDATIONS
  - Design System Agent: running...
  - Copy & Clarity Agent: pending

### Remaining
- [ ] Iteration 3 — ENHANCE
- [ ] Iteration 4 — SHIP

### Recent Fixes Applied
1. Contrast ratio bumped to WCAG AA (consensus)
2. Focus-visible states added globally (critic, critical)
3. ...

### Estimate
~2 iterations remaining
```

Build the progress bar using block characters:
- Use `█` for completed portions and `░` for remaining portions
- Scale to 16 characters total width
- Show the fraction and percentage beside it

For the Completed section, list each finished iteration and its agents with a brief summary of what each agent found and how many fixes were applied.

For the In Progress section, mark the currently running agent as "running..." and subsequent agents in the same iteration as "pending".

For the Remaining section, list future iterations by name only.

For Recent Fixes Applied, show the last 5-8 fixes from the applied fixes list. Include the source agent and severity in parentheses when available.

For the Estimate, calculate remaining iterations based on current position.

## Completion Status

If the `status` field is `complete`, output:

```markdown
## Design Loop — Complete

**File**: path/to/file.html
**Duration**: 12 minutes (4 iterations)
**Agents**: 8/8 completed
**Fixes Applied**: 47 total

### Summary
[contents of Summary section from progress file]
```

Calculate duration from the `started` timestamp to the current time or to a completion timestamp if one is recorded. Show the total number of iterations completed. Include the full summary section text from the progress file verbatim.

If the `status` field is `partial`, output:

```markdown
## Design Loop — Partial (stopped early)

**Completed**: 3/4 iterations (6/8 agents)
**Reason**: Context exhaustion after iteration 3
**Fixes Applied**: 34 total
```

For partial completions, look for a reason field or infer the stopping point from the last checked agent. Report exactly how far the loop got before stopping.

## NEVER

- NEVER modify the progress file — this skill is read-only
- NEVER start a new design loop from this skill — just report status
- NEVER delete or rename the progress file
- NEVER write any changes back to the target file being reviewed
- NEVER guess at data that is not present in the progress file — report only what is recorded

Run `/compound-design-loop:design-loop` to start a new loop, or continue an existing one.
