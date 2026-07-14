# Comprehensive Change Report HTML Prompt

Use this prompt to create a comprehensive, self-contained HTML report for a change, pull request, commit, migration, incident, refactor, configuration update, or similar body of work.

```text
Create a comprehensive, self-contained HTML report that helps me fully understand the change described below.

CHANGE OR TASK:
{{Describe the change, pull request, commit, migration, incident, refactor, configuration update, or other work here}}

SOURCE MATERIAL:
{{Provide relevant diffs, commits, files, tickets, requirements, logs, test results, discussions, or links here}}

TARGET AUDIENCE:
{{For example: engineer, technical lead, product manager, new team member, or non-technical stakeholder}}

DEPTH:
{{Overview / Standard / Deep technical analysis}}

Your goal is to explain not only what changed, but also why it changed, how the work was performed, how it was validated, and what consequences it may have.

## Report requirements

Produce a single, valid HTML document that can be opened directly in a browser.

The HTML report must:

- Be self-contained, with all CSS and JavaScript embedded in the file.
- Use clear semantic HTML, readable typography, responsive styling, and accessible color contrast.
- Include a title, generation date, table of contents, and navigable section headings.
- Use diagrams, tables, timelines, code snippets, or before-and-after comparisons where they improve understanding.
- Clearly distinguish confirmed facts, interpretations, assumptions, and unresolved questions.
- Never invent missing details. Label unavailable information as “Unknown” or “Not provided.”
- Explain technical terms when the intended audience may not know them.
- Focus on understanding and decision-making rather than merely repeating the source material.

## Required sections

Include the following sections in this order:

1. Executive Summary
   - What changed
   - Why it matters
   - The most important outcome
   - Overall risk level

2. Background and Context
   - The situation before the change
   - The original problem, request, or motivation
   - Relevant business, product, or technical context
   - Constraints and dependencies

3. Scope
   - What was included
   - What was intentionally excluded
   - Systems, components, users, files, or workflows affected

4. Before and After
   - How the system or process behaved before
   - How it behaves after
   - A concise side-by-side comparison
   - Any user-visible differences

5. Detailed Change Breakdown
   For every meaningful change, explain:
   - What was modified
   - Where it was modified
   - How the implementation works
   - Why that approach was selected
   - How it relates to the larger change

6. Reasoning and Intuition
   - The reasoning behind the solution
   - The mental model that makes the change easier to understand
   - Alternatives that were considered or could reasonably have been considered
   - Trade-offs, compromises, and rejected approaches
   - Why the final approach was preferable

7. Work Performed
   - A chronological or logical account of the work
   - Important implementation steps
   - Refactoring, migration, cleanup, or supporting work
   - Problems encountered and how they were resolved

8. Architecture and Data Flow
   - Relevant components and their responsibilities
   - How data, control, or requests move through the system
   - Updated interactions between components
   - Include a diagram when useful

9. Impact Analysis
   - Expected benefits
   - Behavioral changes
   - Performance implications
   - Security and privacy implications
   - Compatibility implications
   - Operational and maintenance implications
   - Potential impact on users and dependent systems

10. Verification and Evidence
    - Tests performed
    - Validation steps
    - Test results or other evidence
    - Important scenarios that were checked
    - Scenarios that were not checked
    - Whether the available evidence supports the intended outcome

11. Risks, Limitations, and Edge Cases
    - Known risks
    - Remaining limitations
    - Failure modes
    - Edge cases
    - Rollback or recovery considerations
    - Monitoring recommendations

12. Open Questions and Follow-Up Work
    - Unresolved questions
    - Deferred work
    - Recommended next steps
    - Decisions that still require confirmation

13. Key Takeaways
    - The five to ten most important points a reader should remember

14. Glossary
    - Define project-specific, technical, or unfamiliar terminology used in the report

15. Evidence and Assumptions
    - List the source material used
    - Identify conclusions directly supported by evidence
    - Identify assumptions and inferences
    - Highlight missing information that reduced confidence

## Final comprehension quiz

Place the quiz at the absolute bottom of the HTML document, after every other report section.

The quiz must:

- Contain 7–12 questions covering the most important concepts in the report.
- Include a mixture of multiple-choice, true-or-false, and scenario-based questions.
- Test understanding rather than simple word matching.
- Require a score of at least 80% to pass.
- Be interactive using embedded JavaScript.
- Include a “Submit Quiz” button.
- Show the score after submission.
- Explain why each submitted answer is correct or incorrect.
- Allow the reader to retry the quiz.
- Randomize answer order when practical.
- Display a clear “Passed” or “Not Passed” result.
- Show a completion message only after the passing score has been achieved.
- Keep the correct answers hidden until the reader submits the quiz.

## Quality checks

Before returning the report, verify that:

- The output is a complete HTML document beginning with `<!DOCTYPE html>`.
- All required sections are present.
- The quiz is the final section.
- Internal navigation links work.
- The embedded JavaScript has no obvious syntax errors.
- The report does not contain unsupported claims.
- Important conclusions are connected to evidence.
- The explanation is appropriate for the stated target audience.
- The document can be saved as an `.html` file and opened without external dependencies.

Return only the finished HTML document unless I explicitly request commentary outside the report.
```
