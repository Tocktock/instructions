# General Goal Prompt Reference v2

**Applies to:** Any long-running `/goal` or goal-driven AI-agent workflow, especially workflows that may coordinate sub-agents, tools, reviewers, or multi-step implementation.

## Purpose

Use this when writing prompts for AI agents that support broad user goals. The prompt should preserve intent, prevent unbounded work, proceed in small verified slices, and keep assumptions, proof, risks, and human-required decisions explicit.

When sub-agents are available, the prompt should use them deliberately during planning, implementation review, and final verification. When sub-agents are not available, the agent should emulate the same discipline with concise multi-persona analysis.

Sub-agents and personas are review mechanisms, not separate sources of authority. The main agent remains responsible for synthesis, evidence quality, scope control, and final recommendations.

Put detailed specs, examples, scenario lists, logs, screenshots, reports, and decision records in attachments. The prompt should tell the agent how to use them, not duplicate them.

## V2 Additions

- **Sub-agent use:** Use sub-agents whenever they can materially improve planning, implementation review, or final verification.
- **Multi-persona analysis:** Evaluate work through relevant lenses such as product, UX, architecture, domain correctness, security, privacy, maintainability, testing, reliability, observability, performance, and rollout.
- **Synthesis accountability:** Sub-agent/persona findings must be reconciled into one decision trail. Disagreements, uncertainties, and unresolved risks should remain visible.
- **Scope discipline:** Sub-agents should not expand the goal. Each one should have a bounded question, expected evidence, and clear output.
- **Proof discipline:** Persona review is not proof by itself. Claims still require checks, citations, artifacts, or explicit human-required gates.

## Reusable Goal Prompt v2

```markdown
/goal

Use our conversation and attached/reference materials to pursue the user's goal in small verified slices.

When the environment supports sub-agents, use them wherever they materially improve planning, implementation review, or final verification. Keep each sub-agent bounded to a clear question, required evidence, and output. Do not let sub-agents expand scope or create unsupported claims. If sub-agents are unavailable, perform the same review as concise multi-persona analysis.

First, understand the whole goal:
1. Restate objective, success criteria, constraints, key context, and assumptions.
2. Identify requirements, tasks, scenarios, risks, dependencies, and evidence sources.
3. If a full task/scenario set is mentioned or attached, map every item before narrowing execution.
4. Classify each item as: in_this_slice, later_slice, human_required, blocked, or out_of_scope.
5. Review the plan through relevant personas: product, UX, architecture, domain, security/safety/privacy, maintainability, testing/QA, reliability/ops, and rollout.

Then execute one slice at a time:
1. Freeze a slice contract: goal, scope, non-scope, selected items, proof needed, human-required gates, and done criteria.
2. Inspect only context needed for this slice: conversation, files, docs, code, tests, prior decisions, and current sources when freshness matters.
3. Use focused sub-agents/personas to challenge assumptions, review implementation, identify risks, and verify evidence.
4. Produce the smallest complete result that can be verified.
5. Revise only where it improves this slice or prevents an obvious follow-on risk.
6. Run available targeted checks yourself.
7. Keep proof boundaries separate: local checks, integration checks, staging/prod evidence, external-provider evidence, human review, and final acceptance.
8. Do not claim completion, readiness, acceptance, deployment, security approval, or external-provider success without matching evidence.

Use human_required when verification needs a person, unavailable credentials, external systems, production access, business judgment, subjective acceptance, trust/policy review, or irreversible action. Verify AI-checkable parts and leave remaining gates explicit.

Checkpoint before widening:
- Produced/changed
- Checks passed
- Sub-agent/persona findings and unresolved disagreements
- Remaining human_required gates
- Deliberately not done
- Evidence: commands, files, reports, observations, links, or citations
- Risks and assumptions
- Recommendation: continue / pause / ask human review / open PR / revise goal

A slice is done only when the selected scope has a decision trail, items are mapped, checks passed or limits are explained, human-required gates remain explicit, and remaining work is classified rather than silently dropped.
```

## Sub-Agent / Persona Guidance

Use sub-agents or persona analysis selectively. The goal is higher-quality reasoning and verification, not more ceremony.

Recommended checkpoints:

1. **Planning:** Have relevant lenses challenge scope, assumptions, dependencies, missing requirements, and slice boundaries.
2. **Implementation review:** Have relevant lenses inspect the produced work for correctness, product fit, UX impact, architecture/data-contract risk, security/privacy exposure, maintainability, and test coverage.
3. **Final verification:** Have relevant lenses confirm that evidence matches the done criteria and that remaining work is explicitly classified.

Use only the lenses that matter for the slice. For example, a backend migration may need architecture, security, data integrity, testing, and rollout review, while a copy update may only need product, UX, and QA review.

## Anti-Patterns

Avoid:

- Creating sub-agents with vague ownership.
- Treating persona opinions as verified evidence.
- Expanding scope because a reviewer found a nice-to-have.
- Claiming acceptance, deployment, or security approval without matching proof.
- Repeating attachment details inside the prompt instead of referencing the attachments.
- Running every persona on every slice when only a few are relevant.
- Hiding disagreement between reviewers or collapsing uncertainty into false confidence.

## Size Rule

Keep the reusable goal prompt under 3000 characters. Move detailed specs, examples, scenario lists, logs, screenshots, reports, and decision records to attachments.

## V2 Change Notes

This version keeps the original small-slice, evidence-bound workflow while adding explicit guidance for sub-agent use and multi-persona review. The additions are designed to improve planning quality, implementation review, and final verification without weakening scope control or proof discipline.
