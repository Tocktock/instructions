# General Goal Prompt Reference v6 — Goal Contract + Work Packages + Bounded Runs

**Applies to:** long-running `/goal` or goal-driven agent workflows in Codex, Claude Code, or similar coding agents.

## Purpose

A goal prompt is a compact completion contract, not a full project manual. It should tell the agent what outcome must become true, what evidence proves it, what must not change, when to stop, and how to work efficiently — including when stopping is the correct result even though more defensible work exists.

Use attachments for detailed requirements, scenarios, logs, screenshots, examples, and decisions. The goal text should point to those materials and enforce the operating discipline.

## Operating Model

- **Slice:** the current user goal or feature scope.
- **Work package:** the unit of execution and reporting. The largest coherent group of scenarios that can be implemented as one change-set and verified in one pass.
- **Scenario:** the unit of judgment. Each scenario still receives its own verdict and evidence.
- **Proof boundary:** source inspection, local test, integration test, UI proof, staging/prod evidence, external-provider evidence, or human review. Never claim beyond the strongest available proof.
- **Run:** one bounded goal execution: one package, bounded commits, one push, one broad verification, one terminal state.
- **AI-complete horizon:** the subset of scenarios whose verification mode is automated or locally automatable. The done-check is computed over this subset only.
- **Terminal state:** the declared end of a run. DONE and AI_COMPLETE are completion states; BLOCKED_STOP is a clean stop when progress requires access, a decision, or another human-owned gate.

## Why v6

v5 stopped micro-slicing and made evidence evaluator-visible. In practice it still allowed one expensive failure mode: on a slice whose remaining items were all human-gated, a goal runner that re-checks completion every cycle kept finding defensible evidence-hardening work — hours of guard/tighten/harden commits that moved no scenario verdict, each paying the full verification cost (observed: a ~9h run, 55 commits, ~47 of them verdict-neutral). v6 closes this with terminal states (AI_COMPLETE is a completion state; BLOCKED_STOP is a clean stop), a hard run budget, a commit admission rule, and idempotent re-evaluation, so repeated goal checks converge instead of spawning work.

## Reusable Goal Prompt v6

```markdown
/goal

Achieve the current slice described in our conversation and attached references. Treat the attached scenario/implementation document as the source of truth.

Done means:
- every in-scope work package is implemented or explicitly classified as DONE, LATER_SLICE, HUMAN_REQUIRED, BLOCKED, or OUT_OF_SCOPE
- every applicable MUST_PASS and REGRESSION scenario has an individual PASS/FAIL/PARTIAL/BLOCKED/HUMAN_REQUIRED verdict with scenario-specific evidence
- the broadest available regression/check surface has run once at the final gate, cross-package interactions have been reviewed, and one adversarial evidence pass is complete
- no completion, readiness, deployment, security, external-provider, or acceptance claim exceeds the available proof boundary

Terminal states — each is a terminal run exit:
- DONE: all required scenarios pass.
- AI_COMPLETE: every remaining required item is HUMAN_REQUIRED, EXTERNAL_ENVIRONMENT, BLOCKED, LATER_SLICE, or OUT_OF_SCOPE. This is a completion state because more agent work cannot close the remaining gates.
- BLOCKED_STOP: missing access, an unresolved human-owned decision, an unfit package contract, or an irreversible/human-authority decision is required. This is a clean stop, not a completion state. Ordinary technical or implementation choices are not blockers; resolve them through the autonomous decision rule below.
On any terminal state: emit the handoff report — verdict table plus every remaining blocker with owner and exact next human action — and stop. The done-check counts only scenarios inside the AI-complete horizon; human/external scenarios are satisfied by an explicit gate entry, never by additional agent work.

Run budget — hard limits:
- one work package per run, at most 3 commits, one push
- the broadest verification surface runs exactly once, at the final gate; during iteration run only checks targeted at the current package
- if two consecutive iterations change no scenario verdict and no blocker count, stop and declare the current terminal state

Commit admission rule:
- a commit must change at least one scenario verdict or reduce a blocker count
- evidence-hygiene, doc-metadata, and guardrail-tightening commits on already-passed scenarios are forbidden
- never re-open a passed scenario to strengthen its evidence
- every new test cites the scenario ID it verifies

Autonomous implementation and decision rule:
- directly implement every required part that can be safely completed with the available repository, tools, evidence, and permissions; do not push routine implementation work or ordinary technical decisions back to the user
- when a non-trivial implementation, design, or architecture decision is required, identify viable options and compare them across implementation difficulty, complexity, operational stability, maintainability, reversibility, delivery speed, and fit with the long-term product and architecture plan
- create multiple decision-relevant personas — such as implementer, maintainer/architect, operations/SRE or security reviewer, tester, and product/domain owner — and have them independently challenge the options and confirm, qualify, or dissent from the preferred choice
- synthesize the persona reviews and choose the best-balanced option. Persona agreement is advisory, not authority; the main agent owns the final decision, implementation, verification, and resulting trade-offs
- prefer the smallest complete and reversible solution that satisfies the required scenarios. Do not add long-term complexity for speculative value, and do not choose short-term convenience when it creates disproportionate stability risk or structural debt
- request human input only when the decision requires unavailable authority, credentials, destructive or irreversible action, legal/compliance judgment, security acceptance, or a business choice that cannot be responsibly inferred from the available evidence
- record the options considered, trade-offs, selected rationale, persona conclusions or dissent, residual risks, and rollback or follow-up path in the evaluator-visible handoff

Work style:
1. Plan once upfront: restate objective, constraints, assumptions, human gates, scenario coverage, package boundaries, and the AI-complete horizon this run can actually close.
2. Batch by default. Group scenarios by shared code path, subsystem, feature flow, root cause, or verification surface. A small slice may be one package.
3. Split only when needed: irreversible/destructive action, security/auth/payments/data-retention boundary, unresolved decision, unknown root cause requiring investigation, human/external gate mid-flow, or verification that cannot run together.
4. Use subagents or personas proportionally. The main agent decides routine implementation directly. For each non-trivial decision, select multiple relevant personas and obtain independent challenge and confirmation before choosing. Keep reviews bounded to planning, material decisions, high-risk packages, and the final evidence gate. Their output is input, not authority; the main agent owns synthesis, implementation, and verdicts.
5. Execute one package as one coherent change-set, fixing shared root causes once. Then verify the package deeply, once.
6. Prefer integration/UI checks that exercise the package behavior. Inspect sources and flow directly with search, file reads, diffs, and path:line references. Use short deterministic scripts only for small invariants, never as a substitute for flow analysis or real tests.
7. On failure, fix and re-verify affected scenarios only. If unrelated failures recur, split adaptively at the fault line.
8. Keep context and reports economical: do not re-read unchanged files, restate frozen contracts, paste long logs, or checkpoint per scenario.

After each package, surface evaluator-visible evidence:
- package scope and changed files/symbols
- checks run and result
- scenario table: ID/name -> verdict -> one-line evidence pointer (test/assertion, observed behavior, path:line, screenshot, or cited source)
- remaining risks, human/external gates, and next package

Final gate (runs once):
- run the broadest available regression/check surface
- inspect the whole-slice diff and shared flows for cross-package interactions
- one adversarial pass over partial verdicts, assumptions, and weakest evidence; act only on findings that change a verdict
- classify all remainder, declare the terminal state, and emit the handoff report with proof boundaries explicit

Idempotent re-evaluation:
If a new evaluation cycle finds repository and gate state unchanged since the last handoff report, re-emit that report and stop. Re-evaluation is not a request for more work.
```

## Autonomous Implementation and Decision Discipline

The agent owns implementable work from analysis through verification. A solvable technical choice must not be reclassified as a human blocker merely because several reasonable alternatives exist.

For every non-trivial implementation, design, or architecture decision:

1. Define the decision, constraints, must-pass behavior, and viable options.
2. Compare the options using implementation difficulty, total complexity, stability, maintainability, reversibility, delivery cost, and long-term direction.
3. Select multiple relevant personas and have each independently challenge the options from its own responsibility boundary. Typical personas include an implementer, maintainer or architect, operations/SRE or security reviewer, tester, and product/domain owner. Use only the personas that materially improve the decision.
4. Resolve disagreements by returning to observed evidence, scenario requirements, and explicit trade-offs. Do not treat a majority vote or reviewer opinion as proof.
5. Make and implement the final decision. Prefer the smallest complete, stable, and reversible option that preserves a credible path toward the long-term plan.
6. Report the rationale, rejected alternatives, material dissent, residual risk, and rollback or migration path.

Human escalation is reserved for authority-owned or externally gated choices: destructive or irreversible actions, unavailable access or credentials, legal/compliance decisions, security or business acceptance, and choices whose governing intent cannot be inferred from the available source material.

## Run Discipline (anti-loop)

These rules exist because an unbounded goal on a partially human-gated slice degenerates into an infinite improvement loop: there is always a weakest claim to harden.

- A run is a bounded transaction: one package, bounded commits, one push, one broad verification, one terminal state.
- Evidence sufficiency: one qualifying scenario-specific pointer, or the minimum evidence set defined by the scenario/verification class, is sufficient. Additional evidence beyond that threshold has zero value; producing it is a defect, not diligence.
- The adversarial pass is a gate, not a loop. It runs once, at the final gate, and only verdict-changing findings are acted on.
- When the goal objective is broader than the slice (e.g., a multi-phase migration), the run terminates on the slice's terminal state, never on the objective's completion.
- Terminal states are sticky: unchanged state produces the same report and no work.
- Verification economy: expensive suites and hooks are paid once per run, at the final push — not per micro-commit. Iterate with targeted checks.

## Work Package Discipline

Form packages after mapping scenarios, not while coding. Prefer the largest coherent package that can be attributed and verified cleanly. Typical packages cover 3–8 related scenarios; trivial slices may be one package.

Good grouping axes: shared user flow, shared subsystem, shared state transition, shared root cause, shared interface, shared migration, or shared verification surface.

Avoid both extremes:

- **Micro-slicing:** one scenario per loop when scenarios share a surface.
- **Over-batching:** packages so broad that failures cannot be attributed or split triggers are crossed silently.

## Planner / Evaluator Guidance

Embed planning and evaluation into the same goal run. Do not add a separate workflow layer.

Use three heavy review gates only. Decision personas operate inside these gates and must not create an additional review loop:

1. **Planning challenge:** Are scenarios missing? Are package boundaries too small or too large? Did any split trigger get ignored? Is the AI-complete horizon correct? Which non-trivial decisions require persona-based option review?
2. **High-risk package challenge:** For security, irreversible, external, or unknown-root-cause work, attack the design and verification plan before claiming progress.
3. **Final evidence challenge:** Does each required scenario have direct evidence? Does the final verdict overclaim? Are human/external gates explicit with owners? This challenge runs once and acts only on verdict-changing findings.

When actual subagents are unavailable or too costly, emulate the selected personas with concise internal multi-perspective review. Reviewer opinions are never proof; only observed evidence is proof. The main agent remains responsible for the final choice and must not escalate a resolvable technical decision merely because persona opinions differ.

## Evidence Rules

- Scenario verdicts require scenario-specific evidence.
- A green package-level suite does not by itself pass any individual scenario.
- Evidence should be pointers, not transcripts: command, test name, assertion, path:line, observed UI state, screenshot, artifact path, or citation.
- Separate observed evidence from assumptions, judgment, recommendations, and human-required acceptance.
- Evidence has a sufficiency ceiling: once a scenario has one qualifying pointer, further evidence work on it is forbidden.
