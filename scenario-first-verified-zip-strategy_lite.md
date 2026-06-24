# Scenario-First Verified ZIP Strategy — Lite

Version: 4.3-lite  
Delivery: **one subject-prefixed ZIP**  
Input: **the user's natural-language intent**

---

## Core Idea

Success is not “patch created,” “code looks right,” or “tests passed.” Success means the user's intent was frozen into observable scenarios before implementation, the smallest scenario-mapped change was made, every required scenario/regression passed with current evidence, and the delivered patch/package matches that evidence.

Final acceptance question:

```text
Did the user's intent become frozen scenarios, and did every required scenario pass with evidence?
```

---

## Output Naming

Freeze a short subject slug before implementation.

Slug rule: lowercase ASCII, numbers and underscores only, short and recognizable.

```text
{subject}_result.zip
└── {subject}_result/

{subject}_blocked_result.zip
└── {subject}_blocked_result/
```

Examples: `checkout_api_result.zip`, `notification_settings_blocked_result.zip`.

Record `subject`, `filename`, and `top_level_directory` in `manifest.json`.

---

## Required ZIP Contents

Use one machine file, one human report, one patch, and one evidence folder.

```text
{subject}_result/
├── manifest.json
├── report.md
├── changes.patch
└── evidence/
```

Blocked output uses the same shape with `{subject}_blocked_result/`.

| Path | Purpose |
|---|---|
| `manifest.json` | Strict machine-checkable contract, scenario status, evidence refs, patch status, and final gate. |
| `report.md` | Short reviewer-facing decision memo. |
| `changes.patch` | Unified diff, or empty when no change is needed. |
| `evidence/` | Raw logs, screenshots, command outputs, API responses, or `README.md` if no separate artifacts exist. |

Optional strict-mode file: `checksums.sha256`.

---

## `report.md` Template

The report should be a decision memo, not an audit binder.

```markdown
# Report

## Result
Goal: [what the user wanted]
Scope: [included / excluded]
Assumptions: [important only, or None]
Verdict: PASS / BLOCKED

## Scenarios
| ID | Type | Expected Behavior | Evidence | Why This Proves It | Status |
|---|---|---|---|---|---|
| S01 | MUST_PASS | [observable behavior] | E01 | [brief proof] | PASS |
| R01 | REGRESSION | [nearby behavior still works] | E02 | [brief proof] | PASS |

## Evidence
| ID | Check / Observation | Result | Artifact |
|---|---|---|---|
| E01 | `[command or observation]` | PASS, exit code 0 | `evidence/test-output.txt` |

## Delivery
Patch: included / not needed
Patch apply check: PASS / NOT_APPLICABLE / BLOCKED
Human required: None / [specific item]
Remaining risks: None / [specific non-blocking risk]
```

Collapse older sections this way:

```text
Goal + assumptions + constraints -> Result
Scenario contract + verification + proof -> Scenarios
Raw checks/logs/screenshots -> Evidence
Patch + risks + human-required + final gate -> Delivery
```

---

## Workflow

1. **Intake and freeze.** Before editing, freeze subject slug, goal, assumptions, behavior-affecting constraints, scenarios, acceptance predicates, regressions, verification methods, out-of-scope items, human-required candidates, source snapshot, and patch target assumptions.
2. **Inspect and map.** Inspect the current system before changing it. Reconstruct current behavior from available evidence. Map every scenario to affected files/layers, predicates, verification method, evidence needed, and regression risk.
3. **Implement.** Make the smallest complete scenario-mapped change. Avoid unrelated refactors. Preserve compatibility unless breaking behavior is explicitly in scope.
4. **Verify.** A scenario is `PASS` only when every predicate is `PASS`. A predicate is `PASS` only with evidence produced after the final relevant change.
5. **Loop on failures.** For any `FAIL`, `NOT_VERIFIED`, `NOT_RUN`, `BLOCKED`, or release-blocking `HUMAN_REQUIRED`: state expected vs. actual, find the first failing layer, identify root cause or blocker, fix safely or document blocker, rerun the failed scenario and related regressions, then update evidence and status.
6. **Deliver.** Produce `{subject}_result.zip` only when every required AI-verifiable scenario/regression is `PASS`; otherwise produce `{subject}_blocked_result.zip`.

Never weaken, remove, narrow, or downgrade required scenarios after coding starts.

---

## Scenario Rules

Write scenarios in behavior language:

```text
Given [initial state], When [action], Then [observable result].
```

Each scenario must have predicates:

```text
S01.P01 — Observable requirement — Evidence: E01 — Status: PASS
```

Required scenario types:

| Type | Use |
|---|---|
| `MUST_PASS` | Directly required by the user's intent. |
| `REGRESSION` | Nearby existing behavior that must keep working. |
| `PATCH_INTEGRITY` | Required when a patch is included. |

Status gate:

| Status | Meaning | Success allowed? |
|---|---|---:|
| `PASS` | Verified with current evidence. | Yes |
| `FAIL` | Expected result not met. | No |
| `NOT_VERIFIED` | Evidence insufficient. | No |
| `NOT_RUN` | Planned check not executed. | No |
| `BLOCKED` | Access/tool/environment prevents completion. | No |
| `HUMAN_REQUIRED` | Needs unavailable human/external verification. | No, unless outside verified scope before coding |

---

## Evidence Rules

Evidence must include an ID, scenario/predicate IDs, command or observation, result/output, exit code when command-based, artifact/reference when available, timestamp or `unknown`, and whether it was produced after the final relevant change.

Invalid proof: `tests passed`, `looks right`, `should work`.

Valid proof: `S01.P01 passed because E01 ran the save action after the final patch and asserted the persisted value; E02 reloaded the page and confirmed the UI read back that value.`

---

## Patch Rule

If code changes are required, include `changes.patch` as a unified diff with only scenario-mapped changes. Run `git apply --check changes.patch` or equivalent when possible and record it as evidence.

If no code change is needed, include an empty `changes.patch`, set patch status to `not_needed`, and set apply-check to `NOT_APPLICABLE`.

If a patch is required but apply-check cannot be run, produce blocked output unless patch application is genuinely not applicable.

---

## Manifest Gate

`manifest.json` must be valid JSON and include at least:

```text
schema_version = scenario-first-verified-zip/v4.3-lite
result_kind = verified_result | blocked_result
zip.subject, zip.filename, zip.top_level_directory
contract.frozen_before_implementation
contract.no_required_scenario_weakened_after_freeze
scenarios[] with ids, types, predicates, statuses, evidence_ids
evidence[] with ids, results, artifacts/observations, freshness
patch.status = included | not_needed
patch.apply_check = PASS | NOT_APPLICABLE | BLOCKED
gates.all_required_scenarios_passed
gates.all_regressions_passed
gates.every_pass_has_current_evidence
gates.every_predicate_has_evidence
gates.every_changed_artifact_maps_to_scenarios
gates.contains_generic_test_only_pass
gates.contains_release_blocking_human_required
gates.human_release_gate
gates.final_verdict
```

For success, all required scenarios/predicates must be `PASS`, every `PASS` must have current evidence, patch apply-check must be `PASS` or `NOT_APPLICABLE`, human release gate must be `clear`, and final verdict must be `done`.

---

## Harness Acceptance

Accept `{subject}_result.zip` only if filename/top-level directory match the subject, required entries exist, manifest is valid, contract was frozen before implementation, no required scenario was weakened, all required scenarios/predicates are `PASS`, every `PASS` references current evidence, no generic-test-only `PASS` exists, changed artifacts map to scenarios, patch apply-check passed when patch is included, human release gate is clear, and report verdict matches manifest verdict.

Reject success if any required scenario or predicate is `FAIL`, `NOT_VERIFIED`, `NOT_RUN`, `BLOCKED`, or release-blocking `HUMAN_REQUIRED`.

---

## Blocked and Read-Only Rules

Produce `{subject}_blocked_result.zip` when success cannot be verified. The blocked report must state scenario IDs, first failing layer, root cause or missing evidence, what was tried, what would unblock it, and release impact.

If repository access is read-only, do not claim remote files changed, a branch was pushed, a commit was created, a PR was opened, or an issue comment was posted. A verified package may still include a locally verified patch for a human to apply later.

---

## Anti-Gaming Rules

Reject outputs that rely on patch without scenario verification, generic tests as proof, `PASS` without evidence IDs, stale evidence, scenario weakening after coding starts, human-required used for AI-verifiable checks, source review replacing safe runnable verification, unmapped changed artifacts, missing patch apply-check, read-only connector remote-mutation claims, or a final verdict stronger than the weakest evidence-backed status.

---

## Final Principle

```text
One manifest for machines.
One report for humans.
One patch for changes.
One evidence folder for proof.
```
