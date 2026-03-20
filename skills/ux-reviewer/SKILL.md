---
name: ux-reviewer
description: "Review UI code for UX quality, accessibility, and design consistency. 10 priority categories from accessibility to data visualization."
origin: ECC
---

# UX Reviewer

Review UI code for user experience quality, accessibility, and design consistency. Covers 10 priority categories with specific checks and anti-patterns.

## When to Use

- Reviewing UI for quality, usability, or accessibility
- Pre-launch UI quality audit
- Aligning cross-platform design (web, iOS, Android)
- Investigating "looks unprofessional but not sure why" feedback
- Building or reviewing design systems

Skip when: pure backend, API/database design, infrastructure, non-visual scripts.

**Decision rule**: If the task changes how a feature looks, feels, moves, or is interacted with, use this skill.

## Review Process

1. Fetch the latest Web Interface Guidelines from: `https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md`
2. Read the specified files (or ask the user which files to review)
3. Check against the priority categories below AND the fetched guidelines
4. Output findings in `file:line` format

## Priority Categories

Review in this order (highest impact first):

### Priority 1: Accessibility (CRITICAL)

| Check | Standard |
|-------|----------|
| Color contrast | 4.5:1 normal text, 3:1 large text |
| Focus states | Visible 2-4px focus rings on all interactive elements |
| Alt text | Descriptive alt on all meaningful images |
| ARIA labels | aria-label on icon-only buttons |
| Keyboard nav | Tab order matches visual order, full keyboard support |
| Form labels | `<label>` with `for` attribute on all inputs |
| Skip links | "Skip to main content" for keyboard users |
| Heading hierarchy | Sequential h1-h6, no level skips |
| Color not only | Never convey info by color alone (add icon/text) |
| Reduced motion | Respect `prefers-reduced-motion` |

**Anti-patterns**: Removing focus rings, icon-only buttons without labels, placeholder-only labels.

### Priority 2: Touch and Interaction (CRITICAL)

| Check | Standard |
|-------|----------|
| Touch target size | Min 44x44pt (Apple) / 48x48dp (Material) |
| Touch spacing | Min 8px gap between touch targets |
| Hover vs tap | Primary interactions use click/tap, not hover-only |
| Loading buttons | Disable + spinner during async operations |
| Error feedback | Clear error messages near the problem field |
| Cursor pointer | `cursor: pointer` on all clickable elements |
| Tap delay | `touch-action: manipulation` to remove 300ms delay |
| Press feedback | Visual feedback on press (ripple/highlight) |
| Safe area | Keep targets away from notch, gesture bar, screen edges |

**Anti-patterns**: Hover-only interactions, instant state changes (0ms), precision-required taps.

### Priority 3: Performance (HIGH)

| Check | Standard |
|-------|----------|
| Image format | WebP/AVIF with fallbacks |
| Lazy loading | Below-fold images and heavy components |
| Layout stability | CLS < 0.1, reserve space for async content |

**Anti-patterns**: Layout thrashing, cumulative layout shift, unoptimized images.

### Priority 4: Style Consistency (HIGH)

| Check | Standard |
|-------|----------|
| Product-type match | Style matches the product category |
| Consistency | Single design language throughout |
| Icons | SVG icons, not emoji |

**Anti-patterns**: Mixing flat and skeuomorphic randomly, emoji as icons.

### Priority 5: Layout and Responsive (HIGH)

| Check | Standard |
|-------|----------|
| Mobile-first | Breakpoints start from mobile |
| Viewport meta | Proper viewport meta tag |
| No horizontal scroll | Content fits viewport at all breakpoints |
| Navigation | Predictable back behavior, bottom nav max 5 items, deep linking |

**Anti-patterns**: Horizontal scroll, fixed px container widths, disabled zoom, overloaded nav.

### Priority 6: Typography and Color (MEDIUM)

| Check | Standard |
|-------|----------|
| Base font size | 16px minimum for body text |
| Line height | 1.5 for body text |
| Color tokens | Semantic color tokens, not raw hex in components |

**Anti-patterns**: Body text < 12px, gray-on-gray, raw hex values scattered in components.

### Priority 7: Animation (MEDIUM)

| Check | Standard |
|-------|----------|
| Duration | 150-300ms for most interactions |
| Purpose | Motion conveys meaning, not decoration |
| Spatial continuity | Elements animate in relation to each other |

**Anti-patterns**: Decorative-only animation, animating width/height, no reduced-motion support.

### Priority 8: Forms and Feedback (MEDIUM)

| Check | Standard |
|-------|----------|
| Visible labels | Labels always visible (not placeholder-only) |
| Error placement | Error message near the field, not only at top |
| Helper text | Contextual guidance for complex fields |
| Progressive disclosure | Show only what's needed at each step |

**Anti-patterns**: Placeholder-only labels, errors only at page top, all fields shown at once.

### Priority 9: Navigation (HIGH)

| Check | Standard |
|-------|----------|
| Predictable back | Back button always works as expected |
| Bottom nav | Max 5 items |
| Deep linking | Every meaningful state has a URL |

**Anti-patterns**: Broken back behavior, overloaded nav, no deep links.

### Priority 10: Charts and Data (LOW)

| Check | Standard |
|-------|----------|
| Legends | All chart data series labeled |
| Tooltips | Interactive data points show values on hover/tap |
| Accessible colors | Color-blind-safe palettes, not color-only distinction |

**Anti-patterns**: Color-only data differentiation, missing legends.

## Output Format

```
UX Review: [file or component name]

Priority 1 - Accessibility
  FAIL  src/Button.tsx:23  Icon button missing aria-label
  PASS  Focus states present on all interactive elements

Priority 2 - Touch & Interaction
  FAIL  src/Form.tsx:45  Submit button has no loading state
  PASS  Touch targets meet 44x44pt minimum

[continue for each priority]

Summary: X issues found (Y critical, Z high, W medium)
```

## Key Rules

- Always fetch the latest Web Interface Guidelines before starting a review
- Report every finding with exact `file:line` location -- no vague references
- Review in priority order (accessibility first, charts last)
- Mark findings as PASS or FAIL with the specific standard that applies
- Never auto-fix code during a review -- report findings and let the user decide
- If no files are specified, ask which files or components to review before proceeding

## Examples

**Example 1 -- Accessibility finding:**
```
Priority 1 - Accessibility
  FAIL  src/components/IconButton.tsx:12  Icon button missing aria-label (button contains only an SVG icon, no accessible name)
  Standard: WCAG 4.1.2 Name, Role, Value
  Recommendation: Add aria-label="Delete item" to the button element
```

**Example 2 -- Touch and interaction finding:**
```
Priority 2 - Touch & Interaction
  FAIL  src/components/Toolbar.tsx:34  Action buttons are 32x32px, below 44x44pt minimum
  Standard: Apple HIG touch target guideline
  Recommendation: Increase button padding to achieve minimum 44x44pt touch area
```

**Example 3 -- Full summary:**
```
UX Review: src/pages/Dashboard.tsx

Priority 1 - Accessibility
  FAIL  :23  KPI cards use color-only status indicators (no icon or text)
  PASS  Focus states present on all interactive elements

Priority 2 - Touch & Interaction
  PASS  All buttons meet 44x44pt minimum

Summary: 1 issue found (1 critical, 0 high, 0 medium)
```

## Common Pitfalls

- Reviewing only accessibility and skipping lower-priority categories (touch, performance, layout)
- Flagging decorative images for missing alt text (they should use `alt=""`)
- Reporting contrast failures without calculating the actual ratio
- Missing hover-only interactions that break on touch devices
- Ignoring `prefers-reduced-motion` when animations are present
- Reviewing backend-rendered HTML without accounting for client-side hydration changes

## Dhwani RIS Context

When reviewing Dhwani RIS products, also check:
- Brand colors used correctly (maroon `#8B1A1A`, gold `#C8991F`, cream `#FDF8F0`)
- Playfair Display used for display text
- Light theme consistency
- Frappe UI component usage (for Frappe-based products)
