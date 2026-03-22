# Compound Design Loop

**Build fast, then refine through specialized lenses.**

[![Version](https://img.shields.io/badge/version-1.1.0-blue.svg)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## What It Does

Compound Design Loop is a Claude Code plugin that orchestrates multi-persona design reviews. It spawns 8 specialist agents across 4 structured iterations (DIAGNOSE, FOUNDATIONS, ENHANCE, SHIP). Hand it any functional UI file and it takes it from "working" to "production-grade" -- critique, typography, layout, motion, accessibility, resilience, and polish in a single run.

Good design is not one big decision. It is 15 small ones made through different lenses.

## Install

```bash
claude plugin install compound-design-loop
```

Or test locally:

```bash
claude --plugin-dir /path/to/compound-design-loop
```

## Quick Start

```bash
/compound-design-loop:design-loop src/dashboard.tsx --domain=finance
```

Here is what happens when you run it:

1. **Context gathering** -- the plugin asks you about platform (mobile/web), theme (dark/light/both), scope (component/page), and usage context (data-heavy, outdoor, etc.). These shape every agent prompt so advice is specific, not generic.

2. **Iteration 1: DIAGNOSE** -- a Design Critic and a Domain Expert agent spawn in parallel. The critic evaluates visual hierarchy, contrast, states, and accessibility. The domain expert reviews through the lens of a real user in your domain (e.g., a day trader for finance). Both report findings without editing files.

3. **Synthesis** -- the orchestrator reads both reports, categorizes findings by consensus and severity, and applies fixes to your file using the Edit tool. Progress is logged to `design-review-progress.md`.

4. **Iterations 2-4** repeat the pattern with different specialist pairs (design system + copy, motion + resilience, polish + boldness), each building on the previous iteration's improvements.

5. **Final summary** -- total fixes applied, grouped by category, with the top 5 most impactful improvements highlighted.

A full loop typically takes 5-10 minutes depending on file size and complexity.

## Commands

### `/compound-design-loop:design-loop <file|dir> [flags]`

The full loop. 8 agents, 4 iterations, auto-applied fixes between each round.

| Iteration | Name | Agents | What They Cover |
|-----------|------|--------|-----------------|
| 1 | **DIAGNOSE** | Design Critic + Domain Expert | Visual hierarchy, accessibility audit, contrast ratios, domain-specific usability, competitor comparison |
| 2 | **FOUNDATIONS** | Design System + Copy & Clarity | Spacing grid, type scale, color tokens, layout rhythm, label clarity, empty states, error messages, tone |
| 3 | **ENHANCE** | Motion & Delight + Resilience | Purposeful animations, celebration moments, color safety, text overflow, edge cases, responsive adaptation |
| 4 | **SHIP** | Polish & Extract + Bolder/Overdrive | Pixel alignment, performance optimization, design token extraction, bold amplification, signature effects |

**Flags:**
| Flag | Description |
|------|------------|
| `--domain=X` | Sets the domain expert persona (see [Domain Presets](#domain-presets)) |
| `--iterations=N` | Run only the first N iterations (1-4). `--iterations=1` runs DIAGNOSE only. |
| `--resume` | Continue a previous run from where it left off. Reads the progress file and skips completed iterations. |
| `--no-apply` | Run all agents but output findings as a report instead of editing the file. Review before committing. |
| `--dry-run` | Gather context and output the plan without executing any agents. |

**Accepts:** single file, multiple files (space-separated), or a directory (globs for supported types).

**Supported file types:** `.html`, `.jsx`, `.tsx`, `.vue`, `.svelte`

**Multi-file mode:** When given multiple files or a directory, runs the loop on each file sequentially with shared context and cross-file design token consistency.

### `/compound-design-loop:design-brainstorm <file> [--domain=X]`

Quick 3-agent brainstorm without the full loop. Spawns a Design Critic, Domain Expert, and a generalist UX reviewer in parallel. No file editing -- returns prioritized findings for you to act on manually. Use this for early-stage feedback before committing to a full loop.

### `/compound-design-loop:design-status`

Check progress of a running design loop. Reads the `design-review-progress.md` file created by design-loop and reports which iterations have completed, which agents have run, and what fixes have been applied so far.

## How It Works

### The 4 Iterations

```
Iteration 1: DIAGNOSE     --> Design Critic + Domain Expert
                              (research-only: analyze and report)
                                        |
                              [Synthesis: categorize + apply fixes]
                                        |
Iteration 2: FOUNDATIONS   --> Design System + Copy & Clarity
                              (system agent edits, copy agent reports)
                                        |
                              [Synthesis: resolve + apply fixes]
                                        |
Iteration 3: ENHANCE      --> Motion & Delight + Resilience
                              (both may edit: add animations + safety)
                                        |
                              [Synthesis: resolve conflicts + apply]
                                        |
Iteration 4: SHIP         --> Polish & Extract + Bolder/Overdrive
                              (research-only: final recommendations)
                                        |
                              [Final synthesis: apply polish + bold]
                                        |
                              DONE --> Summary + Progress File
```

### Context Gathering

Before any agents run, the loop gathers five signals that parameterize every agent prompt. Generic agents produce generic advice -- context makes the difference.

| Signal | Options | How It Is Determined |
|--------|---------|---------------------|
| **Platform** | `mobile`, `web`, `responsive`, `cross-platform` | Auto-detected from imports (React Native, Expo, Next.js, etc.) or asked |
| **Theme** | `dark-only`, `light-only`, `both`, `system-adaptive` | Auto-detected from CSS variables / color scheme queries or asked |
| **Scope** | `component`, `page`, `multi-screen`, `app-shell` | Asked |
| **Context** | `outdoor-use`, `glove-constraints`, `data-heavy`, `content`, `conversion`, `none` | Asked |
| **Domain** | From `--domain` flag or asked | Maps to a persona in domain-experts.md |

### Auto-Apply Between Iterations

After each pair of agents completes, the orchestrator runs a synthesis protocol:

1. **Read both outputs completely** -- no skimming.
2. **Categorize findings**: CONSENSUS (both flagged it, highest priority), SINGLE-CRITICAL (one agent, apply immediately), SINGLE-MAJOR (apply with note), SINGLE-MINOR (defer or skip), CONTRADICTION (domain-specific rules break ties).
3. **Apply fixes** in logical batches using the Edit tool.
4. **Update the progress file** with what changed and why.
5. **Proceed** to the next iteration, where agents read the improved file.

This means each iteration builds on real improvements, not just theoretical suggestions.

## Domain Presets

The `--domain` flag customizes the Domain Expert agent with a specialized persona, evaluation criteria, and competitor awareness.

| Domain | Expert Persona | Key Evaluation Focus |
|--------|---------------|---------------------|
| `motorcycle` | 15-year rider, Calimoto/RISER power user | Glanceability at speed, glove-friendly tap targets (min 48px), sunlight contrast, vibration tolerance |
| `fitness` | Daily Strava user, marathon runner | Mid-workout readability, GPS accuracy display, splits/pace formatting, sweaty-finger interaction |
| `finance` | Day trader, 14-hour session veteran | Data density, real-time update handling, P&L color coding (green/red), order confirmation safety |
| `ecommerce` | Power shopper, deal hunter | Product image quality, price clarity, checkout friction reduction, trust signals, cart persistence |
| `medical` | ER nurse, high-stress clinical environment | Alert hierarchy (red/yellow/green), medication dosage safety, patient ID prominence, one-hand operation |
| `default` | Power user of the app's domain | Workflow efficiency, feature discovery, keyboard shortcuts, edge case handling |

Each domain preset includes competitor comparisons, environmental constraints, and a "switch likelihood" rating at the end of the review.

## Architecture

### File Structure

```
compound-design-loop/
|-- .claude-plugin/
|   |-- plugin.json                              Plugin metadata, version, keywords
|   |-- marketplace.json                         Marketplace listing and tags
|-- .claude/
|   |-- skills/
|       |-- design-loop/
|       |   |-- SKILL.md                         Main orchestration skill
|       |   |-- reference/
|       |       |-- agent-roles.md               8 agent definitions with checklists
|       |       |-- domain-experts.md            6 domain personas and criteria
|       |       |-- critique-framework.md        10 evaluation dimensions
|       |       |-- accessibility-checklist.md   WCAG AA requirements
|       |-- design-brainstorm/
|       |   |-- SKILL.md                         Quick 3-agent brainstorm skill
|       |-- design-status/
|           |-- SKILL.md                         Progress tracking skill
|-- examples/
|   |-- dashboard-before.html                    Sample motorcycle dashboard (test file)
|   |-- sample-progress.md                       Example completed progress file
|-- CLAUDE.md                                    Plugin development guidelines
|-- CHANGELOG.md                                 (34 lines)   Version history
|-- LICENSE                                      (21 lines)   MIT License
|-- README.md                                                 This file
```

### The 8 Agents

| # | Agent | Focus Areas | Iteration | Edits File? |
|---|-------|------------|-----------|-------------|
| 1 | Design Critic | Visual hierarchy, accessibility audit, contrast ratios | 1 - DIAGNOSE | No (research-only) |
| 2 | Domain Expert | Domain-specific persona evaluation | 1 - DIAGNOSE | No (research-only) |
| 3 | Design System Agent | Spacing grid, type scale, color tokens | 2 - FOUNDATIONS | No (research-only) |
| 4 | Copy & Clarity Agent | Labels, errors, empty states, tone | 2 - FOUNDATIONS | No (research-only) |
| 5 | Motion & Delight Agent | Animations, celebrations, color safety | 3 - ENHANCE | No (research-only) |
| 6 | Resilience Agent | Overflow, edge cases, responsive adaptation | 3 - ENHANCE | No (research-only) |
| 7 | Polish & Extract Agent | Pixel alignment, performance, token extraction | 4 - SHIP | No (research-only) |
| 8 | Bolder/Overdrive Agent | Bold amplification, signature effects | 4 - SHIP | No (research-only) |

All agents are research-only -- they analyze and output findings with exact code changes. The orchestrator applies all edits during synthesis steps between iterations, resolving conflicts before any changes touch the file. This prevents write collisions and ensures consistent application.

## Principles

1. **Separate building from designing.** Get the feature working first, then hand it to the design loop for refinement. Building and designing use different thinking modes.

2. **Multi-persona brainstorm.** One person cannot hold all perspectives simultaneously. A motion designer sees opportunities an accessibility auditor would miss, and vice versa. Eight specialists find more than one generalist.

3. **One agent = one lens.** Each agent evaluates through a single concern. Opposing forces (delight vs distill, bolder vs harden) naturally balance each other when run in sequence.

4. **Subtraction before addition.** The Resilience Agent distills before hardening. The loop runs DIAGNOSE before ENHANCE. Remove what does not serve the user before adding new elements.

5. **Critique sandwich.** The loop starts with critique (Iteration 1) and ends with polish (Iteration 4). Early critique identifies problems. Late polish verifies they were solved. The middle iterations do the actual work.

## Dependencies

- **Required**: Claude Code with agent tool support
- **No other plugins required** -- all agent prompts are fully self-contained with their own evaluation criteria, checklists, and output formats.
- **Complementary**: [Impeccable](https://impeccable.style) -- provides individual slash commands for design concerns (critique, audit, animate, etc.). Compound Design Loop orchestrates the full pipeline. They are independent tools that work well together.
- **Optional**: [Ralph Loop](https://github.com/nichochar/ralph-loop) -- persistent iteration loop for running multiple design passes across a session.

## Examples

The `examples/` directory contains a sample before-file and a completed progress file:

- **[dashboard-before.html](examples/dashboard-before.html)** -- a motorcycle ride dashboard with common design issues (small tap targets, off-grid spacing, missing accessibility, no animations). Run the loop on it:
  ```bash
  /compound-design-loop:design-loop examples/dashboard-before.html --domain=motorcycle
  ```

- **[sample-progress.md](examples/sample-progress.md)** -- what a completed progress file looks like after a full 4-iteration run. Shows 23 fixes across 8 categories with the synthesis categorization in action.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Agent timeout | Reduce the file size or target a single component. Ensure a stable connection. Large files (3000+ lines) may exceed agent context. |
| Context exhaustion mid-loop | The file may be too large. Review a single component instead of a full page. The loop will set status to `partial` and preserve completed work. |
| Impeccable plugin not installed | No action needed. This plugin is fully standalone. Impeccable is a complementary tool, not a dependency. |
| "No active design loop found" from design-status | Run `/compound-design-loop:design-loop` first. The status command reads `design-review-progress.md`, which is created by the loop. |
| Stale progress file from a previous run | Delete `design-review-progress.md` in the target file's directory and start a fresh loop. |
| Agents giving generic advice | Check that context gathering completed. If you skipped the questions, agents run without platform/theme/scope context and produce less relevant findings. |
| File corrupted after loop | Run `git diff` to review changes and `git checkout -- <file>` to revert. This is why the loop requires a clean git working tree before starting. |

## Contributing

Contributions are welcome. Here is how to extend the plugin:

- **Adding a domain**: Create a new section in `.claude/skills/design-loop/reference/domain-experts.md` following the existing format (persona description, 10 evaluation criteria, competitor list, environmental constraints).

- **Modifying an agent**: Edit the agent's prompt in `.claude/skills/design-loop/SKILL.md` and update its role definition in `.claude/skills/design-loop/reference/agent-roles.md`. Keep the prompt structure consistent (mission, checklist, output format, DO/DON'T).

- **Testing locally**: Run `claude --plugin-dir ./` from the repository root, then invoke `/compound-design-loop:design-brainstorm test.html` to verify your changes with a quick brainstorm before testing the full loop.

- **Adding a reference file**: Place it in `.claude/skills/design-loop/reference/` and reference it from the agent prompts in `SKILL.md`.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

## License

MIT -- see [LICENSE](LICENSE).
