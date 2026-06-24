# Prompt: Test Scenario-Based Implementation Document

Use the conversation and interview content below as the source material, and create a test scenario-based implementation document.

The goal is to turn the problems, requirements, edge cases, concerns, and decisions discussed in the conversation and interview into a practical implementation document that an engineer can execute directly.

Please organize the document using the following criteria.

## 1. Problem Definition

- Identify the current problems revealed in the conversation and interview.
- Explain the root causes, not just the surface-level feature requests.
- Separate product confusion, domain boundary issues, validation gaps, data consistency risks, and downstream execution risks when applicable.

## 2. Requirement Extraction

- Extract both explicit and implicit requirements from the conversation and interview.
- Clearly distinguish between:
  - Requirements directly stated by stakeholders
  - Requirements inferred from implementation constraints
  - Assumptions that need confirmation
- Do not invent facts that are not supported by the conversation or interview.

## 3. Test Scenarios

Convert each requirement into verifiable test scenarios.

For each scenario, include:

- Scenario ID
- Priority: `MUST_PASS`, `REGRESSION`, or `HUMAN_REQUIRED`
- Given / When / Then
- Expected behavior
- Related requirement
- Verification method
- Required evidence

The scenarios should include:

- Happy path cases
- Edge cases
- Invalid or partial input cases
- Boundary value cases
- Backward compatibility cases
- Regression cases
- Downstream read / execution cases

## 4. Implementation Plan

For each test scenario, describe what needs to be implemented.

Break the implementation down by area when applicable:

- Domain model
- Source adapter / input mapping
- Canonical interface or contract
- Validation / normalization
- Persistence / snapshot strategy
- API behavior
- UI behavior
- Downstream reader behavior
- Migration or backward compatibility
- Error handling
- Logging / observability

Keep the implementation steps as small and independently verifiable as possible.

## 5. Verification Plan

For each scenario, define how it should be verified.

Include:

- Unit tests
- Integration tests
- API tests
- Database state checks
- UI checks
- Log or metric checks
- Manual verification steps, only when automation is not enough

Clearly separate:

- Automatically verifiable items
- Human-required checks
- Not-yet-verifiable items

## 6. Completion Criteria

Define what must be true before the work can be considered complete.

Use the following rule:

- Every `MUST_PASS` scenario must pass with concrete evidence.
- Every `REGRESSION` scenario must pass with concrete evidence.
- Any item that cannot be verified must remain as `NOT_VERIFIED` or `HUMAN_REQUIRED`.
- Do not mark the work as `done` unless all required scenarios have passed with evidence.
- The final verdict must not be stronger than the weakest scenario result.

## 7. Output Format

Please produce the final document using this structure:

```markdown
# Summary

# Problem Definition

# Requirements

## Explicit Requirements

## Implicit Requirements

## Assumptions

# Test Scenarios

| Scenario ID | Priority | Given | When | Then | Verification | Evidence |
| --- | --- | --- | --- | --- | --- | --- |

# Implementation Plan

# Verification Plan

# Completion Criteria

# Risks

# Open Questions

# Final Verdict
```

## Important Constraints

- Base the document only on the conversation and interview content.
- Do not guess unsupported facts.
- Mark uncertain points as `Assumption` or `Open Question`.
- Make the document specific enough for an engineer to start implementation.
- Focus on test scenario-based implementation, not just summarization.
- Prefer the smallest complete implementation that satisfies the scenarios.
- Include evidence requirements for every scenario.

---

## Short Version

Use the conversation and interview content below to create a test scenario-based implementation document.

Analyze the problems, requirements, edge cases, concerns, and decisions discussed in the conversation and interview, then produce:

1. Core problem and root cause
2. Explicit and implicit requirements
3. Test scenarios in Given / When / Then format
4. Priority classification: `MUST_PASS`, `REGRESSION`, `HUMAN_REQUIRED`
5. Implementation plan mapped to each scenario
6. Verification method and evidence criteria
7. Completion criteria, risks, and open questions

Do not invent unsupported facts. Mark uncertain points as `Assumption` or `Open Question`.

The final output should be concrete enough for an engineer to implement directly.
