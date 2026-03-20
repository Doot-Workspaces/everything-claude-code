# CLAUDE.md Generator Prompt

Copy the prompt below and paste it into your AI tool at the **start of your first session** on a new repository.

---

## COPY THIS PROMPT

At the end of every coding session, generate or update a file called `CLAUDE.md` in the repository root. This file is the project's living knowledge base — it lets any future AI agent (or human) pick up exactly where you left off.

CLAUDE.md must contain:

### Project Overview
One paragraph: what this project does, who it's for, and the core problem it solves.

### Tech Stack
Language, framework, database, key libraries, package manager, and versions. Be specific (e.g., "Frappe v15, Python 3.11, MariaDB 10.6" — not just "Python").

### Repository Structure
Tree of key directories and files with one-line descriptions. Skip node_modules, __pycache__, and other generated directories.

### What Has Been Built
List of completed features, modules, or components. For each: what it does, which files it touches, and any non-obvious decisions made.

### What Is In Progress
Current feature branch, what you were working on, what's done vs what's left. Include the exact file and function you stopped at.

### Architecture Decisions
Any decisions that a new agent would get wrong without context. Examples: "We use server scripts instead of hooks for X because Y", "Permissions are handled via User Permission not Role, because Z."

### Patterns & Conventions
Coding patterns used in this repo — naming conventions, file organisation, how API endpoints are structured, how tests are written. If the project deviates from framework defaults, say so.

### Environment Setup
How to get this project running locally. Include bench commands, config steps, env variables needed.

### Known Issues & Gotchas
Bugs, workarounds, things that look wrong but are intentional, performance bottlenecks.

### Session Log
Append a dated entry each session:
- Date and what was accomplished
- Files created or modified
- Decisions made and why
- What the next session should pick up

RULES:
- Write for a cold-start AI agent that has zero prior context.
- Be concrete. File paths, function names, exact commands — not vague descriptions.
- Update every session. Stale CLAUDE.md is worse than no CLAUDE.md.
- Raise a PR to commit CLAUDE.md to the repo so it persists.
- This file is stack-agnostic. Adapt sections to whatever language/framework the project uses.

---

*Source: Dhwani RIS Deploy Control Guide v1.0, March 2026*
