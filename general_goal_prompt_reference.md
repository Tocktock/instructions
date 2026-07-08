# General Goal Prompt Reference v4

**Applies to:** Any long-running `/goal` or goal-driven AI-agent workflow, especially workflows that may coordinate sub-agents, tools, reviewers, implementation work, or multi-step verification.

## Purpose

Use this when writing prompts for AI agents that support broad user goals. The prompt should preserve intent, prevent unbounded work, and keep assumptions, evidence, risks, and human-required decisions explicit — while executing in bold, coherent **work packages** instead of micro-slices.

v4 exists because v3's per-scenario execution caused slow progress and high token usage: repeated context reloading, per-scenario contracts, and per-scenario checkpoint reports. v4 keeps the slice model and every safety rail, but moves execution and reporting to package granularity. Effort saved on ceremony is reinvested into deeper direct verification and a slice-level final quality gate.

Two units, deliberately separated:

- **Work package** — the unit of *execution and reporting*. A coherent group of scenarios implemented as one change-set and verified in one deep pass.
- **Scenario** — the unit of *judgment*. Every scenario still receives an individual pass/fail/partial/blocked/human_required verdict backed by scenario-specific evidence.

Put detailed specs, examples, scenario lists, logs, screenshots, reports, and decision records in attachments. The prompt should tell the agent how to use them, not duplicate them.

## V4 Additions (vs v3)

- **Batch by default:** scenarios sharing a surface are grouped into work packages; splitting requires an explicit trigger. v3's "one scenario at a time" rule is retired.
- **Two-phase package execution:** implement the whole package boldly, then verify the whole package deeply. No interleaved per-scenario verify loops.
- **One checkpoint per package,** containing a compact per-scenario results table — not a full checkpoint block per scenario.
- **Final quality gate per slice:** regression sweep, cross-package interaction check, and adversarial review of the weakest evidence.
- **Token economy rules:** load context once per package, reuse it, report by pointer, never restate frozen contracts.
- **Attribution guard:** package-level green never substitutes for scenario-level evidence.

## Reusable Goal Prompt v4

```markdown
/goal

Use our conversation and references to pursue the user's goal in bold, coherent work packages, each deeply verified.

Use sub-agents/personas sparingly: optional planning review, adversarial review for high-risk packages, and the final quality gate. The main agent owns synthesis, evidence quality, scope, source inspection, flow analysis, and final verdicts.

Understand (once, upfront):
1. Restate objective, success criteria, constraints, context, and assumptions.
2. Map every requirement and scenario before execution.
3. Group scenarios into work packages by shared code path, subsystem, feature flow, or verification surface. Batch by default: a package is the largest coherent group you can implement as one change-set and verify in one pass. A small slice may be a single package.
4. Split a package only on a trigger: irreversible/destructive actions, security/auth/payments surface, unknown root cause requiring investigation, a human gate mid-flow, or verification methods that cannot run together.
5. Classify items as: this_slice, later_slice, human_required, blocked, or out_of_scope.

Execute one package at a time, in two phases.

Phase A — Implement (bold):
1. Freeze the package contract once: scenarios covered, expected behaviors, affected flows/files, verification plan, human gates, done criteria. If the package grows beyond its contract mid-flight, stop, re-scope, re-freeze — do not drift.
2. Load needed context once and reuse it across the whole package. Do not re-read unchanged files between scenarios.
3. Implement all scenarios in the package as one coherent change-set. Fix shared root causes at package level, once.

Phase B — Verify (deep):
4. Verify the package as a whole: prefer one local integration/UI test pass exercising all scenarios; verify sources directly with rg, sed, git diff, and flow tracing (entry, state/data, control flow, boundaries, UI behavior, edge/error paths).
5. Judge every scenario individually: pass / fail / partial / blocked / human_required. Each verdict must cite scenario-specific evidence (test name, assertion, observed behavior, path:line). A green suite alone marks nothing pass.
6. On failures: fix, then re-verify only the affected scenarios. If failures recur across unrelated scenarios, split the package and continue — adaptive splitting, not preemptive micro-slicing.
7. Use Python only for short deterministic checks (regex, counts, small parsing, file existence, short invariants). Never as a substitute for inspection, flow analysis, or integration/UI tests.
8. Keep proof boundaries separate: source inspection, local checks, integration checks, UI checks, staging/prod evidence, external-provider evidence, human review, final acceptance. Do not claim completion, readiness, acceptance, deployment, security approval, or external-provider success without matching evidence.

Checkpoint once per package (not per scenario):
- package scope and change summary (files/symbols)
- checks run
- scenario results table: name → verdict → one-line evidence
- remaining risk and explicit human gates
- next package or recommendation

Final quality gate (once per slice), after the last package:
- full regression sweep across the slice's broadest available test surface
- cross-package interaction check on shared files/flows (git diff over the whole slice)
- adversarial pass on the weakest evidence: partial verdicts and assumption-backed claims
- classify everything remaining: done, later_slice, human_required, blocked, out_of_scope
- slice verdict with proof boundaries explicit

Use human_required when verification needs a person, unavailable credentials, external systems, production access, business or subjective judgment, trust/policy review, or irreversible action. Verify AI-checkable parts and leave remaining gates explicit.

A package is done when its contract is met, per-scenario verdicts are evidenced, and remainder is classified. A slice is done only when all packages are done and the final quality gate has passed or its blockers are explicit.
```

## Work Package Discipline

Use this whenever the task includes user stories, test cases, acceptance criteria, bug reports, flows, or implied scenarios.

Forming packages:

1. Map every scenario first, then group — never group lazily as you go.
2. Group by shared code path, subsystem, feature flow, shared root cause, or shared verification surface. Any one strong shared axis justifies batching; v3's requirement that code path, verification method, *and* risk all match is retired.
3. Prefer the largest package that still fits one coherent change-set and one verification pass. Typical: 3–8 scenarios; small slices may be one package covering everything.
4. Split triggers (any one suffices): irreversible or destructive actions; security, auth, payments, or data-retention surface; unknown root cause that needs investigation first; a human gate in the middle of the flow; verification methods that cannot run in one pass.
5. Order packages by dependency; among independents, highest risk first.
6. Adaptive splitting: if a package fails verification across unrelated scenarios twice, split it at the fault line and continue. Splitting is a response to observed complexity, not a default posture.

## Per-Package Verification

The agent verifies as an investigator, not a summarizer of test output or reviewer opinions. One deep pass per package:

- Prefer a single local integration/UI test run exercising all scenarios in the package (see Verification Priority).
- Verify sources directly: `rg`, `sed`, `git diff`, inspection of changed files and symbols.
- Trace the real flows the package touches: entry points, state/data, control flow, API/component boundaries, UI behavior, edge/error paths.
- Then judge each scenario: **pass / fail / partial / blocked / human_required**.

Attribution guard: every scenario verdict must cite evidence specific to that scenario — a test name, an assertion, an observed behavior, a path:line. Package-level green output, changed code, or sub-agent agreement marks nothing pass by itself.

Failure loop: fix → re-verify affected scenarios only (no full re-run) → after repeated unrelated failures, split the package.

## Verification Priority for Implementation Work

Unchanged from v3 in substance; applied per package rather than per scenario:

1. **Local integration tests** — prefer when they exercise real behavior across the package's changed components.
2. **UI tests** — prefer when the package affects user-facing flow, routing, visual state, forms, interactions, or client/server integration.
3. **Unit tests** — isolated logic, edge cases, fast regression coverage.
4. **Source inspection and flow analysis** — `rg`, `sed`, `git diff`, logs, targeted manual checks that explain why the checks cover each scenario.
5. **Next-best substitute** — if integration/UI tests cannot run, say why and use the strongest available alternative.

## Python Verification Limits

Unchanged from v3. Python only for short, deterministic checks: regex, occurrence counts, small parsing, comparing small structured outputs, file existence/content, short invariants. Never to replace source inspection, flow analysis, or integration/UI tests. Python evidence must be short, deterministic, and tied to the scenario it verifies.

## Evidence and Reporting Economy

Evidence should justify the verdict, not transcribe the work.

- One checkpoint per package; scenario table rows are one line each.
- Evidence by pointer: command names, path:line, changed symbols, test names, short excerpts, screenshots when UI matters, citations when external material matters.
- Establish context once per package; do not re-read unchanged files or restate established findings between scenarios.
- Reference the frozen package contract; never restate it.
- Avoid: long logs, broad diffs, repeated context, full file contents, persona opinions treated as proof.
- Separate observed evidence from assumptions, judgments, and recommendations.

## Sub-Agent / Persona Guidance

Fewer, heavier gates. Sub-agents are review mechanisms, not authority; the main agent owns synthesis and final verdicts.

1. **Planning (optional, once per slice):** challenge package boundaries, scope, missing scenarios, and split-trigger classification.
2. **Per-package: none by default.** Add one adversarial reviewer only for packages that carry a split trigger (security, irreversibility, unknown root cause).
3. **Final quality gate (recommended):** a verification-focused review that evidence matches done criteria, cross-package interactions were checked, and the weakest claims survive attack.

When sub-agents are unavailable, emulate with concise multi-persona analysis at the same three points only.

## Final Quality Gate (per slice)

This gate absorbs the effort v3 spent on per-scenario ceremony. After the last package:

- run the broadest available regression surface for the slice
- check cross-package interactions on shared files and flows (`git diff` across the whole slice)
- adversarially re-examine the weakest evidence: every partial verdict and every assumption-backed claim
- confirm all human gates are explicitly surfaced, none silently absorbed
- classify all remainder and issue a slice verdict with proof boundaries explicit

## Anti-Patterns

Avoid:

- micro-slicing: executing one scenario per pass when scenarios share a surface
- per-scenario contracts, checkpoints, or verify loops
- re-reading unchanged context between scenarios in a package
- marking scenarios pass from package-level green without scenario-specific evidence
- over-batching: packages crossing split triggers, or so large that failures cannot be attributed
- skipping the final quality gate because packages were individually green
- preemptive splitting out of caution when no trigger applies
- Python replacing flow analysis or integration/UI tests
- claiming acceptance, deployment, readiness, or security approval without matching proof
- persona opinions treated as verified evidence; scope expansion for reviewer nice-to-haves
- recording excessive evidence that obscures the decision; hiding disagreement or collapsing uncertainty into false confidence

## Size Rule

Keep the reusable goal prompt compact. Move detailed specs, examples, scenario lists, logs, screenshots, reports, and decision records to attachments.

## V4 Change Notes

v3's one-scenario-at-a-time rule, triple-condition batching ban, per-scenario contract freezes, and per-scenario eight-field checkpoints forced repeated context reloading and heavy reporting overhead — slow progress, high token cost. v4 keeps the slice model and all safety rails (human gates, proof boundaries, evidence discipline, Python limits, direct source verification with `rg`/`sed`/`git diff`) but makes the work package the unit of execution and reporting: batch by default with explicit split triggers, implement boldly in Phase A, verify deeply in Phase B, checkpoint once per package, and finish each slice with a final quality gate. Per-scenario judgment with scenario-specific evidence is retained so batched execution cannot hide individual failures.
