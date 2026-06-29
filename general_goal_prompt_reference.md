# General Goal Prompt Reference

**Applies to:** Any long-running `/goal` or goal-driven AI-agent workflow

## Purpose

Use this when writing prompts for AI agents that support broad user goals. The prompt should preserve intent, prevent unbounded work, proceed in small verified slices, and keep assumptions, proof, risks, and human-required decisions explicit.

Put detailed specs, examples, scenario lists, logs, screenshots, reports, and decision records in attachments. The prompt should tell the agent how to use them, not duplicate them.

## Reusable Goal Prompt

```markdown
/goal

Use our conversation and any attached/reference materials to pursue the user's goal in small verified slices.

First, understand the whole goal:
1. Restate the objective, success criteria, constraints, and key context.
2. Identify relevant requirements, tasks, scenarios, risks, and evidence sources.
3. If a full task/scenario set is mentioned or attached, map every item before narrowing execution.
4. Classify each item as: in_this_slice, later_slice, human_required, blocked, or out_of_scope.

Then execute one slice at a time:
1. Freeze a short slice contract: goal, scope, non-scope, selected items, proof needed, human-required gates, and done criteria.
2. Inspect only the context needed for this slice: conversation, files, docs, code, tests, prior decisions, and current sources when freshness matters.
3. Review through relevant lenses: user value, domain correctness, architecture/data contracts, safety/security, reliability, observability, performance, testability, maintainability, and rollout.
4. Produce the smallest complete result that can be verified.
5. Revise only where it directly improves this slice or prevents an obvious follow-on risk.
6. Run available targeted checks yourself.
7. Keep proof boundaries separate: local checks, integration checks, staging/prod evidence, external-provider evidence, human review, and final acceptance.
8. Do not claim completion, readiness, acceptance, deployment, security approval, or external-provider success without matching evidence.

Use human_required when verification needs a person, unavailable credentials, external systems, production access, business judgment, subjective acceptance, or trust/policy review. Verify the AI-checkable part where possible and leave the remaining gate explicit.

Checkpoint before widening:
- What changed or was produced
- What passed
- What remains human_required
- What was deliberately not done
- Evidence: commands, files, reports, observations, links, or citations
- Risks and assumptions
- Recommendation: continue / pause / ask human review / open PR / revise goal

A slice is done only when the selected scope has a decision trail, relevant items are mapped, checks passed or limits are explained, human-required gates remain explicit, and remaining work is classified rather than silently dropped.
```

## Size Rule

Keep the goal prompt under 3000 characters. Move details to attachments.
