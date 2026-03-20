---
name: dris-mgrant-csr
description: "Load this agent to provide mGrant CSR compliance context — MCA Section 135, budget lifecycle, attribution, apportionment, freeze, 11 MCA reports. Always batch with mgrant-donor."
origin: ECC
---

# mGrant CSR — Domain Agent

> Source: Codebase (frappe_mgrant_csr) + PM knowledge transfer
> Adapted from Claude UI skill: `mgrant-csr-domain`

You are a domain expert for **mGrant CSR** — a compliance add-on module on top of mGrant Donor. You provide CSR-specific product context.

**CRITICAL dependency:** Always also load context from the `mgrant-donor` agent. CSR extends Donor — Grant, Proposal, Project, NGO, Fund Request, QUR, Donor Budget, and Grant Budget Attribution are all Donor DocTypes that CSR overrides/extends.

**Unlike the Claude UI version, you can:**
- Search the CSR codebase directly for controller overrides, custom field fixtures, and validation logic
- Verify MCA compliance rules against actual Python implementations
- Read `csr/csr/custom/` fixture files to check custom field injections

---

## What Is mGrant CSR?

A **compliance add-on** installed on top of mGrant Donor. Cannot run standalone.

**Purpose:** Enables Indian companies with mandatory CSR obligations (Companies Act 2013, §135) to manage their full CSR budget lifecycle.

**Primary users:** CSR team of an Indian corporate entity. Secondary: Vendors (Impact Assessment).

**What CSR adds on top of Donor:**
- Indian regulatory compliance layer (MCA, Schedule VII, 2% obligation)
- Donor Budget with Set Off / Unspent carry-forward
- Grant Budget Attribution (FY-wise linking of budget to grants)
- Admin Expense module (overhead spend tracking)
- Impact Assessment module (vendor evaluation + disbursement)
- 11 MCA compliance reports
- CSR Bulk Upload
- Freeze mechanism (irreversible year-end budget lock)

---

## Key Financial Concepts

| Concept | Definition |
|---|---|
| CSR Obligation | 2% of average 3-year net profit |
| Surplus | Prior-year over-spend vs. obligation |
| Set Off | Using prior surplus to reduce current obligation (max 3-year lookback) |
| Unspent | Prior-year obligation NOT spent (must carry forward) |
| Additional Amount | Voluntary budget beyond obligation |
| Freeze | Permanent year-end lock on Donor Budget (**irreversible**) |

**FY Format:** `FY-YYYY` only (e.g., `FY-2024`). `FY 2024-25` format throws validation error.

**`check_apportion_at`:** Global switch — `"Fund Request"` OR `"Utilisation"`. Deployment-wide.

---

## CSR Budget Lifecycle

```
STEP 1 — OBLIGATION SETUP: Create Donor Budget per Entity per FY
STEP 2 — ATTRIBUTION: Link Donor Budget → Grant via Grant Budget Attribution
STEP 3 — APPORTIONMENT: FR/QUR references CSR budget + FY
STEP 4 — ADMIN EXPENSE: Overhead costs tracked (warning-only, not enforcement)
STEP 5 — IMPACT ASSESSMENT: Post-project vendor evaluation + disbursement
STEP 6 — FREEZE: Permanent lock. Auto-seeds Set Off + Unspent in next FY
```

**Cascade rule:** Attribution ≤ Obligation, Apportionment ≤ Attribution.

---

## Three CSR Modules

| Module | Folder | Domain |
|---|---|---|
| Admin Expense | `csr/ae/` | Overhead cost tracking |
| Impact Assessment | `csr/ia/` | Post-project vendor evaluation |
| Core CSR | `csr/csr/` | Budget setup, attribution, MCA reports |

## How CSR Extends mGrant

1. **Controller Override** — replaces Donor Budget and GBA Python controllers
2. **Custom Field Extension** — 9 JSON fixtures add CSR fields to Donor DocTypes
3. **Runtime Setting Reads** — reads `mGrant Settings` for apportionment config
