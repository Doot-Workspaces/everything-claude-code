---
name: accessibility-auditor
description: "Audit and fix accessibility issues in UI code. WCAG 2.1 AA compliance including color contrast, use of color, link purpose, semantic HTML, keyboard navigation."
origin: ECC
---

# Accessibility Auditor

Analyze, report, and fix WCAG 2.1 accessibility issues in UI code. Covers contrast ratios, color-only indicators, link purpose, semantic HTML, keyboard navigation, ARIA patterns, and focus management.

## When to Use

- Auditing a component, page, or codebase for WCAG 2.1 compliance
- Fixing reported accessibility violations
- Reviewing color contrast, keyboard navigation, or ARIA usage
- Preparing for an accessibility certification or compliance review
- Checking Dhwani RIS brand colors against contrast requirements

Skip when: pure backend logic, API design, database schema, or infrastructure tasks with no UI impact.

## Modes

This skill operates in two modes:

1. **Audit mode** (default) — analyze files and report violations with recommendations
2. **Fix mode** — apply fixes directly to the codebase (ask before major changes)

## Scope

File paths are required. If none provided, ask which files or components to check.

- Only analyze files within the specified scope
- Search the full codebase for color definitions, CSS variables, and theme files referenced by scoped components

## WCAG Checks

### 1. Color Contrast (WCAG 1.4.3 — Level AA)

**Requirements:**
- Normal text: 4.5:1 minimum contrast ratio
- Large text (18pt+ or 14pt+ bold): 3:1 minimum
- UI component boundaries (borders, focus indicators, icons): 3:1 minimum (WCAG 1.4.11)

**Important**: Text inside UI components (buttons, inputs) needs TEXT contrast ratios (4.5:1), not UI component ratios (3:1).

**Analysis process:**
1. Extract all color values from the file (inline styles, CSS classes, variables)
2. Trace CSS variables and design tokens to their actual values
3. Calculate contrast ratios for every foreground/background pair
4. Report violations with specific hex values and current ratios

**Report format per violation:**
```
Location: file:line
Component: [type] ([text content])
Current: #foreground on #background (X.X:1)
Required: 4.5:1 (normal text — WCAG 1.4.3)
Status: FAIL
Recommendation: Change text to #hexcode (new ratio: X.X:1)
```

### 2. Use of Color (WCAG 1.4.1 — Level A)

**Requirement**: Color must not be the only visual means of conveying information.

**Common violations:**
- Links distinguished from text only by color (no underline)
- Form errors shown only with red border (no icon or message)
- Required fields marked only with colored asterisk
- Status indicators (success/error/warning) using only color
- Hover/focus states with only color change
- Charts differentiating data only by color
- Color-coded categories without icons or text

**For each violation, recommend:**
- Add a secondary indicator (icon, text label, underline, pattern)
- Add ARIA attributes (`aria-invalid`, `aria-describedby`)
- Use `prefers-color-scheme` and `prefers-contrast` media queries

### 3. Link Purpose (WCAG 2.4.4 — Level A)

**Requirement**: Link purpose must be determinable from the link text alone or from text + programmatic context.

**Detect these patterns:**
- Generic link text: "click here", "read more", "learn more", "more", "here", "continue", "details"
- Multiple links with identical text pointing to different destinations (especially in `.map()` loops)
- Image-only links without alt text or aria-label
- URL-only link text
- Download links without file type/size information
- Icon-only links without accessible names

**Fixes (in preference order):**
1. Make link text descriptive on its own (best — Level AAA)
2. Add visually hidden text (`<span class="sr-only">`) inside the link
3. Use `aria-label` or `aria-labelledby`
4. Link the heading or image instead of adding a separate link

### 4. General Accessibility Fixes

**Simple fixes:**
- Missing alt text on images
- Missing ARIA labels on buttons and links
- Form inputs without associated labels
- Missing `lang` attribute on HTML
- Broken heading hierarchy (h1 -> h3 skipping h2)
- Missing landmark roles

**Moderate fixes:**
- `<div>` used where semantic HTML applies (`<nav>`, `<main>`, `<section>`, `<button>`)
- Buttons vs links used incorrectly (links navigate, buttons act)
- Missing keyboard event handlers on custom interactive elements
- No skip links
- Form validation without accessible error messages

**Complex fixes (ask before applying):**
- Focus trap for modals
- Accessible dropdown/select components
- Accessible tabs/accordion patterns
- ARIA live regions for dynamic content
- Full keyboard navigation restructuring

## Output Format

### Audit Mode

```
Accessibility Audit Report

Files analyzed: N
Violations found: N
  - Contrast: X
  - Use of color: X
  - Link purpose: X
  - Other: X

---

Violation #1: src/components/Card.tsx:23
Category: Color Contrast
Issue: Text on button has insufficient contrast
Current: #7c8aff on #ffffff (3.03:1)
Required: 4.5:1 (normal text — WCAG 1.4.3)
Recommendation: Change text to #5061ff (4.67:1)

---

[more violations...]

Summary:
  Critical (blocks Level A): X
  Important (blocks Level AA): X
  Recommendations: X
```

### Fix Mode

For each file modified:
```
File: src/components/Modal.tsx
Issue: No focus trap, missing ARIA attributes, no Escape key handler
Changes:
  1. Added role="dialog" and aria-modal="true" (line 15)
  2. Added aria-labelledby pointing to title (line 16)
  3. Implemented Escape key handler (lines 22-30)
  4. Added focus trap (lines 12-35)
WCAG addressed: 2.1.2 No Keyboard Trap, 2.4.3 Focus Order, 4.1.2 Name, Role, Value
Testing: Tab through modal, verify Escape closes, verify focus returns to trigger
```

## Testing Recommendations

After audit or fix, recommend:

1. **Screen reader testing**: NVDA (Windows) or VoiceOver (Mac)
   - Pull up links list (Insert+F7 in NVDA, VO+U in VoiceOver)
   - Navigate by headings, landmarks, and form controls
2. **Keyboard testing**: Tab through all interactive elements
3. **Automated tools**: axe DevTools, Lighthouse, WAVE
4. **Color tools**: Browser DevTools contrast checker, Sim Daltonism for color blindness simulation
5. **Manual review**: Read through all links out of context, check all form error flows

## Key Rules

- Always cite specific WCAG success criteria (e.g., 1.4.3, 2.4.4) for every violation
- Never apply complex fixes (focus traps, ARIA live regions, keyboard restructuring) without asking first
- Report exact hex values and calculated contrast ratios, not approximations
- Trace CSS variables and design tokens to their resolved values before reporting
- File paths are required -- if none provided, ask which files to audit
- Audit mode is the default; only switch to fix mode when explicitly requested

## Examples

**Example 1 -- Contrast violation report:**
```
Location: src/components/Badge.tsx:14
Component: <span> ("Pending")
Current: #9CA3AF on #F3F4F6 (1.92:1)
Required: 4.5:1 (normal text -- WCAG 1.4.3)
Status: FAIL
Recommendation: Change text to #4B5563 (5.74:1)
```

**Example 2 -- Use of color violation:**
```
Location: src/components/StatusDot.tsx:8
Component: <div> (status indicator)
Issue: Status conveyed only by dot color (green=active, red=inactive)
WCAG: 1.4.1 Use of Color (Level A)
Recommendation: Add text label or icon alongside color dot
```

## Common Pitfalls

- Checking only inline styles and missing CSS variables / theme tokens that resolve to low-contrast values
- Applying UI component contrast ratio (3:1) to text inside buttons -- text always needs 4.5:1
- Reporting color-only issues on decorative elements that carry no informational meaning
- Fixing heading hierarchy by changing heading levels without checking if it breaks the page outline
- Assuming all images need descriptive alt text -- decorative images should use `alt=""`

## Dhwani RIS Context

When auditing Dhwani RIS products:
- Check maroon `#8B1A1A` on cream `#FDF8F0` passes contrast (it does: ~7.2:1)
- Check gold `#C8991F` on cream `#FDF8F0` for text use (may need darker variant for small text)
- Frappe forms: verify all custom form fields have proper labels and error states
