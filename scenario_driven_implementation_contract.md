# Scenario-Driven Implementation Contract

Read the attached scenario document and use it as the implementation contract.

Do not start by coding immediately.

## 1. Scenario Inventory

Convert the document into a scenario inventory.

For each scenario, include:

- Scenario ID
- Priority: `MUST_PASS` / `REGRESSION` / `HUMAN_REQUIRED`
- Given / When / Then
- Expected behavior
- Verification method
- Required evidence

## 2. Current State Analysis

Inspect the current codebase and classify each scenario as:

- `PASS`
- `FAIL`
- `NOT_VERIFIED`
- `HUMAN_REQUIRED`

Do not mark anything as `PASS` without evidence.

## 3. Code Impact Map

For each failing or unverified scenario, identify:

- Related modules
- Classes
- APIs
- DTOs
- Mappers
- Validators
- Repositories
- DB tables
- Downstream readers
- Tests that need to be added or changed

## 4. Implementation Plan

Create a small-step implementation plan.

Prioritize `MUST_PASS` scenarios first, then `REGRESSION` scenarios.

Avoid broad refactoring unless it is required by multiple scenarios.

## 5. Implementation

Implement the smallest complete change needed to satisfy the scenarios.

Keep source-specific logic inside adapter or mapper boundaries.

Do not leak source-specific models into downstream domains.

Do not change unrelated behavior.

## 6. Verification

Run the relevant tests.

Add missing tests when the scenario is not covered.

For every scenario, provide concrete evidence.

## 7. Final Report

Return a final report with:

- Summary of changes
- Scenario-by-scenario result
- Test commands executed
- Evidence
- Remaining risks
- Open questions
- Final verdict

## Completion Rule

The work is not done unless every `MUST_PASS` and `REGRESSION` scenario is `PASS` with evidence.

If something cannot be verified automatically, mark it as `HUMAN_REQUIRED` or `NOT_VERIFIED`.

Do not claim done without evidence.
