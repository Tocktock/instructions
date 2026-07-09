# Prompt: Goal-Ready Architecture and Test-Scenario Implementation Document v7

Use the supplied conversation, documents, decisions, and explicitly verified evidence to create a compact, evidence-grounded implementation document that can be used directly with Codex or Claude `/goal` workflows.

The document must do three things well:

1. Explain the minimum architecture and rationale needed to understand why the solution should work.
2. Convert requirements, risks, and failure modes into verifiable acceptance scenarios.
3. Produce a compact **Goal Execution Brief** that a downstream agent can follow without micro-slicing.

This is a reasoning framework, not a form. Adapt terminology, depth, and verification to the actual problem. Omit irrelevant sections and avoid repeating facts across sections.

---

## 0. Production Workflow

Author in one forward pass with one consolidation review at the end.

1. **Ingest once:** read all supplied material before writing. Build a private working map of source authority, problems, requirements, candidate scenarios, open decisions, and evidence gaps.
2. **Frame once:** write the problem, root causes, architecture model, and requirement set in a single pass.
3. **Design scenarios as a set:** create the scenario index first. It is the coverage contract. Add detailed narratives only for scenarios that are high-risk, cross-boundary, disputed, irreversible, or otherwise hard to verify.
4. **Plan in work packages:** group scenarios by coherent execution and verification surface, not by smallest steps.
5. **Write the Goal Execution Brief:** summarize outcome, scope, package plan, verification surfaces, stop conditions, and evaluator-visible evidence requirements.
6. **Consolidate once:** challenge package boundaries, missing scenarios, evidence strength, ID integrity, vocabulary consistency, and final verdict strength.

Ownership rule: state each fact once in the section that owns it and reference by ID elsewhere.

---

## 1. Working Rules

### Evidence discipline

Use only these claim labels for statements, requirements, and scenario evidence status:

`VERIFIED`, `SPECIFIED`, `PROPOSED`, `UNVERIFIED`, `BLOCKED`, `NOT_APPLICABLE`, `HUMAN_REQUIRED`, `EXTERNAL_ENVIRONMENT`.

Use only these execution statuses for checks actually run:

`PASS`, `FAIL`, `NOT_RUN`, `BLOCKED`, `NOT_APPLICABLE`.

Rules:

- Base factual claims only on supplied material or explicitly verified evidence.
- Do not upgrade documentation, schemas, mocks, examples, static inspection, or local tests into stronger readiness claims.
- Preserve the source domain's vocabulary. Do not invent technologies, roles, entities, boundaries, metrics, or deployment details.
- Label assumptions and open questions. Continue with labeled assumptions when useful work is not blocked.
- Keep distinct: current fact, stakeholder requirement, recommendation, unresolved decision, test result, readiness claim.

### Proportionality

Focus detail on behavior that materially affects value, correctness, safety, compatibility, operability, or delivery risk. Consolidate related requirements and scenarios when separation would create repetition. The index carries breadth; narratives carry depth.

---

## 2. Problem and Architecture

Identify:

- Goal and intended user/system outcome.
- Current situation and observable problems.
- Root causes, not only requested changes.
- Scope, constraints, dependencies, non-goals, and source authority.
- Strongest readiness claim currently supported by evidence.

For each important problem, connect:

```text
symptom -> root cause -> consequence -> requirement/design response -> acceptance scenario
```

Explain only the necessary conceptual model:

- Core concepts and responsibilities.
- Lifecycle and boundary crossings.
- Current state, target direction, gaps, and minimum correction.
- Invariants that must hold.
- Trade-offs that could change the recommended design.

---

## 3. Requirements, Assumptions, Non-Goals, and Decisions

Classify material into:

- **Explicit requirements:** stated by stakeholders or authoritative sources.
- **Derived requirements:** necessary for stated constraints, invariants, safety, consistency, compatibility, or operability.
- **Assumptions:** needed but not confirmed.
- **Non-goals:** intentionally excluded.
- **Decisions:** already made or must be frozen before implementation.

Use stable IDs only when they improve traceability. For each material requirement, include concise statement, source/rationale, acceptance implication, and uncertainty.

---

## 4. Scenario Set

Design scenarios as a whole from requirements, invariants, risks, and failure modes.

Keep separate:

- **Priority:** `MUST_PASS`, `REGRESSION`, `OPTIONAL`.
- **Verification mode:** `AUTOMATED`, `HUMAN_REQUIRED`, `EXTERNAL_ENVIRONMENT`, or combined.
- **Current evidence status:** a claim label from §1.

### Scenario Index

| Scenario ID | Priority | Scenario | Risk Addressed | Related Requirements | Verification Mode | Evidence Status |
| --- | --- | --- | --- | --- | --- | --- |

The index should show coverage and gaps without duplicating full prose.

### Detail tiers

Default to the lowest sufficient tier:

1. **Index-only:** simple, low-risk, obvious verification.
2. **Compact row:** one Given/When/Then plus expected evidence, grouped with related scenarios.
3. **Full narrative:** only for `MUST_PASS` scenarios that are complex, risky, disputed, cross-boundary, or irreversible.

For full narratives, use only relevant fields:

```markdown
### SC-001 — Scenario name
- Intent / risk addressed:
- Priority / verification mode / evidence status:
- Related requirements:
- Preconditions:
- Given / When / Then:
- Observable result:
- Boundary / responsibility crossed:
- State changes and invariants:
- Edge and failure cases:
- Verification class and PASS evidence:
- Implementation implications:
```

Scenario rules: outcomes must be observable and falsifiable; test the real behavior or state transition; avoid vague claims like “works correctly”; do not present local/simulated proof as live proof.

---

## 5. Implementation Plan — Work Packages

Organize implementation into dependency-ordered work packages. A package is the largest group of related changes that can be implemented as one change-set and verified in one pass.

Group by shared subsystem, user flow, code path, state transition, root cause, interface, migration, or verification surface.

Split only on a trigger:

- irreversible/destructive action
- security, authority, privacy, payment, or retention boundary
- unresolved decision
- unknown root cause requiring investigation
- human or external gate in the middle of the flow
- verification methods that cannot run together

For each package, include:

- Package ID and goal.
- Included scenarios by ID.
- Dependencies or decisions to resolve first.
- Required changes at the level of responsibilities, state, interfaces, validation, processing, user surfaces, integrations, or operations.
- Migration, compatibility, rollout, rollback, observability, and failure handling when relevant.
- Exit evidence required per included scenario.
- Strongest readiness claim the package can support.

Identify shared foundations once rather than repeating them under every package.

---

## 6. Verification Classes, Completion, and Verdicts

Define each verification class once. Scenarios and packages reference the class by name.

For each class, state:

- What it proves.
- What it does not prove.
- Required environment or dependency.
- Required evidence.
- Whether human or external participation is needed.

Completion rules:

- Every applicable `MUST_PASS` and `REGRESSION` scenario must pass with scenario-specific evidence.
- Package-level or suite-level green does not by itself pass any individual scenario.
- Required human or external verification must either pass or remain explicitly gated.
- The verdict must not be stronger than the weakest applicable required scenario or unresolved gate.
- Separate document readiness from implementation readiness.

---

## 7. Goal Execution Brief

Include this compact block near the end. It should be concise enough to paste into or attach beside a `/goal` prompt.

```markdown
## Goal Execution Brief
Outcome:
Source of truth:
Scope:
Non-goals:
Human/external gates:
Work packages:
- WP-001: goal; scenarios; verification surface; exit evidence
- WP-002: goal; scenarios; verification surface; exit evidence
Split triggers:
Verification classes/checks:
Evaluator-visible evidence required:
Stop/block conditions:
Final verdict rule:
```

The brief must support goal-mode evaluation: it should state measurable completion criteria, checks the downstream agent should run, constraints that must remain intact, and the exact evidence that must be surfaced in package checkpoints and the final verdict.

---

## 8. Final Consolidation

Run one consolidation pass at the end.

Planner challenge:

- Are root causes, requirements, scenarios, and packages aligned?
- Are packages too small, too broad, or crossing split triggers?
- Are obvious low-risk scenarios demoted and high-risk scenarios given enough depth?

Evaluator challenge:

- Can a downstream `/goal` evaluator decide completion from the surfaced evidence?
- Does every required scenario have direct evidence requirements?
- Are proof boundaries, assumptions, human/external gates, and blocked items explicit?
- Does the final verdict overclaim?

Mechanical checks:

- Every referenced ID resolves once.
- Claim labels and execution statuses match §1 exactly.
- Verification classes are defined once and referenced elsewhere.
- The Goal Execution Brief is consistent with the scenario index, packages, and completion rules.

---

## Recommended Output Structure

Adapt as needed:

```markdown
# Title and Status
# Executive Summary
# Scope, Sources, and Evidence Status
# Problems and Root Causes
# Architecture Model
# Requirements, Assumptions, Non-Goals, and Decisions
# Scenario Set
# Implementation Plan — Work Packages
# Verification Classes, Completion, and Verdicts
# Goal Execution Brief
# Risks, Open Decisions, and Questions
```

---

## Source Material

Paste or attach the conversation, requirements, architecture notes, implementation evidence, and stakeholder decisions below this line.
