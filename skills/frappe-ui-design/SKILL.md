---
name: frappe-ui-design
description: "Build minimal, production-ready interfaces for Frappe-based products using ui.frappe.io components. Light theme only. Frappe component library exclusively."
origin: ECC
---

# Frappe UI Designer

Build minimal, implementation-ready interfaces for Frappe-based products using only `ui.frappe.io` components and patterns. Produces both UX guidance and production-ready code.

## When to Use

- Building new screens or modules in a Frappe-based product
- Extending or redesigning existing Frappe UI pages
- Translating a screenshot or Figma mockup into Frappe UI code
- Scaffolding a full feature module in a Frappe repo
- Choosing the right `ui.frappe.io` components for a given layout

Skip when: the project does not use Frappe framework, or the task is purely backend (API, DocType field logic, server scripts).

## Dhwani RIS Defaults

| Element | Value |
|---------|-------|
| Primary | Maroon `#8B1A1A` |
| Secondary | Gold `#C8991F` |
| Background | Cream `#FDF8F0` |
| Display font | Playfair Display |
| Theme | Light only |

If building for a non-Dhwani project, ask for primary and secondary colors before finalizing.

## Workflow

1. Identify the source: new feature, existing repo, screenshot/figma reference
2. Inspect current layout and repo conventions when available
3. Confirm primary and secondary colors (use Dhwani defaults or ask)
4. Map the solution to `ui.frappe.io` components only
5. Preserve existing layout structure unless redesign is explicitly requested
6. Return UX rationale and production-ready code

## Rules

- Use only `ui.frappe.io` components, patterns, and compatible compositions
- Never introduce a different component library as the main solution
- Keep layouts minimal, clean, and practical
- Prefer incremental improvement over disruptive redesign
- When a repo is available, adapt to its routing, naming, file structure, and shared utilities
- Scaffold full feature modules (not isolated snippets) when the repo structure supports it
- Light theme only

## Color Usage

- Primary: main CTA, active states, focus emphasis, key highlights
- Secondary: support emphasis, filters, badges, selected surfaces (use sparingly)
- Neutrals: dominant — keep the interface minimal
- Never use color as the only indicator of meaning

## Input-Specific Guidance

### New Feature or Module
- Clarify user goal, main actions, key states
- Choose a simple information hierarchy
- Break the screen into reusable `ui.frappe.io` sections
- Output file-level implementation steps and production-ready code

### Existing Repo
- Inspect current shell, sidebar, topbar, route structure, naming conventions
- Preserve current layout pattern where possible
- Reuse existing components before creating new ones
- Output exact files to create or update

### Screenshot or Figma Reference
- Preserve the recognizable structure first
- Simplify clutter, strengthen hierarchy, improve action clarity
- Translate to `ui.frappe.io`-compatible implementation
- Call out assumptions when the visual reference omits behavior

## Design Principles

Optimize for:
- Clear hierarchy
- Minimal visual noise
- Obvious primary action
- Consistent spacing
- Readable forms and data views
- Practical empty, loading, and error states

Avoid:
- Decorative styling that overwhelms task completion
- Too many competing accent colors
- Unnecessary component variety on a single screen
- Using color as the only indicator of meaning

## Response Structure

Always respond in this order:

1. **Problem understanding** — what the user wants to build or change
2. **UX approach** — layout, interaction flow, how existing structure is preserved
3. **Component map** — screen mapped to `ui.frappe.io` sections and components
4. **Color usage** — how primary/secondary colors apply
5. **Implementation steps** — files to create or update, in order
6. **Code** — production-ready code or exact file-level updates

## Key Rules

- Use only `ui.frappe.io` components -- never introduce a different component library as the primary solution
- Light theme only -- do not generate dark mode variants unless explicitly requested
- Preserve existing repo conventions (routing, naming, file structure) when extending an existing codebase
- Scaffold full feature modules, not isolated code snippets
- Always confirm primary and secondary colors before finalizing if building for a non-Dhwani project
- Never apply color as the only indicator of meaning

## Examples

**Example 1 -- New list view for a DocType:**
```
Problem: Build a list view for "Fund Request" DocType
UX approach: Standard Frappe list with filters sidebar, bulk actions toolbar
Component map:
  - ListView from ui.frappe.io for the main table
  - TextInput for search bar
  - Button (primary, maroon) for "New Fund Request"
  - Badge (gold accent) for status indicators
Files: fund_request/list_view.vue (new), fund_request/routes.js (update)
```

**Example 2 -- Screenshot translation:**
```
Problem: Translate dashboard mockup screenshot to Frappe UI
UX approach: Preserve the 3-column KPI card row, simplify the chart section
Component map:
  - Card for each KPI metric
  - Chart (BarChart) from ui.frappe.io for the trend visualization
  - Tabs for switching between "Monthly" and "Quarterly" views
Assumptions: Tooltip values not visible in screenshot -- defaulting to hover-to-reveal
```

## Common Pitfalls

- Importing components from libraries other than `ui.frappe.io` (e.g., Vuetify, Element UI)
- Generating dark theme styles when the project is light-theme-only
- Creating isolated component snippets instead of full module scaffolds with routing
- Ignoring existing sidebar, topbar, and shell patterns in the repo
- Using raw hex colors instead of CSS variables from the Frappe theme
