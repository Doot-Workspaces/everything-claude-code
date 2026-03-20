---
name: dris-frappe-doctype
description: "Design and spec Frappe DocTypes with fields, permissions, workflows, and approval chains. Produces code-ready JSON fixtures. Invoke for 'design a DocType', 'spec the fields', 'workflow planning'."
origin: ECC
---

# Frappe DocType & Workflow — Claude Code Agent

> Adapted from Claude UI skill: `frappe-doctype-skill`
> **Claude Code upgrade:** Can read existing DocType JSON files, verify field types against Frappe conventions, check for naming conflicts, and generate the actual DocType JSON fixture files.

Designs complete, code-ready DocType specs. If you can fill in this spec completely, a developer can build it without a single clarifying question.

---

## DocType Types

1. **Regular (Master)** — Reference/entity data (NGO, Donor, Employee)
2. **Regular (Transactional)** — Event records (Grant Application, Leave Request). Often submittable.
3. **Submittable** — Three-state lifecycle: Draft(0) → Submitted(1) → Cancelled(2)
4. **Child (Table)** — Lives inside parent DocType as rows. `"istable": 1`
5. **Single** — One record only. Global settings/config. `"issingle": 1`
6. **Tree (Nested Set)** — Parent-child hierarchies. `"is_tree": 1`

## Field Type Reference

**Data/Text:** Data, Small Text, Text, Long Text, Text Editor, Markdown Editor, Code, Password
**Numbers:** Int, Float, Currency, Percent, Rating, Duration
**Date/Time:** Date, Datetime, Time
**Links:** Link, Dynamic Link, Table, Table MultiSelect
**Selection:** Select, Check (boolean), Color, Geolocation
**Layout:** Section Break, Column Break, Tab Break, HTML
**Attachments:** Attach, Attach Image

## Standard Auto-Fields

Every DocType gets: `name`, `owner`, `creation`, `modified`, `modified_by`, `docstatus`, `idx`, `parent`, `parenttype`, `parentfield`

---

## Spec Template

For every DocType being designed, produce:

```
DOCTYPE SPEC: [Name]
Type: Regular / Submittable / Child / Single / Tree
Module: [module name]
Naming: [autoname rule]

FIELDS:
fieldname | label | fieldtype | reqd | options | depends_on | notes

PERMISSIONS:
role | read | write | create | delete | submit | cancel | amend

WORKFLOW (if submittable):
state | allowed_roles | next_states | closure_type

VALIDATIONS:
condition | error_message | when_triggered

AUTO-ACTIONS:
trigger | action | target
```

---

## Claude Code Execution

Unlike the Claude UI version:
1. **Search existing DocTypes** in the codebase before designing — avoid conflicts
2. **Generate the actual JSON fixture** file for the DocType definition
3. **Generate the Python controller** skeleton with validate/on_submit hooks
4. **Generate the JS client script** with frm.trigger setup
5. **Verify** field naming against Frappe conventions (snake_case, no reserved words)
6. **Hand off** to `frappe-engineer` agent for implementation
