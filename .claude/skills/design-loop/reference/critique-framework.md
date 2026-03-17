# 10-Dimension Critique Framework

Score each dimension independently. Use the anchor descriptions to calibrate your ratings. A dimension scored without justification is worthless — always cite specific elements.

---

## 1. Visual Hierarchy

How effectively does the layout guide the eye through content in order of importance?

**Scale**: 1-5
- **1** — No clear focal point. Everything competes for attention equally. User doesn't know where to look.
- **3** — Primary element is identifiable, but secondary and tertiary levels blur together.
- **5** — Immediate focal point, clear reading order, deliberate use of size/weight/color/position to rank every element.

**Non-Obvious Insights**:
- Whitespace is a hierarchy tool, not wasted space. The element with the most breathing room often reads as most important, even if it's not the largest.
- Hierarchy breaks down when more than 3 visual weights compete on a single screen. If you have 4+ "levels," 2 of them are functionally identical to the user.
- Users scan in F-pattern (content pages) or Z-pattern (marketing/hero pages) — hierarchy that fights these patterns creates cognitive friction even if it looks good statically.

---

## 2. Information Density

Is the amount of information appropriate for the context and viewport?

**Scale**: 1-5
- **1** — Either overwhelming (wall of data, no grouping) or wastefully sparse (one number on a full screen).
- **3** — Reasonable density but some information could be progressively disclosed or some gaps could be filled.
- **5** — Every element earns its screen space. Nothing to add, nothing to remove. Progressive disclosure used where appropriate.

**Non-Obvious Insights**:
- Optimal density depends on expertise level. A Bloomberg terminal user wants maximum density; a consumer fitness app user wants curated highlights. Neither is wrong.
- Grouping related items with shared borders or backgrounds increases perceived density without increasing cognitive load — 12 grouped items can feel lighter than 6 scattered ones.
- The "squint test" reveals density issues: squint at the screen and see if content clusters are identifiable or if it's uniform gray noise.

---

## 3. Touch/Interaction Targets

Are all interactive elements large enough and spaced enough for reliable activation?

**Scale**: Pass/Fail
- **Pass**: All interactive elements are at least 44x44px (Apple HIG) with at least 8px spacing between adjacent targets.
- **Fail**: Any interactive element below 44x44px or adjacent targets with insufficient spacing.

**Non-Obvious Insights**:
- The visual size of a button can be smaller than its hit area. A 32px icon can have a 48px tap target using padding — but this must be implemented, not assumed.
- Bottom-of-screen targets are harder to reach on large phones (thumb zone mapping). Critical actions should live in the comfortable reach zone.
- Adjacent destructive and constructive actions (Delete next to Save) need more than minimum spacing — add visual differentiation (color, position separation) as a second safeguard.

---

## 4. Contrast Ratios

Do text and interactive elements meet WCAG AA contrast requirements?

**Scale**: Pass/Fail
- **Pass**: Normal text meets 4.5:1 against its background. Large text (18px+ or 14px bold) meets 3:1. UI components and graphical objects meet 3:1.
- **Fail**: Any element below these thresholds.

**Non-Obvious Insights**:
- Semi-transparent overlays on images create variable contrast. The worst-case image (lightest or darkest region) determines the contrast, not the average.
- Placeholder text in inputs often fails contrast requirements. If it's important enough to display, it's important enough to be readable.
- Dark mode is not just inverted light mode. Pure white (#FFFFFF) text on dark backgrounds causes halation (glow effect) for users with astigmatism. Use off-white (#E0E0E0 to #F0F0F0).

---

## 5. Emotional Resonance

Does the design evoke the intended emotional response for the context?

**Scale**: 1-5
- **1** — The design feels generic, clinical, or emotionally disconnected from the content it presents.
- **3** — Appropriate tone but not memorable. Functional without being engaging.
- **5** — The design creates an emotional connection. Users would describe it with feeling words ("satisfying," "calming," "exciting").

**Non-Obvious Insights**:
- Emotional resonance is domain-dependent. A medical app should feel "trustworthy and calm," not "exciting." A fitness app post-PR should feel "triumphant," not "calm."
- Micro-interactions carry more emotional weight than static design. A button that gives satisfying haptic feedback on tap creates more emotional connection than a beautiful illustration the user glances past.
- Negative emotional resonance (anxiety at checkout, frustration at errors) is a design failure even if the visual design is polished.

---

## 6. State Differentiation

Can users clearly distinguish between all states of interactive and data-driven elements?

**Scale**: 1-5
- **1** — States are indistinguishable. Users can't tell if a button is active, disabled, or loading.
- **3** — Primary states (active/inactive) are clear, but intermediate states (loading, partial, error) are ambiguous.
- **5** — Every state is visually distinct and immediately identifiable: default, hover, pressed, focused, disabled, loading, error, success, empty, skeleton.

**Non-Obvious Insights**:
- The most commonly missed states are: disabled (grayed vs just lower contrast?), loading (spinner vs skeleton vs shimmer?), and partial/indeterminate (checkbox with mixed children).
- State transitions matter as much as state appearance. An element that jumps from loading to loaded feels broken; one that smoothly transitions feels polished.
- Disabled elements should look disabled but still be discoverable. Completely hidden disabled features frustrate users who know they exist ("Where did the button go?").

---

## 7. Typography Quality

Is the type system applied consistently with appropriate sizing, weight, and line-height?

**Scale**: 1-5
- **1** — Multiple arbitrary font sizes, inconsistent weights, poor line-height causing cramped or floating text.
- **3** — Follows a type scale mostly, but with a few off-system sizes and inconsistent line-height application.
- **5** — Strict adherence to type scale. Every text element has an intentional size, weight, line-height, and letter-spacing. Hierarchy is achieved through the type system alone.

**Non-Obvious Insights**:
- Line-height for body text should be 1.4-1.6x the font size. For headings, 1.1-1.3x. Using the same line-height multiplier for both creates either cramped headings or floaty body text.
- Letter-spacing should be tighter for large headings (negative tracking) and slightly open for small uppercase labels (positive tracking). Default letter-spacing is almost never optimal at size extremes.
- Font weight is a hierarchy tool, not a boldness toggle. Using only Regular and Bold wastes the Medium, Semibold, and Light weights that create nuanced hierarchy.

---

## 8. Color Coherence

Is the color palette intentional, consistent, and semantically meaningful?

**Scale**: 1-5
- **1** — Colors feel random. No clear semantic meaning. Brand colors absent or overused.
- **3** — Core palette is defined but application is inconsistent. Some colors have clear meaning, others are ambiguous.
- **5** — Every color has a purpose. Semantic colors (success, warning, error, info) are distinct. Brand colors are used strategically, not liberally. The palette works in both light and dark modes.

**Non-Obvious Insights**:
- More than 5-6 distinct hues on a single screen creates visual chaos. Use tints and shades of fewer hues instead of introducing new ones.
- Semantic colors should be reserved exclusively for their meaning. If green means "success," it should never be used decoratively — it will confuse the semantic signal.
- Color temperature affects perceived hierarchy. Warm colors (red, orange, yellow) advance visually; cool colors (blue, green, purple) recede. Use temperature intentionally to push and pull elements.

---

## 9. Animation Purposefulness

Does every animation serve a functional or emotional purpose?

**Scale**: 1-5
- **1** — Animations are absent (jarring state changes) or gratuitous (delays user, no informational value).
- **3** — Key transitions are animated but some animations feel decorative rather than purposeful.
- **5** — Every animation communicates (origin, destination, relationship, status) or celebrates (achievement, completion). No animation exists without a reason. Reduced motion alternatives are provided.

**Non-Obvious Insights**:
- The best animations are invisible — users don't notice them consciously but would notice their absence. If a user comments "nice animation," it might be too prominent.
- Animation duration communicates importance. A 100ms fade is "this is routine." A 400ms morph is "pay attention to this change." Using the same duration for everything removes this signal.
- Exit animations are as important as enter animations but are designed far less frequently. Content that appears with a graceful fade but disappears instantly feels unfinished.

---

## 10. Accessibility Compliance

Does the design meet WCAG AA standards across all applicable criteria?

**Scale**: Pass/Fail
- **Pass**: All contrast ratios met, touch targets sized correctly, screen reader flow logical, motion respects preferences, no information conveyed by color alone, heading hierarchy correct.
- **Fail**: Any WCAG AA criterion violated.

**Non-Obvious Insights**:
- Accessibility and aesthetics are not in tension. The constraint of 4.5:1 contrast ratios forces better color choices, not worse ones.
- Focus order is invisible to sighted users but defines the entire experience for keyboard and screen reader users. Test it by tabbing through the interface — if the order is illogical, the design has a structural problem.
- Decorative images should have empty alt text (alt=""), not no alt attribute. Missing alt forces screen readers to announce the filename, which is worse than silence.

---

## Scoring Summary Template

| Dimension | Score | Justification |
|---|---|---|
| Visual Hierarchy | /5 | |
| Information Density | /5 | |
| Touch/Interaction Targets | Pass/Fail | |
| Contrast Ratios | Pass/Fail | |
| Emotional Resonance | /5 | |
| State Differentiation | /5 | |
| Typography Quality | /5 | |
| Color Coherence | /5 | |
| Animation Purposefulness | /5 | |
| Accessibility Compliance | Pass/Fail | |
| **Scaled Total** | **/35** + **3 Pass/Fail** | |

---

**Avoid**: Scoring everything 3/5. If you can't identify specific strengths (4-5) and weaknesses (1-2), you haven't looked closely enough.
