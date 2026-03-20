# Skill Authoring Standards — Dhwani RIS AI Hub

How to write, test, and maintain skills that actually work.

## 1. Frontmatter — Get It Right

Every skill file starts with YAML frontmatter. This is what triggers the skill, so precision matters.

```yaml
---
name: kebab-case-name
description: >
  One line. Specific. Answers "when should this load?"
metadata:
  author: your-name
  version: "1.0.0"
  category: product | builder | design | domain
---
```

### Common YAML Mistakes

| Mistake | Wrong | Right |
|---------|-------|-------|
| Unquoted special chars | `description: Design UI/UX` | `description: "Design UI/UX"` |
| Missing version quotes | `version: 1.0.0` | `version: "1.0.0"` |
| Tab indentation | `\t name: foo` | `  name: foo` (2 spaces) |
| Colon in value | `description: Step 1: do X` | `description: "Step 1: do X"` |
| Multi-line without `>` | Long description on one line | Use `>` for folded block |

### Description Writing for Trigger Accuracy

The `description` field is the single most important line. AI tools match user intent against this line to decide whether to load the skill.

**Good descriptions** are:
- Specific: "Audit Frappe DocType forms for WCAG 2.1 AA accessibility compliance"
- Action-oriented: starts with a verb (Design, Review, Create, Audit, Generate)
- Scoped: mentions the domain (Frappe, mGrant, CSR, React Native)

**Bad descriptions** are:
- Vague: "Help with design" (matches everything, useful for nothing)
- Too long: 3+ sentences (tools truncate)
- Too short: "UX skill" (insufficient matching signal)

## 2. Skill Structure

Use the template at `templates/SKILL_TEMPLATE.md`. Every skill must have:

| Section | Required | Purpose |
|---------|----------|---------|
| When to Use (must/recommended/skip) | Yes | Prevents false triggers |
| Protocol (numbered steps) | Yes | The actual workflow |
| Key Rules | Yes | Non-negotiable constraints |
| Output Format | Yes | What the skill produces |
| Examples | Yes | At least one worked example |
| Common Pitfalls | Recommended | What agents get wrong |
| Cross-References | Recommended | Upstream/downstream skills |

## 3. Evals — Test Before You Ship

Every skill should have an `evals.json` companion. Use `templates/evals-template.json` as the starting point.

### Eval Types

| Type | What It Tests | Pass Criteria |
|------|--------------|---------------|
| `trigger` | Does the skill activate on the right prompts? | Activates on positives, stays silent on negatives |
| `output` | Does the output contain required elements? | All `expected_contains` present, none of `expected_not_contains` |
| `quality` | Is the output expert-level? | All quality checks pass |
| `safety` | Does the skill respect safety rules? | Must be 100% — no exceptions |

### Running Evals

Use the `skill-creator:skill-creator` skill or `autoresearch:fix` to:
1. Load the skill
2. Run each eval case
3. Score results
4. Fix failures iteratively

### When to Re-run Evals

- After any edit to the skill file
- After changing the description (trigger accuracy may shift)
- After a user reports a false trigger or missed trigger
- Before merging a skill PR

## 4. Versioning

Follow semver:
- **Patch** (1.0.0 → 1.0.1): Fix typos, clarify wording, no behavior change
- **Minor** (1.0.0 → 1.1.0): Add new sections, expand coverage, new examples
- **Major** (1.0.0 → 2.0.0): Change the protocol, restructure, break existing workflows

Update the version in frontmatter AND add a changelog entry at the bottom of the skill file.

## 5. Deduplication Rules

Before creating a new skill, check:

1. **Does a skill already cover this?** Search existing files by keyword.
2. **Can an existing skill be extended?** Add a section instead of a new file.
3. **Is this a different perspective on the same thing?** Merge, don't multiply.

The 16→6 design skills compression taught us: overlapping skills confuse AI tools and waste context window.

**Rule of thumb:** If two skills share >30% of their content, merge them.

## 6. Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| File name | `kebab-case.md` | `csr-compliance.md` |
| Skill name (frontmatter) | `kebab-case` | `csr-compliance` |
| Folder | `category-skills/` | `domain-skills/` |
| Eval file | `skill-name.evals.json` | `csr-compliance.evals.json` |

## 7. Categories

| Category | Folder | Who Uses It |
|----------|--------|------------|
| Product | `product-skills/` | PMs, analysts, delivery |
| Builder | `builder-skills/` | Developers, vibe coders |
| Design | `design-skills/` | UI/UX, frontend, accessibility |
| Domain | `domain-skills/` | Anyone needing sector knowledge |

## 8. Quality Checklist

Before submitting a skill PR, verify:

- [ ] Frontmatter parses without YAML errors
- [ ] Description is specific and action-oriented
- [ ] "When to Use" has must/recommended/skip sections
- [ ] Protocol has numbered steps
- [ ] At least one worked example included
- [ ] Key rules section is present
- [ ] Output format is defined
- [ ] No duplicate content with existing skills
- [ ] evals.json exists with at least 3 test cases
- [ ] Trigger tests pass (positive and negative)
- [ ] Safety tests pass at 100%
- [ ] Version number is set
- [ ] File follows naming conventions

## 9. Autoresearch Integration

For complex skills, use `autoresearch` to iteratively improve:

```
/autoresearch:fix — Fix skill eval failures one at a time
/autoresearch:scenario — Generate edge case scenarios for the skill
/autoresearch:security — Audit skill for prompt injection vulnerabilities
```

Run these after initial authoring, not before. Write the skill first, then refine.

## 10. Cross-Referencing

Skills work best in chains. Document the chain in each skill:

```
upstream: feature-spec → THIS SKILL: frappe-ui-designer → downstream: frappe-engineer
```

When a skill references another, use the exact file name so agents can load it.
