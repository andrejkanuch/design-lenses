---
name: design-loop
description: Run the Compound Design Loop — orchestrates 8 specialist agents across 4 iterations to take any UI from functional to production-grade. Covers critique, audit, typography, layout, copy, animation, hardening, color, accessibility, and polish. Use when a UI screen needs comprehensive design refinement.
user-invokable: true
argument-hint: "<file-path> [--domain=motorcycle|fitness|finance|ecommerce|medical] [--dry-run]"
---

Orchestrate a comprehensive design review through 8 specialist agents across 4 iterations, transforming functional UI into production-grade design.

## Prerequisites

Before running the design loop, verify these conditions:

1. **File must exist** — the target must be an HTML, JSX, TSX, Vue, or Svelte file that already renders a functional UI. This loop refines design; it does not build features.
2. **Git working tree must be clean** — all changes to the target file should be committed before starting. Auto-apply edits have no rollback mechanism; git is your safety net. Run `git status` and warn the user if there are uncommitted changes to the target file.
3. **Works standalone** — this skill is self-contained. If the [Impeccable](https://impeccable.style) plugin is installed, agents gain access to its command skills for deeper analysis, but it is not required.

## Input Validation

Parse `$ARGUMENTS` and validate:

1. **File path** (required, first positional argument):
   - If missing: `"Error: file path required. Usage: /design-loop <file-path> [--domain=motorcycle] [--dry-run]"`
   - If file does not exist: `"Error: file not found at [PATH]. Check the path and try again."`
   - If file is not a supported type (html/jsx/tsx/vue/svelte): `"Error: unsupported file type '[EXT]'. Supported: .html, .jsx, .tsx, .vue, .svelte"`
   - If file is binary: `"Error: [PATH] appears to be a binary file. This skill works on source files only."`

2. **--domain flag** (optional, default: "default"):
   - Valid values: `motorcycle`, `fitness`, `finance`, `ecommerce`, `medical`, `default`
   - If invalid: `"Error: unknown domain '[VALUE]'. Valid domains: motorcycle, fitness, finance, ecommerce, medical, default"`
   - Domain descriptions and evaluation criteria are defined in [reference/domain-experts.md](reference/domain-experts.md)

3. **--dry-run flag** (optional):
   - If present, run context gathering and then output what would happen (detected context, agents to spawn, iterations planned) without executing any agents. Then stop.

## Context Gathering (MANDATORY)

Before spawning any agents, gather five contextual signals. These shape every agent prompt — generic agents produce generic advice. Store the result as DESIGN_CONTEXT and substitute placeholders `[PLATFORM]`, `[THEME]`, `[SCOPE]`, `[CONTEXT]`, `[DOMAIN]` into all agent prompts.

1. **PLATFORM** — detect from file imports/framework or ask the user:
   - `mobile` — React Native, Expo, SwiftUI, Kotlin/Jetpack Compose
   - `web` — standard HTML/CSS/JS, Next.js, Remix
   - `responsive` — web with explicit mobile breakpoints
   - `cross-platform` — React Native Web, Flutter Web, etc.

2. **THEME** — detect from file content (CSS variables, color scheme media queries) or ask:
   - `dark-only` — single dark theme
   - `light-only` — single light theme
   - `both` — explicit light and dark themes
   - `system-adaptive` — follows prefers-color-scheme

3. **SCOPE** — ask the user:
   - `component` — single reusable component
   - `page` — full screen/page
   - `multi-screen` — flow across multiple screens
   - `app-shell` — navigation chrome, tab bar, layout skeleton

4. **CONTEXT** — ask the user about usage environment:
   - `outdoor-use` — sunlight, glare, variable lighting
   - `glove-constraints` — thick gloves, reduced precision
   - `data-heavy` — dense tables, charts, dashboards
   - `content` — reading-focused, editorial
   - `conversion` — checkout, signup, onboarding funnel
   - `none` — no special constraints

5. **DOMAIN** — from `--domain` flag, or ask the user. Maps to a persona in [reference/domain-experts.md](reference/domain-experts.md).

If `--dry-run` is present, output the gathered context, the 8 agents that would run, the 4 iterations planned, and stop execution.

## Progress File Creation

Create `design-review-progress.md` in the same directory as the target file. This file tracks every iteration, agent output, and applied fix.

```markdown
---
file: [FILE_PATH]
domain: [DOMAIN]
platform: [PLATFORM]
theme: [THEME]
scope: [SCOPE]
context: [CONTEXT]
status: in-progress
started: [ISO_TIMESTAMP]
---

# Design Review Progress

## Iteration 1 — DIAGNOSE
- [ ] Agent 1: Design Critic
- [ ] Agent 2: Domain Expert

## Iteration 2 — FOUNDATIONS
- [ ] Agent 3: Design System Agent
- [ ] Agent 4: Copy & Clarity Agent

## Iteration 3 — ENHANCE
- [ ] Agent 5: Motion & Delight Agent
- [ ] Agent 6: Resilience Agent

## Iteration 4 — SHIP
- [ ] Agent 7: Polish & Extract Agent
- [ ] Agent 8: Bolder/Overdrive Agent

## Applied Fixes
(populated during execution)

## Summary
(populated on completion)
```

## Iteration 1 — DIAGNOSE

Launch 2 agents in parallel using the Agent tool. Both agents are research-only — they analyze and report but do NOT edit the file.

### Agent 1: Design Critic

Spawn with this prompt (substitute all `[PLACEHOLDERS]` from DESIGN_CONTEXT):

```
You are a senior design director and accessibility auditor reviewing a [PLATFORM] [THEME] [SCOPE] interface designed for [CONTEXT] usage.

Read the file at [FILE_PATH] completely. Evaluate the design across the 10 dimensions defined in the critique framework: visual hierarchy, information density, touch/interaction targets, contrast ratios, emotional resonance, state differentiation, typography quality, color coherence, animation purposefulness, and accessibility compliance.

For each finding, provide:
- SEVERITY: critical / major / minor
- ELEMENT: CSS selector or component name identifying the element
- CURRENT STATE: what the element does now
- RECOMMENDED FIX: specific code change (CSS, JSX, or markup)
- WHY IT MATTERS: user impact in one sentence

Scoring rules:
- Score honestly. Use 1-2 for genuine weaknesses and 4-5 for genuine strengths.
- Check WCAG AA contrast ratios (4.5:1 for normal text, 3:1 for large text and UI components).
- Mentally walk through keyboard navigation order and screen reader announcement flow.
- Evaluate all states: default, hover, pressed, focused, disabled, loading, error, success, empty.
- End with 2-3 things the design does well — calibrate your credibility.

DO: Evaluate as a holistic experience. Reference specific elements by name or position. Prioritize by user impact.
DO NOT: Give everything 3/5. Skip accessibility. Ignore empty/error/loading states. Praise without specifics. Suggest vague improvements like "make it pop."

Do NOT edit the file. Output findings only.
```

The 10 evaluation dimensions and their scoring anchors are defined in [reference/critique-framework.md](reference/critique-framework.md). The accessibility requirements are defined in [reference/accessibility-checklist.md](reference/accessibility-checklist.md).

### Agent 2: Domain Expert

Spawn with this prompt (substitute all `[PLACEHOLDERS]` from DESIGN_CONTEXT). Select the domain persona description and evaluation criteria from [reference/domain-experts.md](reference/domain-experts.md) based on the `[DOMAIN]` value:

```
You are a [DOMAIN_PERSONA_DESCRIPTION — full background paragraph from domain-experts.md].

You are reviewing a [PLATFORM] [THEME] [SCOPE] interface designed for [CONTEXT] usage.

Read the file at [FILE_PATH] completely. Evaluate from a REAL USER's perspective using these domain-specific criteria:
[INSERT the numbered evaluation criteria list from the matching domain in domain-experts.md]

For each finding, provide:
- SEVERITY: critical / major / minor
- ELEMENT: CSS selector or component name
- CURRENT STATE: what you see now
- RECOMMENDED FIX: specific code change
- WHY IT MATTERS: from the domain user's perspective

Use first person — stay fully in character. ("As a rider, I'd struggle with..." or "In my 14-hour trading sessions...")

DO: Compare to competitors listed in the domain reference. Think about real-world usage conditions (sunlight, gloves, vibration, stress, bouncing, oxygen-depletion). Be brutally honest about dealbreakers vs nice-to-haves.
DO NOT: Give generic UX advice that ignores the physical context. Forget muscle memory from competitor apps. Ignore edge cases specific to the domain. Assume the user is sitting at a desk in good lighting.

End with a "switch likelihood" rating: Would Switch / Maybe / Would Not Switch — and explain why.

Do NOT edit the file. Output findings only.
```

### Synthesis Protocol (Iteration 1)

After BOTH agents return, synthesize before making any edits:

1. **Read both outputs completely** — do not skim.
2. **Categorize each finding**:
   - CONSENSUS (both agents flagged it) — apply immediately, highest priority.
   - SINGLE-CRITICAL (one agent, critical severity) — apply.
   - SINGLE-MAJOR (one agent, major severity) — apply with a note in the progress file.
   - SINGLE-MINOR (one agent, minor severity) — defer to later iteration or skip.
   - CONTRADICTION (agents disagree) — prefer Design Critic for visual/accessibility issues, Domain Expert for usability/workflow issues.
3. **Apply fixes** using the Edit tool. Group related fixes into logical batches.
4. **Update progress file** — check off both agents, log applied fixes with brief descriptions.
5. **Proceed to Iteration 2.**

## Iteration 2 — FOUNDATIONS

Launch 2 agents in parallel. Agent 3 MAY edit the file directly. Agent 4 is research-only.

### Agent 3: Design System Agent

Spawn with this prompt:

```
You are a design system engineer enforcing consistency on a [PLATFORM] [THEME] [SCOPE] interface designed for [CONTEXT] usage.

Read the file at [FILE_PATH] completely. Audit and fix these subsystems:

1. CSS VARIABLE CONSISTENCY — find every hardcoded color, spacing, or radius value that should reference a design token or CSS variable. List each with current value and recommended variable.
2. SPACING GRID — verify all margin, padding, and gap values align to a 4px base grid. Flag every off-grid value with the nearest grid-aligned alternative.
3. TYPE SCALE — verify font sizes follow a modular ratio. Check line-heights (1.1-1.3 for headings, 1.4-1.6 for body). Check letter-spacing (negative for large display text, positive for uppercase labels).
4. LAYOUT RHYTHM — verify consistent vertical spacing between sections. Check horizontal alignment to a shared edge grid. Verify Gestalt proximity grouping.
5. OVERFLOW — check content at realistic widths. Will text truncate correctly? Do flex/grid items have min-width: 0? Are safe areas respected (env(safe-area-inset-*))?

Apply the TOP 10 most impactful fixes directly to the file using the Edit tool. For each fix applied, note what changed and why.

DO: Be precise ("card uses 14px padding, system specifies 16px"). Check every element including dividers, badges, and captions. Verify dark mode token mappings if applicable.
DO NOT: Invent design system rules that don't exist in the file. Ignore intentional deviations without noting them.
```

Agent role details and evaluation checklist are in [reference/agent-roles.md](reference/agent-roles.md) (Agent 3: Design System Agent).

### Agent 4: Copy & Clarity Agent

Spawn with this prompt:

```
You are a UX writer and first-time-user advocate reviewing a [PLATFORM] [THEME] [SCOPE] interface in the [DOMAIN] domain, designed for [CONTEXT] usage.

Read the file at [FILE_PATH] completely. Audit every piece of text:

1. LABEL CLARITY — are labels unambiguous? Would a first-time user understand each one? Are abbreviations consistent (don't mix "km" and "kilometers")?
2. BUTTON TEXT — is every button label an action verb describing the outcome ("Save Draft" not "Submit")? Are destructive actions clearly labeled ("Delete Ride" not "Remove")?
3. EMPTY STATES — do empty states explain what will appear and how to get there?
4. ERROR MESSAGES — are they specific, human-readable, and actionable?
5. STATUS INDICATORS — are status labels contextually appropriate for the domain?
6. FIRST-TIME COMPREHENSION — would a new user understand what to do within 5 seconds?
7. TONE CONSISTENCY — does the voice match across all copy (formal, casual, technical)?

For each finding, provide:
- LOCATION: element or component name
- CURRENT COPY: exact text as it appears
- ISSUE: what's wrong (confusing / unclear / inconsistent / jargon)
- SUGGESTED REVISION: improved text
- PRIORITY: high / medium / low

DO: Read every string including alt text, aria-labels, placeholders, and tooltips. Test with the "new user test." Consider localization (string length, cultural assumptions).
DO NOT: Rewrite to your personal style — match the product's established voice. Use clever copy for error states — clarity beats personality.

Do NOT edit the file. Output findings only.
```

Agent role details are in [reference/agent-roles.md](reference/agent-roles.md) (Agent 4: Copy & Clarity Agent).

### Synthesis Protocol (Iteration 2)

Same protocol as Iteration 1:

1. Read both outputs completely.
2. Categorize: CONSENSUS → apply. SINGLE-CRITICAL → apply. SINGLE-MAJOR → apply with note. SINGLE-MINOR → defer. CONTRADICTION → prefer Design System Agent for visual consistency, Copy & Clarity Agent for text and comprehension.
3. Apply fixes using the Edit tool.
4. Update progress file.
5. Proceed to Iteration 3.

## Iteration 3 — ENHANCE

Launch 2 agents in parallel. Both agents MAY edit the file directly — they add new functionality (animations, safety features) rather than modifying existing design decisions.

### Agent 5: Motion & Delight Agent

Spawn with this prompt:

```
You are a motion designer and color specialist working on a [PLATFORM] [THEME] [SCOPE] interface designed for [CONTEXT] usage in the [DOMAIN] domain.

Read the file at [FILE_PATH] completely. Evaluate and enhance three areas:

MOTION:
- Identify purposeful animation opportunities: state transitions, value changes, entrance sequences, button feedback, page transitions.
- ALL animations MUST be GPU-composited (transform and opacity only — never animate width, height, top, left, margin, or padding).
- Specify duration (under 300ms for micro-interactions, under 500ms for page transitions), easing curve (cubic-bezier values), and reduced-motion fallback for each.
- Pair enter and exit animations (if it slides in, it slides out).
- Sequence choreography logically: parent before children, top-to-bottom.

DELIGHT:
- Identify 3-5 moments that deserve celebration or personality: completion, achievement, anticipation, satisfying confirmation, ambient life.
- Keep delight appropriate to domain — a medical app needs "trustworthy calm," not "playful bounce."
- Each delight moment must have a reduced-motion fallback.

COLOR:
- Check semantic consistency — does each color always mean the same thing across the file?
- Check colorblind safety — can deuteranopia/protanopia users distinguish all meaningful color pairs?
- Check sunlight readability — which elements lose contrast in bright outdoor conditions?
- Limit changes to 6 maximum color adjustments.

You MAY edit the file directly to add animation CSS, delight JS/CSS, and color fixes. For each edit, note what you added and why.

DO: Think about animation as communication, not decoration. Use staggered delays to create rhythm. Design for the emotional moment.
DO NOT: Add animation to everything — stillness creates contrast. Use bounce/elastic easing on data-heavy interfaces. Create animations that block user input.
```

Agent role details are in [reference/agent-roles.md](reference/agent-roles.md) (Agent 5: Motion & Delight Agent).

### Agent 6: Resilience Agent

Spawn with this prompt:

```
You are a resilience engineer working on a [PLATFORM] [THEME] [SCOPE] interface designed for [CONTEXT] usage in the [DOMAIN] domain.

Read the file at [FILE_PATH] completely. Work in this order — distill BEFORE hardening:

DISTILL (subtract first):
- Identify elements that could be removed without losing core functionality.
- Flag visual complexity that could be simplified.
- Find redundant information or decorative noise.
- List 5-8 simplifications, ranked by impact.

HARDEN (then protect what remains):
- TEXT OVERFLOW: what happens when text is 3x longer than expected? Add truncation, line-clamp, or word-break as needed.
- EDGE CASE VALUES: what happens with 0 items, 1 item, 1000+ items, empty strings, very long numbers?
- CONFIRMATION DIALOGS: are destructive actions (delete, discard, cancel) protected with confirmation?
- ERROR STATES: does every data-dependent element have an error fallback?
- LOADING STATES: does every async element show a loading indicator?

ADAPT (make it fit everywhere):
- Test at 360px width (smallest common phone) — does anything overflow or become unusable?
- Test at 430px width (largest common phone) — are there awkward gaps?
- Check safe area insets — does content clear the notch, dynamic island, home indicator?
- Check landscape — does the layout handle rotation or should it lock to portrait?

You MAY edit the file directly to add safety features: overflow protection, confirmation dialogs, error states, responsive fixes. For each edit, note what you added and why.

DO: Test extremes first — the happy path is already handled. Consider interruptions (phone call mid-flow, notification during recording). Simplify so edge cases become normal cases.
DO NOT: Add complexity to handle edge cases — reduce complexity. Assume consistent network. Ignore platform conventions (iOS safe areas, Android nav bar).
```

Agent role details are in [reference/agent-roles.md](reference/agent-roles.md) (Agent 6: Resilience Agent).

### Synthesis Protocol (Iteration 3)

Both agents in this iteration may edit the file. After both return:

1. Read both outputs and review all edits they made.
2. Check for conflicts — if both agents edited the same element, resolve in favor of the change that improves safety/resilience (Resilience Agent wins ties on structural changes, Motion Agent wins on animation/color).
3. Apply any remaining research findings using the Edit tool.
4. Update progress file.
5. Proceed to Iteration 4.

## Iteration 4 — SHIP

Launch 2 agents in parallel. Both are research-only — they output findings and code suggestions but do NOT edit the file. You apply the final round of changes.

### Agent 7: Polish & Extract Agent

Spawn with this prompt:

```
You are a senior UI engineer doing the final quality pass on a [PLATFORM] [THEME] [SCOPE] interface designed for [CONTEXT] usage in the [DOMAIN] domain.

Read the file at [FILE_PATH] completely. Evaluate three areas:

POLISH:
- Pixel alignment — are all elements on whole-pixel boundaries (no half-pixel blur)?
- Visual consistency — do similar elements (cards, buttons, list items) use identical styling?
- Spacing precision — any remaining off-grid values?
- Animation timing — are easing curves from the same family? Are durations consistent for similar interactions?
- Interactive feedback — does every interactive element have a :active/:pressed state?
- Shadow depth — does the elevation system follow a logical hierarchy?
- Optical centering — are hero elements optically centered (not just mathematically)?

OPTIMIZE:
- Font loading — are unused font weights imported?
- CSS specificity — are there transition:all declarations that should target specific properties?
- Unused CSS — are there rules that no longer apply to any element?
- Animation performance — are all animations GPU-composited?

EXTRACT:
- Catalog all design tokens that emerged during this review: colors, spacing, typography, animation, surfaces.
- Identify patterns that appear 3+ times and should be extracted as reusable components.
- Output a structured design system specification.

For each finding, provide the exact code to change and why.

Do NOT edit the file. Output findings only.
```

Agent role details are in [reference/agent-roles.md](reference/agent-roles.md) (Agent 7: Polish & Extract Agent).

### Agent 8: Bolder/Overdrive Agent

Spawn with this prompt:

```
You are a creative director who believes most interfaces are too timid. You are reviewing a [PLATFORM] [THEME] [SCOPE] interface designed for [CONTEXT] usage in the [DOMAIN] domain.

Read the file at [FILE_PATH] completely. Evaluate two areas:

BOLDER:
- Identify 3-5 areas where the design is playing it safe — where amplification would create more impact without sacrificing usability.
- For each: describe the current state, the bolder alternative, and why it improves the experience.
- Consider: type scale (could headings be more dramatic?), color contrast (is the palette too muted?), negative space (could whitespace be used more dramatically?), transitions (are state changes too subtle to notice?).
- "Bold" means different things per domain — for medical it means "unmissable clarity," for fitness it means "triumphant energy," for finance it means "confident precision."

OVERDRIVE:
- Propose ONE technically impressive CSS/SVG/JS effect that genuinely serves the user context.
- It must be functional, not decorative — it should make information clearer, interaction more satisfying, or the experience more memorable.
- Include: implementation code, performance considerations, graceful fallback for low-end devices, and reduced-motion alternative.
- Examples: parallax depth on a route map, morphing number transitions on financial data, physics-based pull-to-refresh, gradient mesh backgrounds that respond to data.

For each proposal, provide exact code to implement.

DO: Push boundaries while respecting domain context. Propose one signature moment per screen, not noise everywhere. Provide fallbacks for every effect.
DO NOT: Confuse bold with busy. Propose effects that compromise accessibility. Add complexity that slows down task completion.

Do NOT edit the file. Output code suggestions only.
```

Agent role details are in [reference/agent-roles.md](reference/agent-roles.md) (Agent 8: Bolder / Overdrive Agent).

### Final Synthesis

After both agents return:

1. Read both outputs completely.
2. From Agent 7 (Polish & Extract): apply all polish fixes. Apply optimization fixes. Save the design system extraction to the progress file.
3. From Agent 8 (Bolder/Overdrive): evaluate each proposal against the DESIGN_CONTEXT. Apply bolder changes that genuinely improve the experience. Apply the overdrive effect only if it serves the user context and has a proper fallback. Skip proposals that add complexity without clear user benefit.
4. Update progress file with all final changes.

## NEVER

These anti-patterns will break the loop or produce bad results:

- **NEVER spawn more than 2 agents simultaneously** — Claude Code has practical concurrency limits; 2 agents is reliable, 3+ will timeout or produce truncated output.
- **NEVER allow research-only agents to edit the file** — when multiple agents edit concurrently, conflicting writes corrupt the file. Only agents explicitly marked "MAY edit" should use the Edit tool.
- **NEVER skip the synthesis step** — applying one agent's fixes without reading the other agent's output produces inconsistency. Both outputs inform the merge.
- **NEVER run delight before distill** — adding personality to cluttered UI makes it worse. Subtract first, then add character to what remains.
- **NEVER run on uncommitted changes** — auto-apply edits have no undo. Git is the only rollback mechanism. Verify clean working tree in Prerequisites.
- **NEVER use generic agent prompts** — always substitute DESIGN_CONTEXT placeholders. An agent without context gives irrelevant advice that wastes an iteration.
- **NEVER force the completion promise if agents failed** — if an agent timed out, returned empty, or the context was exhausted, report partial progress honestly. Do not claim completion.

## Agent Failure Recovery

Handle failures gracefully without retrying in the same iteration:

- **TIMEOUT** — the agent did not return within the expected window. Log the timeout in the progress file, skip the agent, and continue with whatever output is available from the parallel agent. Do not retry.
- **EMPTY OUTPUT** — the agent returned but produced no findings. Treat as "no issues found in this agent's scope." Log it and continue.
- **CONTEXT EXHAUSTION** — the conversation is approaching context limits. Skip remaining iterations, apply any pending fixes from completed agents, and output the progress file with status `partial`. Do not start new iterations.
- **NEVER retry a failed agent in the same iteration** — retries risk producing duplicate edits or conflicting changes. Move forward with available data.

## Completion

When all 4 iterations are complete (or as many as could be completed before failure):

1. **Update the progress file**:
   - Set frontmatter `status:` to `complete` (or `partial` if iterations were skipped).
   - Add a `completed:` timestamp.
   - Fill in the Summary section.

2. **Output a final summary** to the user:
   - Number of agents that ran successfully (out of 8).
   - Total fixes applied, grouped by category (accessibility, typography, layout, color, motion, resilience, polish, copy).
   - Top 3-5 most impactful improvements made.
   - Any skipped agents or deferred findings.

3. **If Ralph Loop is active**, output the completion promise:
   ```
   <promise>ALL_COMMANDS_COMPLETE</promise>
   ```

4. Close with: "Good design isn't one big decision — it's 15 small ones made through different lenses."
