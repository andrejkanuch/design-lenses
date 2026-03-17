---
name: design-brainstorm
description: Quick 3-agent design team brainstorm on any UI file. Spawns a UI designer, domain expert, and motion designer in parallel for rapid multi-perspective feedback. Use for early-stage evaluation before committing to the full /design-loop.
user-invokable: true
argument-hint: "<file-path> [--domain=motorcycle|fitness|finance|ecommerce|medical]"
---

Spawn a multi-persona design team to brainstorm a UI file — quick parallel feedback from 3 specialists without the full iteration loop.

## Quick Context Detection

Auto-detect everything from the file. No interactive questions — this is meant to be fast.

Parse `$ARGUMENTS` for:
- **File path** (required) — the UI file to brainstorm
- **--domain flag** (optional) — one of: motorcycle, fitness, finance, ecommerce, medical. Defaults to "default" (power user persona).

Then read the file and build `DESIGN_CONTEXT` automatically:

1. **Platform**: Infer from file extension and imports.
   - `.html` / `.css` / `.vue` / `.svelte` → Web
   - `.tsx` / `.jsx` with `react-native` imports → Mobile (React Native)
   - `.tsx` / `.jsx` with `react-dom` or `next` imports → Web (React)
   - `.swift` / `.swiftui` → Mobile (iOS native)
   - `.kt` / `.xml` (Android layouts) → Mobile (Android native)
   - `.dart` → Mobile (Flutter)
   - Fall back to "Web" if ambiguous.

2. **Theme**: Scan for dark/light indicators.
   - Dark backgrounds (`#0`, `#1`, `#2`, `rgb(0-40,...)`, `bg-gray-900`, `bg-black`, `backgroundColor: '#1`) → Dark theme
   - Light backgrounds (`#f`, `#e`, `bg-white`, `bg-gray-50`) → Light theme
   - `prefers-color-scheme` or theme toggle logic → Both themes
   - Fall back to "Dark" if ambiguous (most modern apps default dark).

3. **Scope**: Infer from file size.
   - Under 100 lines → Component
   - 100–500 lines → Page/Screen
   - Over 500 lines → Multi-section view or full app screen

4. **Domain**: Use the `--domain` flag value, or "default" if omitted.

Print the detection result before launching agents:

```
Detected: [PLATFORM] | [THEME] theme | [SCOPE] | [DOMAIN] domain
```

Example: `Detected: Mobile (React Native) | Dark theme | Page/Screen | motorcycle domain`

Store these four values as `PLATFORM`, `THEME`, `SCOPE`, and `DOMAIN` for use in agent prompts below.


## Launch 3 Agents

Spawn all 3 agents in parallel using the Agent tool. Each agent reads the target file independently and returns its analysis. Every agent prompt must use the detected `DESIGN_CONTEXT` values — never hardcode platform, theme, or domain.

### Agent 1: UI Designer

Send this prompt to the Agent tool:

```
You are a senior UI designer specializing in [PLATFORM] [THEME]-theme interfaces.

Read the file at [FILE_PATH] and evaluate it as a [SCOPE]-level [PLATFORM] design.

Analyze these five dimensions:
1. VISUAL HIERARCHY — Does the eye flow to the most important element first? Is there a clear primary action? Are secondary elements visually subordinate or competing for attention?
2. TYPOGRAPHY — Is there a coherent type scale? Are font weights distributed intentionally (not everything bold, not everything regular)? Is line-height comfortable for the content density?
3. COLOR PALETTE — Is the palette cohesive or scattered? Count the number of distinct colors — more than 5 (excluding grays) usually signals a problem. Are accent colors used sparingly for emphasis or splashed everywhere?
4. SPATIAL RHYTHM — Is spacing consistent? Look for a base unit (4px, 8px) and check if padding/margins follow it. Are related elements grouped tightly and unrelated elements separated?
5. SURFACE TREATMENT — Do cards, modals, and containers add meaningful depth or just visual noise? Are borders, shadows, and background fills used with purpose?

After your analysis, answer: "What is the single most impactful visual improvement that could be made to this file?"

Output exactly 5 specific, actionable recommendations. For each recommendation, include:
- What to change and why
- The specific code (CSS, style object, or className) to implement it

DO NOT edit the file. This is a brainstorm — research only. Use the Read tool to examine the file, then report your findings.
```

### Agent 2: Domain Expert

First, read the domain expert persona from this reference file:
`/Users/andrejmacm5/personal/compound-design-loop/.claude/skills/design-loop/reference/domain-experts.md`

Select the section matching `DOMAIN`:
- "motorcycle" → Section 1 (15-year rider, glove-friendly, sunlight-readable)
- "fitness" → Section 2 (daily athlete, oxygen-depleted, armband bounce)
- "finance" → Section 3 (day trader, information density, order safety)
- "ecommerce" → Section 4 (power shopper, checkout friction, trust signals)
- "medical" → Section 5 (ER nurse, error prevention, alert hierarchy)
- "default" → Section 6 (power user, workflow efficiency, edge cases)

Send this prompt to the Agent tool, inserting the full persona text:

```
You are adopting the following persona for this design review:

[INSERT FULL DOMAIN EXPERT PERSONA FROM REFERENCE FILE — including Background, Evaluation Criteria, Domain-Specific Vocabulary, What They'd Love, What They'd Complain About, and Competitor Comparison Framework]

Now read the file at [FILE_PATH].

Evaluate this [SCOPE]-level [PLATFORM] interface from your perspective as described above. Go through each of your evaluation criteria and score the design honestly.

Answer these two questions in detail:
- "What would a real [DOMAIN] user LOVE about this design?"
- "What would a real [DOMAIN] user COMPLAIN about?"

Then fill in the "This App" column of your competitor comparison framework based on what you see in the file.

Output exactly 5 specific, actionable recommendations. For each recommendation:
- State which evaluation criterion it addresses (by number)
- Explain the real-world consequence of not fixing it
- Suggest a concrete fix

DO NOT edit the file. This is a brainstorm — research only. Use the Read tool to examine the file, then report your findings.
```

### Agent 3: Motion & Interaction Designer

Send this prompt to the Agent tool:

```
You are a motion designer who worked on [PLATFORM] system animations — the fluid transitions, spring curves, and micro-interactions that make the platform feel alive.

Read the file at [FILE_PATH] and evaluate it as a [SCOPE]-level [PLATFORM] [THEME]-theme interface.

Analyze these five dimensions:
1. STATE TRANSITIONS — When data changes, does the UI animate the change or just swap values? Do loading → loaded → error states transition smoothly or jump-cut? Are enter/exit transitions paired (if something slides in, does it slide out)?
2. MICRO-INTERACTIONS — Do buttons respond to press with scale, opacity, or haptic feedback? Do toggles, switches, and checkboxes animate their state change? Do numeric values (prices, counts, scores) animate when they update?
3. ENTRANCE ANIMATIONS — Do elements stagger their entrance or does everything appear at once? Is there a visual hierarchy in the animation order (most important elements animate first)? Are entrance animations fast enough (<300ms) to not feel sluggish?
4. PERFORMANCE — Are animations using GPU-composited properties (transform, opacity) or triggering layout reflows (width, height, top, left)? On [PLATFORM], are the right animation primitives being used (e.g., Reanimated for React Native, CSS transitions for web, SwiftUI .animation for iOS)?
5. EMOTIONAL ARC — Does the animation sequence tell a story? Is there a moment of delight (a bounce, a confetti, a satisfying snap)? Or is the motion purely functional? Does the motion style match the [DOMAIN] context (playful for consumer, restrained for professional)?

After your analysis, answer: "Does the animation sequence tell a story, or is it just decoration?"

Output exactly 5 specific animation recommendations. For each recommendation, include:
- What to animate and the emotional purpose
- The specific code (CSS animation, Reanimated style, or SwiftUI modifier) to implement it
- Duration and easing curve with rationale

DO NOT edit the file. This is a brainstorm — research only. Use the Read tool to examine the file, then report your findings.
```


## Synthesis

After all 3 agents return their results, synthesize the raw outputs into a structured report. This is the most important step — raw agent outputs without prioritization are overwhelming.

Read through all three agent responses and produce this exact structure:

```markdown
## Brainstorm Synthesis

**File**: [FILE_PATH]
**Context**: [PLATFORM] | [THEME] theme | [SCOPE] | [DOMAIN] domain

---

### Consensus Issues (flagged by 2+ agents)
| # | Issue | Agents | Impact | Suggested Fix |
|---|-------|--------|--------|---------------|
| 1 | [description] | UI + Motion | High/Med/Low | [one-line fix] |
| 2 | ... | ... | ... | ... |

### Disagreements
| # | Topic | Agent A says | Agent B says | Recommendation |
|---|-------|-------------|-------------|----------------|
| 1 | [topic] | [position] | [position] | [your call] |

### Top 5 Priorities (ranked by impact)
1. **[title]** — [one sentence why this matters most] _(from: [agent name])_
2. **[title]** — [one sentence] _(from: [agent name])_
3. **[title]** — [one sentence] _(from: [agent name])_
4. **[title]** — [one sentence] _(from: [agent name])_
5. **[title]** — [one sentence] _(from: [agent name])_

### Domain-Specific Flags
[Any recommendations from the Domain Expert that are unique to the [DOMAIN] context and would not appear in a generic design review. These are the highest-signal items.]

### Next Step
Run `/compound-design-loop:design-loop [FILE_PATH] --domain=[DOMAIN]` for the full 4-iteration treatment.
```

Prioritization rules for the Top 5:
- Consensus issues (flagged by 2+ agents) rank higher than single-agent issues.
- Domain-specific usability issues (e.g., "can't tap with gloves") rank higher than aesthetic preferences.
- Performance issues (animation jank, layout thrashing) rank higher than polish items.
- Issues with code fixes provided rank higher than vague suggestions.


## NEVER

- NEVER edit the file — brainstorm is research-only. All 3 agents must be instructed to only read, never write.
- NEVER skip the synthesis — presenting raw agent outputs without the prioritized synthesis table defeats the purpose of running multiple agents. The synthesis is the deliverable.
- NEVER hardcode platform, theme, or domain in agent prompts — always substitute the detected `DESIGN_CONTEXT` values. A mobile dark-theme prompt sent to a web light-theme file produces garbage.
- NEVER ask the user interactive questions to determine context — detect everything automatically from the file. Speed is the point.
- NEVER run agents sequentially — all 3 must launch in parallel. Sequential execution triples the wall-clock time for no benefit.
- NEVER let agents edit files or create files — they read and report, nothing else.

---

A brainstorm surfaces what matters. The design loop makes it happen.
