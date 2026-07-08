# Prompt: Adaptive Architecture and Test-Scenario Implementation Document (v6)

Use the supplied conversation, interview, documents, decisions, and explicitly verified evidence to create an **evidence-grounded architecture and test-scenario implementation document**.

The document must do two things well:

1. Explain the essential concepts, responsibilities, lifecycle, current gaps, target direction, and design rationale so a new contributor understands **why** the solution should work this way.
2. Turn the important requirements, decisions, risks, and failure modes into implementation work and verifiable acceptance scenarios.

This is a reasoning framework, not a form to fill mechanically. Adapt the terminology, depth, structure, and verification methods to the actual problem. Omit or combine irrelevant sections, and do not repeat the same content merely to satisfy a template.

---

## 0. Production Workflow

Author the document in one forward pass with a single consolidation review at the end. Do not loop per scenario or per section, and do not re-review after every section.

1. **Ingest once:** read all supplied material before writing anything. Build a working map: problems, source authority, candidate requirements, candidate scenarios, open questions.
2. **Frame once:** write the problem/architecture material (§2) and the requirement set (§3) each in a single pass.
3. **Design the scenario set as a whole (§4):** complete the scenario index first — it fixes coverage, priorities, and evidence status. Then write detailed narratives only for scenarios that qualify for the full tier, drafted in batches grouped by subsystem or flow.
4. **Plan in work packages (§5):** group implementation by coherent surface, not smallest steps.
5. **Consolidate once (§7):** run the final checklist, verify ID integrity and vocabulary consistency, and reconcile verdicts.

Ownership rule: state each fact once, in the section that owns it, and reference it elsewhere by ID. Never restate source material, repeat a rationale across sections, or re-describe a verification method a class definition already covers.

---

## 1. Working Rules

### Evidence discipline

Two vocabularies, used consistently and never mixed:

**Claim labels** — for statements, requirements, and scenario evidence status:

- **VERIFIED:** supported by observed implementation or executed evidence.
- **SPECIFIED:** required or described by an authoritative source, but not verified as implemented.
- **PROPOSED:** a recommendation or design inference.
- **UNVERIFIED:** evidence is insufficient.
- **BLOCKED:** evidence cannot currently be gathered (missing access, dependency, or material).
- **NOT_APPLICABLE:** the claim or scenario does not apply in this scope.
- **HUMAN_REQUIRED / EXTERNAL_ENVIRONMENT:** verification depends on subjective judgment, restricted access, a real environment, or an external party. These are verification modes, never business priorities.

**Execution statuses** — only for checks actually run: `PASS`, `FAIL`, `NOT_RUN`, `BLOCKED`, `NOT_APPLICABLE`.

Do not invent synonyms for either set.

Rules:

- Base factual claims only on supplied material and explicitly verified evidence.
- Do not upgrade documentation, schemas, mocks, examples, static inspection, or local tests into stronger implementation or readiness claims.
- When sources conflict, explain the conflict and the authority rule used. Leave it unresolved when evidence does not support a responsible choice.
- Preserve the source domain's vocabulary. Do not introduce technologies, architectural patterns, domain objects, roles, or organizational boundaries merely because they are common elsewhere.
- Label necessary assumptions and open questions. Do not compensate for missing evidence with invented precision.
- When missing information does not block useful analysis, continue with clearly labeled assumptions rather than stalling or fabricating.

### Proportionality

- Focus on behavior that materially affects value, correctness, safety, compatibility, operability, or delivery risk.
- Give more detail to high-impact, uncertain, disputed, irreversible, or cross-boundary behavior.
- Consolidate related requirements and scenarios when separation would create repetition rather than clarity.
- Do not target a fixed number of pages, concepts, scenarios, diagrams, or implementation tasks.
- Use diagrams only when they explain a lifecycle or responsibility boundary better than prose or a table.
- Include negative evidence only where proving that something **did not happen** is important.
- Prefer the smallest complete design that satisfies the applicable scenarios. Avoid speculative platform design beyond the supplied scope.
- The index carries breadth; narratives carry depth. Depth spent on an obvious scenario is depth stolen from a risky one.

Keep these distinct throughout the document:

```text
current fact | stakeholder requirement | recommendation | unresolved decision | test result | readiness claim
```

---

## 2. Establish the Problem and Architecture

Begin by identifying:

- The goal and intended user or system outcome.
- The current situation and observable problems.
- Root causes, not only requested features.
- Scope, constraints, dependencies, and non-goals.
- The source hierarchy or authority model, when multiple sources exist.
- The strongest maturity or readiness claim currently supported by evidence.

For each important problem, connect:

```text
symptom -> root cause -> consequence -> requirement or design response -> acceptance scenario
```

Then explain the minimum conceptual and architectural model needed to implement the solution.

### Core concepts and responsibilities

For each important concept, explain as applicable:

- What it means in this system.
- Why it exists and which failure it prevents.
- Who or what owns it.
- What authority or responsibility it has.
- What it must not decide, contain, or bypass.
- Its relationship to adjacent concepts.
- Its claim label (VERIFIED, SPECIFIED, PROPOSED, UNVERIFIED).

### Lifecycle and boundaries

Describe the meaningful end-to-end flow using only stages relevant to the source problem. At important transitions, identify:

- Preconditions and input.
- Responsibility, trust, or authority boundary crossed.
- Validation or authorization performed.
- State created or changed.
- Failure, retry, recovery, or compensation behavior.
- Evidence needed to prove the transition worked.

### Current state, gaps, and target direction

Separate:

- Verified current behavior.
- Specified target behavior.
- Proposed changes.
- Unknowns and blockers.

For each material gap, state the root cause, impact, and smallest correction needed.

Describe the minimum target design and the few invariants that must always hold. Derive invariants from the source material; they may concern ownership, authority, information flow, ordering, durability, trust, compatibility, approval, or side effects.

When multiple designs are plausible, recommend one based on the stated constraints and summarize only the trade-offs that could change the decision.

---

## 3. Extract Requirements and Decisions

Classify the material into:

- **Explicit requirements:** directly stated by stakeholders or authoritative sources.
- **Derived requirements:** necessary for stated constraints, invariants, safety, consistency, compatibility, or operability.
- **Assumptions:** necessary working assumptions not yet confirmed.
- **Non-goals:** intentionally excluded outcomes.
- **Decisions:** choices already made or choices that must be frozen before implementation.

Use stable IDs when they improve traceability. Do not create IDs solely for appearance, and do not force one-to-one mapping between problems, requirements, scenarios, and tasks. Traceability is expressed by ID reference — the scenario index and package plan reference requirement IDs; do not maintain a separate mapping table unless the references alone cannot show coverage.

For each material requirement, include:

- A concise statement.
- Source or rationale.
- Acceptance implication.
- Relevant constraints or uncertainty.

Include non-functional requirements only when supported by the source or necessary to make a stated outcome testable. Do not invent targets for performance, scale, availability, security, accessibility, retention, or cost.

---

## 4. Design the Scenario Set

Convert the important requirements, invariants, risks, and failure modes into a coherent scenario set — designed as a whole, not accumulated one at a time.

Keep these dimensions separate:

- **Priority:** `MUST_PASS`, `REGRESSION`, or `OPTIONAL` when useful.
- **Verification mode:** `AUTOMATED`, `HUMAN_REQUIRED`, `EXTERNAL_ENVIRONMENT`, or a combination.
- **Current evidence status:** a claim label from §1.

Do not use `HUMAN_REQUIRED` as a business priority.

Select scenario coverage from the actual failure model. Relevant cases may include normal success, invalid or partial input, limits, dependency failure, duplicate or delayed events, multi-step failure, concurrency, permission or policy violations, adversarial input, migration, downstream use, external side effects, recovery, or differences between simulated and real-world proof. Do not include a category merely because it appears in this list.

### Scenario index

Provide a compact index first; it is the coverage contract for everything that follows:

| Scenario ID | Priority | Scenario | Why It Matters / Risk Addressed | Related Requirements | Verification Mode | Current Evidence Status |
| --- | --- | --- | --- | --- | --- | --- |

The index should show coverage and gaps without duplicating full scenario prose.

### Detail tiers

Default every scenario to the lowest sufficient tier:

1. **Index-only:** simple, low-risk, obvious verification — the index row is the full specification.
2. **Compact row:** one Given/When/Then plus PASS evidence, in a grouped acceptance table shared by related scenarios.
3. **Full narrative:** only for scenarios that are `MUST_PASS` **and** complex, risky, disputed, cross-boundary, or irreversible.

### Detailed scenarios (full tier only)

Use the following fields as a toolkit, not a mandatory form. Omit fields with nothing to say — no empty fields, no "N/A" filler:

```markdown
### SC-001 — Scenario name

- Intent / risk addressed:
- Priority / verification mode / evidence status:
- Related requirements:
- Preconditions:                              # when needed
- Given / When / Then:
- Expected observable result:
- Responsibility, trust, or authority boundary:  # when relevant
- State changes and invariants:                   # when relevant
- Edge and failure cases:
- Verification class (see §6) and PASS evidence:
- Negative evidence:                         # only when absence matters
- Implementation implications:
- Downstream or operational impact:          # when relevant
```

Scenario quality rules:

- Explain why a complex or safety-critical scenario exists, but do not add a long rationale to an obvious happy path.
- Make outcomes observable and falsifiable. Avoid phrases such as "works correctly," "is secure," or "handles errors gracefully."
- Specify exact values, records, or limits only when sourced or clearly labeled as proposals.
- Test the real decision, behavior, or state transition rather than only checking that a wrapper returned success.
- Keep evidence proportional to the claim. Local or simulated proof must not be presented as live or production proof.
- Reference verification classes defined in §6 by name; never re-describe the method inside a scenario.

---

## 5. Create the Implementation Plan

Organize implementation into **coherent work packages**, ordered by dependency and then by risk. A package is the largest group of related changes that can be implemented as one change-set and verified in one pass — grouped by subsystem, feature flow, shared root cause, or shared verification surface. Do not decompose into smallest-step increments.

Split a package only on a trigger: an irreversible or destructive action, a security or authority boundary, an unresolved decision it depends on, a human or external gate in the middle of the flow, or verification methods that cannot run together.

For each package, include as applicable:

- Goal and intended outcome.
- Included scenarios (by ID).
- Dependencies and decisions that must be resolved first.
- Required changes to responsibilities, state, interfaces, validation, processing, user surfaces, integrations, or operations.
- Compatibility, migration, rollout, or rollback work.
- Failure handling and observability.
- Exit evidence: what must be shown, per included scenario, before the package is done.
- The strongest readiness claim the package can support.

Identify shared foundations once rather than repeating the same task under every scenario or package.

Use technology-specific details only when established by the sources or necessary to justify the recommendation. Do not invent class names, endpoints, schemas, commands, storage engines, deployment topology, or process boundaries merely to make the plan look concrete.

---

## 6. Define Verification, Completion, and Verdicts

Choose verification methods appropriate to the system, such as static checks, component tests, integration tests, state inspection, fault injection, user-flow checks, external-environment rehearsals, or human review. Do not force every test type onto every scenario.

Define each verification class **once**; scenarios and packages reference it by name. For each class, state:

- What it proves.
- What it does not prove.
- Required environment or dependencies.
- Required evidence.
- Whether human or external participation is needed.

Record executed checks with execution statuses only: `PASS`, `FAIL`, `NOT_RUN`, `BLOCKED`, `NOT_APPLICABLE`.

Where absence is a release condition, define relevant counters, scans, state assertions, or event/call ledgers. Do not create generic counters unrelated to the project.

Completion rules:

- Every applicable `MUST_PASS` scenario must pass with scenario-specific evidence. Package-level or suite-level green marks nothing pass by itself.
- Every applicable `REGRESSION` scenario must pass.
- Required human or external verification must either pass or remain explicitly gated.
- Unverified work must not be described as complete.
- Component, simulated, or local success must not be presented as integrated, live, or production-ready success.
- The verdict must not be stronger than the weakest applicable required scenario or unresolved gate.
- Separate document readiness from implementation readiness.

Use multiple verdicts only when useful, such as design-ready, implementation-ready, locally verified, externally verified, or production-ready. Define what each verdict means in this project.

---

## 7. Recommended Output Structure and Final Consolidation

Adapt this structure to the problem. Omit or combine sections that have no meaningful content.

```markdown
# Title and Status

# Executive Summary

# Scope, Sources, and Evidence Status

# Problems and Root Causes

# Conceptual and Architectural Model
## Core Concepts and Responsibilities
## Lifecycle and Boundaries
## Current State, Target Direction, and Gaps
## Target Design and Invariants

# Requirements, Assumptions, Non-Goals, and Decisions

# Scenario Guide
## Scenario Index
## Detailed Scenarios (full tier only)

# Implementation Plan (Work Packages)

# Verification Classes and Evidence Plan

# Completion Criteria and Verdicts

# Risks, Open Decisions, and Questions

# Glossary                                # only when terminology needs it
```

Run the consolidation pass once, at the end:

Content checks:

- Faithful to the evidence; uncertainty labeled honestly.
- Explains the important rationale without turning every section into an essay.
- Uses the source domain's terminology rather than imposing a predetermined technical model.
- Traces important problems and requirements to scenarios, packages, and evidence.
- Makes acceptance outcomes observable and falsifiable.
- Gives special attention to boundary crossings and irreversible or high-impact effects.
- Avoids duplicated requirements, boilerplate scenarios, irrelevant edge cases, and speculative detail.
- States the smallest implementation and strongest supportable verdict without overclaiming.

Mechanical checks (quick, deterministic — e.g., search by ID):

- Every ID referenced anywhere resolves to exactly one definition; no orphan requirements, scenarios, or packages.
- Claim labels and execution statuses match §1 exactly; no synonyms.
- Each verification method is defined once in §6 and only referenced elsewhere.
- Every full-narrative scenario meets the full-tier criteria; demote any that do not.
- The stated verdict is consistent with the weakest applicable gate.

---

## Source Material

Paste or attach the conversation, interview, requirements, architecture notes, implementation evidence, and stakeholder decisions below this line.

---

## V6 Change Notes

v5 described the document but not how to produce it, which invited scenario-by-scenario authoring, and its §5 "smallest independently verifiable increments" reproduced the micro-slicing that goal-prompt v4 retired. v6 adds a batch-oriented production workflow (ingest once, frame once, index first, narratives in batches, one consolidation pass), replaces smallest increments with dependency-ordered work packages with explicit split triggers, unifies the three overlapping status vocabularies into claim labels plus execution statuses, tiers scenario detail so full narratives are reserved for MUST_PASS high-risk cases, and requires verification classes to be defined once and referenced. All v5 evidence-discipline, proportionality, and anti-fabrication rules are retained.
