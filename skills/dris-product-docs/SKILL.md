---
name: dris-product-docs
description: "Create structured product documentation chapter-by-chapter for Frappe Wiki. Invoke for 'write docs', 'create a user guide', 'document this module'. Outputs .md files directly."
origin: ECC
---

# Product Docs — Claude Code Agent

> Adapted from Claude UI skill: `product-docs-skill`
> **Claude Code upgrade:** Can search codebase for feature details, read DocType schemas for field reference, write .md files directly to a docs folder, and generate complete wiki structures.

Creates product documentation for Frappe Wiki. Interview → chapter tree → write chapter-by-chapter.

---

## Protocol

### Step 1 — Interview

```
DOCUMENTATION INTAKE:

PRODUCT BASICS
1. Product name?
2. One sentence — what does it do?
3. Primary users? (roles, technical level)
4. New docs or updating existing?

SCOPE
5. Main modules or feature areas?
6. Modules to exclude?
7. Most important workflow?

AUDIENCE & TONE
8. Reader technical level? (End user / Power user / Admin / Developer)
9. Tone? (Formal / Friendly / Technical)
10. Internal or public-facing?

OUTPUT
11. Chapters by module or by user journey?
12. Getting Started chapter?
13. Glossary section?
14. Existing notes to start from?
```

**Claude Code bonus:** If product is mGrant, invoke domain agent and auto-answer questions 2, 5, 7 from codebase.

### Step 2 — Build Chapter Tree

Present the complete documentation map before writing anything. Get user approval.

### Step 3 — Write Chapter by Chapter

For each chapter:
1. Announce which chapter is being written
2. Write full chapter using Frappe Wiki Markdown
3. End with helper audit (notes, warnings, tips, screenshots needed)
4. **Save each chapter as a .md file** in user-specified folder
5. Get approval before proceeding to next

### Step 4 — Output

- Each chapter saved as `[nn]-[chapter-slug].md`
- Complete tree saved as `_sidebar.md` for Frappe Wiki
- All files in a single docs folder ready for Wiki import
