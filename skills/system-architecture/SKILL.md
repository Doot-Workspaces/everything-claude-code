---
name: system-architecture
description: "Systematic architectural thinking through five pillars — domain modeling, systems thinking, constraint navigation, AI-aware decomposition, and AI-first development."
origin: ECC
---

# System Architect

Guide systematic architectural thinking through five pillars: domain modeling, systems thinking, constraint navigation, AI-aware decomposition, and AI-first development.

**Core principle**: The "correct" technical solution is often unshippable. Architects navigate the gap between idealized examples and messy reality.

## When to Use

Activate when detecting:
- Architecture or system design discussions
- Technology choice decisions
- Multi-component or integration planning
- Breaking change or migration discussions
- Compliance, regulation, or security concerns
- Team boundary or dependency discussions
- AI tool evaluation or agentic workflow design

## The Five Pillars

### 1. Domain Modeling

Understand the actual problem space before discussing solutions.

**Ask:**
- What does [domain term] actually mean in your context?
- What happens in the edge case where [unusual scenario]?
- Who are the actual users? What do they care about?
- What makes this domain different from the standard pattern?

**Rule**: Complete domain discovery before ANY technical discussion.

### 2. Systems Thinking

Understand how components interact and where failures hide.

**Ask:**
- What happens when this component fails?
- What are the upstream/downstream dependencies?
- Who gets paged at 3 AM when this breaks?
- What changed recently that we didn't control?

**Rule**: Draw the system diagram. Identify every external dependency. Ask: "What if this disappears tomorrow?"

### 3. Constraint Navigation

Surface all real-world constraints before proposing solutions.

**Constraint types:**
- **Technical**: Legacy systems, performance requirements, existing data formats
- **Organizational**: Team boundaries, approval chains, who has context vs authority
- **Business**: Budget, timeline, compliance, vendor contracts
- **Political**: VP ownership, team API gatekeeping, legal approval gaps

**Rule**: Surface constraints BEFORE proposing solutions. A shippable 70% solution beats an unshippable perfect solution.

### 4. AI-Aware Decomposition

Break problems into chunks that AI can reliably solve.

**Good AI task boundaries:**
- Clear input/output contract
- Bounded context (AI has all needed info)
- Verifiable results (tests can validate)
- Failure isolation (one chunk failing doesn't cascade)

**Bad AI task boundaries:**
- "Make it better" (no clear output)
- "Fix the bugs" (unbounded scope)
- "Refactor the system" (too large, too vague)

**After AI solves chunks**, someone must: verify each chunk works, integrate them, handle gaps between chunks, ensure overall coherence.

### 5. AI-First Development

Evaluate whether modern tools genuinely benefit the project.

**Questions to ask:**
- Would multi-agent orchestration simplify this workflow?
- Could edge/local LLMs handle this for lower latency/cost?
- Is this a candidate for agentic workflows vs traditional request-response?
- Could self-learning loops make this smarter over time?
- What automated tests ensure every feature works?

**Default to simplicity**, but don't ignore genuine improvements.

## Key Rules

- Complete domain discovery before ANY technical discussion
- Surface all constraints (technical, organizational, business, political) before proposing solutions
- A shippable 70% solution beats an unshippable perfect solution
- Every AI task must have clear input/output contracts and verifiable results
- Default to simplicity -- only recommend advanced tooling (multi-agent, edge inference) when genuinely beneficial
- Never propose architecture without explicit trade-offs
- Always ask the 20 questions (or relevant subset) before recommending an approach

## Output Format

```
Architecture Decision: [title]

Domain: [1-2 sentence problem summary]
Constraints: [list of fixed and flexible constraints]
Dependencies: [external systems, teams, approvals]

Recommended Approach:
  [description]

Trade-offs:
  + [advantage 1]
  + [advantage 2]
  - [disadvantage 1]
  - [disadvantage 2]

Alternative Considered:
  [brief description and why it was not chosen]

AI Decomposition (if applicable):
  Task 1: [description] -- Input: X, Output: Y, Verification: Z
  Task 2: [description] -- Input: X, Output: Y, Verification: Z

Next Steps:
  1. [action item]
  2. [action item]
```

## Examples

**Example 1 -- Integration planning:**
```
Architecture Decision: mGrant-to-ERP Fund Sync

Domain: Grant fund disbursements in mGrant must sync to ERPNext journal entries
Constraints:
  - Fixed: ERPNext API rate limit (60 req/min), FCRA compliance (audit trail required)
  - Flexible: Sync frequency (real-time vs batch), error notification channel

Recommended Approach: Batch sync every 15 minutes via scheduled job, with idempotent writes and a dead-letter queue for failures.

Trade-offs:
  + Simple, debuggable, within rate limits
  + Full audit trail via sync log DocType
  - 15-minute delay between mGrant action and ERP reflection
  - Requires monitoring the dead-letter queue

Alternative Considered: Real-time webhook -- rejected due to rate limit risk and harder error recovery.
```

**Example 2 -- AI task decomposition:**
```
Architecture Decision: AI-Assisted Grant Report Generation

AI Decomposition:
  Task 1: Extract structured data from grant DocType -- Input: docname, Output: JSON with financials + milestones, Verification: schema validation
  Task 2: Generate narrative summary -- Input: structured JSON, Output: 500-word summary, Verification: human review before send
  Task 3: Format into PDF template -- Input: summary + data, Output: PDF, Verification: visual spot-check

Human checkpoints: After Task 2 (review narrative), After Task 3 (approve final PDF)
```

## The Process

| Phase | Goal | Output |
|-------|------|--------|
| 1. Domain Discovery | Understand the problem | Domain model, shared vocabulary |
| 2. Systems Analysis | Map interactions and failures | Dependency map, failure modes |
| 3. Constraint Mapping | Surface all constraints | Constraint matrix (fixed vs flexible) |
| 4. AI Decomposition | Break into solvable chunks | Task decomposition with boundaries |
| 5. Solution Synthesis | Propose with tradeoffs | Recommended approach with explicit tradeoffs |

## 20 Questions Before Any Architecture

**Domain:**
1. What problem are we actually solving?
2. Who are the real users and what do they need?
3. What domain-specific constraints exist?

**Systems:**
4. What external dependencies exist?
5. How does this fail? What breaks what?
6. Who monitors this? Who gets paged?

**Constraints:**
7. What legacy systems must we integrate with?
8. Who needs to approve this?
9. What's the budget constraint?
10. What compliance/regulatory requirements apply?
11. What can't we change, even if it's wrong?

**AI Decomposition:**
12. What are the discrete, bounded tasks?
13. How do we verify each chunk?
14. Where do humans need to make judgment calls?

**AI-First:**
15. Would modern tools (Rust/WASM, multi-agent) benefit this?
16. Could edge inference improve latency/privacy?
17. Is this a candidate for agentic workflows?
18. Could self-learning loops improve this over time?
19. What automated testing ensures every feature works?
20. Would end users benefit from AI-enhanced skills?

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Jumping to technical solutions | Complete domain discovery first |
| Ignoring constraints | Map constraints before proposing solutions |
| Treating external APIs as stable | Map all external dependencies, plan for breakage |
| Unbounded AI tasks | Define clear input/output contracts |
| No human checkpoints | Insert verification between AI chunks |
| Ignoring politics | Ask about team boundaries, approval chains |
| Premature optimization | Design for actual scale, not hypothetical millions |

## Spec-Driven Development (Optional Extension)

For greenfield systems or quality-critical projects:

```
Phase 1: CONSTITUTION  -> Human defines unbreakable rules
Phase 2: BLUEPRINT     -> Human approves architecture
Phase 3: SUPERHUMAN    -> AI executes with machine precision
```

**Constitution**: Rules that cannot be violated (types, schemas, tests, documented invariants).

**Blueprint**: Hierarchical spec from requirements to atomic tasks, each with input/output contracts and verification commands.

**Superhuman output**: Code so consistent that deviations are visible, patterns are learnable, verification is automatable.

**When to use SDD**: Greenfield with clear requirements, quality-critical systems, regulated industries.
**When to skip**: Prototyping, unclear requirements, small single-dev projects.
