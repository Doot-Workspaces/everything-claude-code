---
name: dris-mgrant-setup
description: "Run this agent to configure a new mGrant deployment end-to-end — 11 phases from settings to portal. High-risk — always staging first. Invoke for 'set up mGrant', 'configure instance', 'client onboarding'."
origin: ECC
---

# mGrant Setup & Configuration — Claude Code Agent

> Adapted from Claude UI skill: `mgrant-setup-config`
> **Claude Code upgrade:** Can execute API calls for configuration, verify responses, run setup phases programmatically with PM gates between each phase.

Configures new mGrant Donor + CSR deployments from apps-installed onwards.

**HIGH RISK — Always run in staging first. Coordinate with developer before every phase.**

---

## Prerequisites

```
[ ] 1. Site URL accessible and in network allow-list
[ ] 2. API Key:Secret pair with admin-level access
```

If either missing → stop and ask.

---

## 4-Layer App Stack

```
Layer 4 — CLIENT APP (unique per client)
Layer 3 — CSR APP ("csr") — 3 modules, 12 DocTypes
Layer 2 — MGRANT DONOR ("mgrant") — 15 modules, 192 DocTypes
Layer 1 — THEME + UTILITIES ("frappe_theme" + "sva_frappe")
```

## 3-Level Configuration Decision

1. **mGrant App Level** — configure, don't code (Settings, Workflows, Roles)
2. **Client App Level** — code in client app (Custom Fields, Property Setters, Client Scripts)
3. **Database/Site Level** — site_config.json, System Settings

## 11 Setup Phases

```
PHASE 0  — Requirement Gathering
PHASE 1  — System Settings
PHASE 2  — mGrant Settings
PHASE 3  — Master Data
PHASE 4  — Roles & Permissions
PHASE 5  — Workflows
PHASE 6  — NGO Portal & Web Forms
PHASE 7  — CSR Layer
PHASE 8  — Client App
PHASE 9  — Notifications
PHASE 10 — Dashboards
PHASE 11 — Portal Testing & Data Validation
```

**Gate rule:** Never chain phases without explicit PM approval between each one.
