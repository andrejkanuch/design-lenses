# Accessibility Checklist (WCAG AA)

This checklist is designed for design review context, not audit compliance paperwork. Each item should be verifiable by inspecting the design or implementation. Evaluate these alongside every other design dimension, not as a separate pass.

---

## 1. Color & Contrast

| Requirement | Threshold | Applies To |
|---|---|---|
| Normal text contrast | 4.5:1 minimum | All text below 18px (or below 14px bold) |
| Large text contrast | 3:1 minimum | Text at 18px+ or 14px+ bold |
| UI component contrast | 3:1 minimum | Borders, icons, focus indicators, form controls |
| Graphical object contrast | 3:1 minimum | Charts, graphs, meaningful icons, infographics |
| Non-text contrast against adjacent | 3:1 minimum | Buttons against background, input borders |

**Testing Tools**: WebAIM Contrast Checker, Stark (Figma plugin), Xcode Accessibility Inspector, Chrome DevTools color picker.

**Common Failures**:
- Placeholder text in inputs (often gray on white, ~2:1 ratio)
- Disabled button text (intentionally low contrast but still needs to be identifiable)
- Text on gradient or image backgrounds (test the worst-case region, not the average)
- Subtle borders on cards or inputs that disappear in certain lighting conditions

---

## 2. Focus & Keyboard

| Requirement | Details |
|---|---|
| Focus indicator visible | All interactive elements must show a visible focus ring/outline when focused via keyboard |
| Focus indicator contrast | Focus indicator itself must have 3:1 contrast against adjacent colors |
| Tab order logical | Focus moves through the interface in a logical reading order (top-to-bottom, left-to-right for LTR) |
| No keyboard traps | User can always tab away from any focused element (modals must trap focus internally but allow Escape to exit) |
| Skip navigation | Long pages provide a "skip to content" link as the first focusable element |
| Custom components | All custom interactive components (dropdowns, sliders, toggles) must be keyboard-operable |

**Common Failures**:
- Custom-styled focus rings that are removed (`outline: none`) without a visible replacement
- Modal dialogs that don't trap focus, allowing users to tab to elements behind the overlay
- Carousels and tabs that don't support arrow key navigation within the component group
- Dropdown menus that can't be dismissed with Escape

---

## 3. Screen Reader

| Requirement | Details |
|---|---|
| Icon-only buttons | Must have `aria-label` or `aria-labelledby` describing the action ("Close dialog", not "X") |
| Decorative images/SVGs | Must have `aria-hidden="true"` to prevent screen reader announcement |
| Meaningful images | Must have descriptive `alt` text (what the image communicates, not what it depicts) |
| Hidden animated content | Use `visibility: hidden` + `height: 0` with `aria-hidden`, not `display: none` for content that animates in/out |
| Live regions | Dynamic content updates (notifications, counters, status changes) must use `aria-live="polite"` or `aria-live="assertive"` |
| Form labels | Every input must have a programmatically associated label (not just placeholder text) |
| Error announcements | Form validation errors must be announced to screen readers, not just visually displayed |

**Common Failures**:
- Toggle switches that announce "button" instead of "switch" with on/off state
- Dynamic toast notifications that appear visually but are never announced
- Icon buttons with `aria-label="icon"` or `aria-label="button"` instead of the action name
- Tables used for layout instead of semantic data, confusing screen reader navigation

---

## 4. Touch Targets

| Requirement | Minimum | Recommended | Notes |
|---|---|---|---|
| Touch target size | 44x44px (Apple HIG) | 48x48px (Material) | Visual size can be smaller if hit area is padded |
| Spacing between targets | 8px | 12px | Prevents accidental activation of adjacent targets |
| Outdoor / glove use | 48x48px | 56x56px | For motorcycle, fitness, outdoor domains |
| Target within scrollable area | 44x44px | 48x48px | Scrolling reduces tap precision |

**Common Failures**:
- Text links in body copy that are the only tap target (add padding or make the entire row tappable)
- Close buttons (X) in top corners that are 24x24px with no padding
- Footer navigation items squeezed to fit 5+ items in a single row
- Inline action buttons within list items that are smaller than the row height

---

## 5. Motion

| Requirement | Details |
|---|---|
| `prefers-reduced-motion: reduce` | All non-essential animations must be eliminated or reduced to simple opacity fades |
| No auto-playing animations | Animations lasting more than 5 seconds must have pause/stop controls |
| No flashing content | Nothing flashes more than 3 times per second (seizure risk) |
| Parallax and scroll-based effects | Must be disabled when reduced motion is preferred |
| Loading animations | Acceptable under reduced motion (functional, not decorative) but should be simplified |
| Transition vs animation | Under reduced motion, transitions should be instant or opacity-only; keyframe animations should be removed |

**Common Failures**:
- Skeleton loading screens with shimmer animation that ignore reduced motion
- Page transition animations that can't be skipped or reduced
- Background video or animated gradients without pause controls
- Hover animations on desktop that have no reduced-motion alternative

---

## 6. Semantics

| Requirement | Details |
|---|---|
| Heading hierarchy | Must follow logical order: h1 then h2 then h3 (never skip from h1 to h3) |
| Single h1 per page | Each screen/page should have exactly one h1 element |
| Landmark roles | Pages should use semantic landmarks: `main`, `nav`, `header`, `footer`, `aside` |
| Button vs link | Buttons perform actions; links navigate. Using one as the other confuses assistive technology |
| Lists | Related items should use semantic list markup (`ul`/`ol` + `li`), not styled divs |
| Tables | Data tables must have `th` headers with `scope` attributes; never use tables for layout |
| Language attribute | The page must declare its language (`lang="en"`) for correct screen reader pronunciation |

**Common Failures**:
- Using heading tags for visual styling rather than document structure (h3 used because it "looks right")
- Links styled as buttons that don't navigate anywhere (should be `button` elements)
- Navigation menus built with divs instead of `nav` + `ul` + `li` + `a`
- Multiple h1 tags on a single page (common in component-based architectures)

---

## 7. Color Independence

| Requirement | Details |
|---|---|
| Color not sole indicator | Status, errors, and states must use icons, text labels, or patterns in addition to color |
| Form validation | Error states must include an icon or text label, not just a red border |
| Charts and graphs | Data series must be distinguishable by shape, pattern, or label, not just color |
| Links in body text | Must be distinguishable from surrounding text by underline or other non-color indicator |
| Status indicators | Online/offline, success/error, active/inactive must use more than green/red |
| Toggle/selection state | Selected items must have a non-color indicator (checkmark, border weight, icon change) |

**Common Failures**:
- Red/green as the only way to distinguish positive/negative values in financial displays
- Form fields that only turn red on error without an error icon or message
- Pie charts and bar graphs with legend colors that are indistinguishable in grayscale
- "Click the blue link" instructions that assume color perception

---

**Avoid**: Treating accessibility as a separate concern. It should be evaluated alongside every other design dimension, not as an afterthought checklist.
