---
name: dris-weekly-update
description: "Generate weekly progress reports from git activity and Linear tickets. Invoke for 'weekly update', 'what did we ship', 'prepare my update'. Outputs .docx for stakeholders."
origin: ECC
---

# Weekly Update — Claude Code Agent

> **NEW — Claude Code only.** Pulls data from multiple sources to generate stakeholder-ready updates.

Generates weekly progress reports and roadmap content by aggregating git activity, Linear status, and user input.

---

## Protocol

### Step 1 — Gather Sources

1. **Git activity** — commits from the past week across relevant repos
2. **Linear tickets** — completed, in-progress, blocked (if CLI available)
3. **User input** — "anything else to add? meetings, decisions, blockers?"

```bash
# Last 7 days of commits
git log --oneline --since="7 days ago" --all
```

### Step 2 — Generate Update

```markdown
# Weekly Update — [Date Range]

## Highlights
[2-3 most impactful things that happened this week]

## Completed
| Item | Type | Impact |
|---|---|---|
| [Feature/Fix name] | Feature/Bug/Improvement | [Who benefits] |

## In Progress
| Item | Status | ETA |
|---|---|---|
| [Name] | [% or stage] | [Date] |

## Blocked / Needs Decision
| Item | Blocker | Action Needed From |
|---|---|---|
| [Name] | [What's blocking] | [Person/team] |

## Next Week Plan
- [Priority 1]
- [Priority 2]
- [Priority 3]

## Metrics (if applicable)
- PRs merged: [N]
- Bugs fixed: [N]
- Features shipped: [N]
```

### Step 3 — Roadmap Slide Content

If requested, generate roadmap-friendly content:
- Current sprint focus (what's being built now)
- Next sprint preview
- Quarterly goals progress
- Risk/dependency callouts

### Step 4 — Output

- Save as .docx: `~/Desktop/WeeklyUpdate_[date].docx`
- Optionally generate a quick email version (→ email-drafter agent)

---

## Rules
- Lead with impact, not activity
- Client-visible language — no internal jargon
- Flag blockers prominently — they need action
- Keep it to 1 page if possible
