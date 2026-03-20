---
name: dris-release-notes
description: "Generate release notes from git log and Linear tickets. Outputs internal .docx + customer email draft. Invoke for 'write release notes', 'what shipped', 'release announcement'."
origin: ECC
---

# Release Notes — Claude Code Agent

> **NEW — Claude Code only.** Automates one of Nihaan's most repetitive tasks.

Generates release notes by reading git history and Linear tickets, then produces:
1. Internal release notes (.docx)
2. Customer-facing email draft (.docx)

---

## Protocol

### Step 1 — Gather Context

Ask:
- Release version or sprint name?
- Date range? (or "since last release")
- Which repo(s)?
- Audience: internal team, specific client, or all clients?

### Step 2 — Pull Data

```bash
# Git log since last tag/release
git log --oneline --since="[date]" --until="[date]"

# Detailed changes
git log --pretty=format:"%h %s (%an)" --since="[date]"
```

If Linear is connected, also pull completed tickets for the sprint.

### Step 3 — Categorize Changes

Group into:
- **New Features** — new capabilities added
- **Improvements** — enhancements to existing features
- **Bug Fixes** — issues resolved
- **Technical** — infrastructure, performance, refactoring (internal only)

### Step 4 — Generate Internal Release Notes

```
# Release Notes — [Version/Sprint] — [Date]

## Highlights
[2-3 sentence summary of most impactful changes]

## New Features
- [Feature name]: [One-line description] ([ticket link])

## Improvements
- [Description] ([ticket link])

## Bug Fixes
- [Description] ([ticket link])

## Technical Changes (Internal)
- [Description]
```

Save as: `~/Desktop/ReleaseNotes_[version]_[date].docx`

### Step 5 — Generate Customer Email

Translate internal notes into customer-friendly language:
- Remove technical jargon
- Focus on user impact ("You can now..." instead of "Added endpoint for...")
- Highlight the 2-3 most impactful changes
- Professional tone matching Nihaan's communication style

Save as: `~/Desktop/ReleaseEmail_[version]_[date].docx`

### Step 6 — Review

Present both documents. Ask: "Review and send, or adjust anything?"
