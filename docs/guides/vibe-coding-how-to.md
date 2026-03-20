# Vibe Coding at Dhwani RIS — How-To Guide

> **What is vibe coding?** Using AI tools (like Claude Code or Codex) to write, edit, and ship code — without being a developer yourself. You describe what you want; the AI writes the code.

---

## Before You Start — Checklist

- [ ] You have access to platform.dhwaniris.in (Deploy Control)
- [ ] You have your repo name confirmed with a developer
- [ ] You have `CLAUDE.md` set up in your repo (or ask a dev to create it)
- [ ] You have `Security_DRIS.md` ready to attach to your session
- [ ] You are working on a **feature branch** — never on `main` or `develop`
- [ ] You know which tool to use: Claude Code (CLI) or Codex (see below)

---

## Getting a GitHub Token

You need a GitHub token to let the AI push code to a repo. You do not need a personal GitHub account.

1. Log in at [platform.dhwaniris.in](https://platform.dhwaniris.in)
2. Click the **GitHub Token** tab
3. Enter the **exact repo name** (confirm this with your developer — there is no validation)
4. Click **Generate Scoped Token**
5. Copy the token immediately — it expires in **1 hour**
6. Paste it into your AI tool session when prompted

**Token details at a glance:**

| Detail | Value |
|---|---|
| Lifetime | 1 hour |
| Scope | Single repo only |
| Permissions | Read + Write |
| Service account | dhwani-jenkins |

---

## The Two Key Files — CLAUDE.md and Security_DRIS.md

**CLAUDE.md** is the AI's memory for your project. It lives in the root of your repo and stores project context, tech stack, and past decisions. Without it, the AI starts from scratch every session.
- Update it at the end of every session
- Raise a PR to commit it into the repo
- If it does not exist yet, ask a developer to create it

**Security_DRIS.md** contains Dhwani RIS's security rules for AI-generated code. Attach it to every session, no exceptions. Without it, the AI has no guardrails and may produce insecure code.

**How to attach both files:** In your Claude Code or Codex session, upload or reference both files at the start before giving any instructions.

---

## Ways to Do Vibe Coding

### Claude Code (CLI)
- Runs in a terminal on your laptop or a server
- Full access to your repo, can read and write files directly
- Best for: building features, refactoring, complex multi-file tasks
- Requires: terminal access, repo cloned locally or on a server

### Claude Code (via Claude.ai)
- Use the Claude.ai web interface with a project or attached files
- Good for: drafting code, reviewing logic, planning before you build
- Does not push code directly — you hand off output to a developer

### Codex (OpenAI)
- OpenAI's code-focused model, accessible via API or ChatGPT
- Good for: quick one-off scripts, Python utilities, standalone tasks
- Use when Claude Code is unavailable or for second opinions

### Claude Code vs Codex — When to Use Which

| Situation | Use |
|---|---|
| Building a Frappe feature or doctype | Claude Code |
| Complex multi-file changes in a repo | Claude Code |
| Quick Python script or data task | Either |
| You want a second opinion on code | Codex |
| Working within the Dhwani RIS repo workflow | Claude Code |

---

## How to Write Better Vibe Code

Good instructions produce good code. The AI is only as good as what you give it.

- **Be specific.** Instead of "add a field," say "add a Currency field called `sanctioned_amount` to the Grant doctype, visible in the main form, not mandatory."
- **Provide context.** Paste error messages, describe what already exists, mention the doctype or module you are working in.
- **One task at a time.** Break large requests into smaller steps. Review the output before asking for the next step.
- **Always reference CLAUDE.md.** It gives the AI the project context it needs without you having to explain everything each time.
- **Ask the AI to explain.** If something looks off, ask it to walk you through what it did. You do not need to understand the code — but you should understand the intent.

---

## How to Learn and Where to Get Reviews

- **Start small.** Ask the AI to make a simple change in a non-critical area. Review the PR with a developer. Build confidence before tackling bigger tasks.
- **Use developer reviews every time.** All code produced by AI must go through a pull request. A developer reviews and merges — never push directly.
- **Ask your developer to pair with you** on your first 2-3 sessions. This is the fastest way to learn what good output looks like.
- **Resources:** Anthropic's Claude documentation, OpenAI Codex docs, and internal sessions shared by the team.

---

## Common Mistakes

- **Generating a token before confirming the repo name.** There is no validation — a wrong name will silently fail or push to the wrong place.
- **Saving or reusing an old token.** Tokens expire in 1 hour. Always generate a fresh one.
- **Skipping Security_DRIS.md.** The AI will not follow Dhwani RIS security standards unless you attach it.
- **Not updating CLAUDE.md at the end of a session.** The next session starts from scratch and you lose all context.
- **Pushing directly to main or develop.** Always work on a feature branch. Push, raise a PR, wait for dev review.
- **Giving the AI a vague instruction and accepting the first output.** Review what it produces. If it does not match what you wanted, correct the instruction and try again.
