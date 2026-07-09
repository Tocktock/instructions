# General Goal Prompt Reference v5 — Goal Contract + Work Packages

**Applies to:** long-running `/goal` or goal-driven agent workflows in Codex, Claude Code, or similar coding agents.

## Purpose

A goal prompt is a compact completion contract, not a full project manual. It should tell the agent what outcome must become true, what evidence proves it, what must not change, when to stop, and how to work efficiently.

Use attachments for detailed requirements, scenarios, logs, screenshots, examples, and decisions. The goal text should point to those materials and enforce the operating discipline.

## Operating Model

- **Slice:** the current user goal or feature scope.
- **Work package:** the unit of execution and reporting. It is the largest coherent group of scenarios that can be implemented as one change-set and verified in one pass.
- **Scenario:** the unit of judgment. Each scenario still receives its own verdict and evidence.
- **Proof boundary:** source inspection, local test, integration test, UI proof, staging/prod evidence, external-provider evidence, or human review. Never claim beyond the strongest available proof.

## Why v5

v4 correctly moved away from micro-slicing, but goal-mode agents also need evaluator-visible completion evidence. Codex and Claude goals are repeatedly checked against the active thread, so the agent must surface compact proof at package checkpoints and at the final gate. Hidden work, raw logs, or vague summaries weaken evaluation.

## Reusable Goal Prompt v5

```markdown
/goal

Achieve the current slice described in our conversation and attached references. Treat the attached scenario/implementation document as the source of truth.

Done means:
- every in-scope work package is implemented or explicitly classified as done, later_slice, human_required, blocked, or out_of_scope
- every applicable MUST_PASS and REGRESSION scenario has an individual pass/fail/partial/blocked/human_required verdict with scenario-specific evidence
- the broadest available regression/check surface has run, cross-package interactions have been reviewed, and the weakest claims have survived an adversarial evidence check
- no completion, readiness, deployment, security, external-provider, or acceptance claim exceeds the available proof boundary

Work style:
1. Plan once upfront: restate objective, constraints, assumptions, human gates, scenario coverage, and package boundaries.
2. Batch by default. Group scenarios by shared code path, subsystem, feature flow, root cause, or verification surface. A small slice may be one package.
3. Split only when needed: irreversible/destructive action, security/auth/payments/data-retention boundary, unresolved decision, unknown root cause requiring investigation, human/external gate mid-flow, or verification that cannot run together.
4. Use subagents or personas sparingly: one planning challenge for package boundaries when useful, one adversarial reviewer for high-risk packages, and one final evidence review. Their output is input, not authority; the main agent owns synthesis and verdicts.
5. Execute one package at a time. Implement the full package as one coherent change-set, fixing shared root causes once. Then verify the package deeply.
6. Prefer integration/UI checks that exercise the package behavior. Also inspect sources and flow directly with tools such as search, file reads, diffs, and path:line references. Use short deterministic scripts only for small invariants, never as a substitute for flow analysis or real tests.
7. On failure, fix and re-verify affected scenarios only. If unrelated failures recur, split adaptively at the fault line.
8. Keep context and reports economical: do not re-read unchanged files, restate frozen contracts, paste long logs, or checkpoint per scenario.

After each package, surface evaluator-visible evidence:
- package scope and changed files/symbols
- checks run and result
- scenario table: ID/name -> verdict -> one-line evidence pointer (test/assertion, observed behavior, path:line, screenshot, or cited source)
- remaining risks, human/external gates, and next package

Final gate:
- run the broadest available regression/check surface for the slice
- inspect the whole-slice diff and shared flows for cross-package interactions
- adversarially re-check partial verdicts, assumptions, and weakest evidence
- classify all remainder and give a final slice verdict with proof boundaries explicit

Stop and report rather than drifting if the goal is blocked, the package contract no longer fits, required access is missing, verification cannot be made defensible, or a human/irreversible decision is required.
```

## Work Package Discipline

Form packages after mapping scenarios, not while coding. Prefer the largest coherent package that can be attributed and verified cleanly. Typical packages cover 3–8 related scenarios; trivial slices may be one package.

Good grouping axes: shared user flow, shared subsystem, shared state transition, shared root cause, shared interface, shared migration, or shared verification surface.

Avoid both extremes:

- **Micro-slicing:** one scenario per loop when scenarios share a surface.
- **Over-batching:** packages so broad that failures cannot be attributed or split triggers are crossed silently.

## Planner / Evaluator Guidance

Embed planning and evaluation into the same goal run. Do not add a separate workflow layer.

Use three heavy gates only:

1. **Planning challenge:** Are scenarios missing? Are package boundaries too small or too large? Did any split trigger get ignored?
2. **High-risk package challenge:** For security, irreversible, external, or unknown-root-cause work, attack the design and verification plan before claiming progress.
3. **Final evidence challenge:** Does each required scenario have direct evidence? Does the final verdict overclaim? Are human/external gates explicit?

When actual subagents are unavailable or too costly, emulate these gates with concise internal multi-perspective review. Reviewer opinions are never proof; only observed evidence is proof.

## Evidence Rules

- Scenario verdicts require scenario-specific evidence.
- A green package-level suite does not by itself pass any individual scenario.
- Evidence should be pointers, not transcripts: command, test name, assertion, path:line, observed UI state, screenshot, artifact path, or citation.
- Separate observed evidence from assumptions, judgment, recommendations, and human-required acceptance.

## Size Rule

Keep the reusable goal prompt compact enough for goal mode. Put detailed specs and long scenario lists in attachments; the goal should reference them and require package-level execution plus scenario-level evidence.
