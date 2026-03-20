---
name: dris-ticketing
description: "Create structured Linear tickets (Feature/Bug/CR) from descriptions or Feature Specs. Batch-creates from specs. Invoke for 'raise a ticket', 'log a bug', 'create tickets from this spec'."
origin: ECC
---

# Ticketing — Claude Code Agent (Linear)

> Adapted from Claude UI skill: `ticketing` (was Bitrix format)
> **Claude Code upgrade:** Creates tickets directly in Linear via `linear` CLI or API. Searches codebase for accurate DocType/field names. Batch-creates from Feature Specs.

Generates structured tickets for Linear. One template, type-aware, actionable.

---

## Step 1 — Product Context

If mGrant → invoke the relevant domain agent for accurate DocType and field names.

## Step 2 — Gather Input

Extract from context first. Ask only for what's missing:

| Field | Required for |
|---|---|
| Ticket type: Feature / Bug / CR | All |
| Short description | All |
| Affected DocType(s) / module | All |
| App layer (mgrant / csr / nuo / client) | All |
| Steps to reproduce | Bug only |
| Expected vs actual | Bug only |
| Environment | Bug only |
| Priority (P0/P1/P2) | All |

## Step 3 — Title Format

```
[TYPE] Verb + object · Module
```

| Tag | Use for |
|---|---|
| `[FEAT]` | New feature or capability |
| `[BUG]` | Something broken |
| `[CR]` | Change to existing behaviour |

## Step 4 — Linear Ticket Body

```markdown
## Description
[2-3 sentences — what and why]

## Acceptance Criteria
- [ ] [Specific, testable criterion]
- [ ] [...]

## Technical Context
- **App layer:** [mgrant / csr / nuo / client]
- **DocType(s):** [exact names from codebase]
- **Affected fields:** [field names if applicable]

## Steps to Reproduce (Bug only)
1. ...
2. ...
- **Expected:** [what should happen]
- **Actual:** [what happens instead]

## Priority
[P0 / P1 / P2] — [one line justification]

## Labels
[feature / bug / change-request], [module name], [app layer]
```

## Step 5 — Batch Mode (from Feature Spec)

When a Feature Spec is provided:
1. Parse all requirements from sections 2 and 4
2. Generate one ticket per distinct requirement
3. Present full list to user for approval
4. On approval, create all tickets

## Step 6 — Execution (Claude Code Only)

Create tickets via Linear CLI if available:
```bash
linear issue create --title "..." --description "..." --team "..." --priority "..." --label "..."
```

If CLI not available, output as markdown files: `~/Desktop/tickets/[TYPE]-[slug].md`

**Always ask before creating:** "I have [N] tickets ready. Create them in Linear now?"
