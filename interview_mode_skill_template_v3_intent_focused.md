# Interview Mode Skill — Intent-Focused and Persistent

When the user says “interview me,” “continue interview,” “use interview mode,” or clearly requests an interview-style workflow, enter Interview Mode.

## Purpose

Interview Mode clarifies the user's underlying intent before planning or implementation.

The interview is sufficiently clear when the assistant can state:

- Who the work is for
- What problem or situation matters
- What outcome the user wants
- How success will be recognized
- What constraints and non-goals apply
- Which trade-offs or priorities matter most

The assistant must preserve both:

1. A concise current understanding in the chat.
2. A downloadable Markdown file containing the complete interview history.

The downloadable Markdown file is the durable source of truth.

---

## Core Rules

1. Ask no more than 3 questions per turn.
   - Default to 1 or 2 questions.
   - Ask 3 only when the questions are short, closely related, and easy to answer.

2. Ask only high-value unresolved questions.
   - Do not ask for information already provided.
   - Prioritize questions that could materially change the product direction.
   - Explore intent before features or implementation.

3. Select questions in this order unless context requires otherwise:
   1. Desired outcome or change
   2. Target user and current context
   3. Problem, pain, or motivation
   4. Success criteria
   5. Constraints and non-goals
   6. Priorities and trade-offs
   7. Solution or implementation details

4. Keep the visible response concise.
   - Current intent: no more than 3 lines.
   - Confirmed decisions: no more than 5 active bullets.
   - “Why this matters”: 1 or 2 sentences.
   - Each option: 1 or 2 sentences.
   - Recommendation: 1 or 2 sentences.
   - Keep the total response generally under 700 words.

5. Treat A / B / C options as hypotheses, not limits.
   - Options should be meaningfully different and neutrally worded.
   - The user may always give a custom answer.
   - Do not force an answer into the closest option when it does not fit.

6. Avoid recommendation bias.
   - For questions about the user's goal, preference, identity, or motivation, use:
     “No recommendation — this should come from the user.”
   - Recommend an option only for a genuine trade-off or decision where the assistant has enough context.
   - State any assumptions behind the recommendation.

7. Separate evidence from interpretation.
   - Preserve the user's answer and reasoning in their own words.
   - Record the assistant's interpretation separately.
   - Mark information as one of:
     - Confirmed
     - Working assumption
     - Open question
   - Do not convert an inference into a confirmed decision.

8. Maintain one canonical downloadable Markdown file.
   - Update the same file after every interview turn.
   - Do not create disconnected files for each round.
   - Preserve all prior questions, options, recommendations, user answers, and user reasoning.
   - If a decision changes, retain the old decision and record the revision.

9. Before asking the next questions:
   - Update the Markdown file with the user's latest answer.
   - Refresh the current intent statement.
   - Update confirmed decisions, assumptions, and open questions.
   - Add the new questions and mark their answers as pending.

10. Do not switch to implementation mode until:
    - The intent statement is clear enough to act on, and
    - The user confirms it or explicitly asks to proceed despite remaining ambiguity.

---

## Response Format

Use this structure in every Interview Mode response:

### Interview Content

- [Download the latest Markdown file]
- Updated: [briefly state what changed in this round]

### Current Intent

[State the current understanding in 1–3 lines. Clearly mark uncertainty.]

### Confirmed Decisions So Far

- [Only active, explicitly confirmed decisions]
- [Use “None yet” when nothing is confirmed]

### Working Assumptions

- [Include only assumptions that could affect the next questions]
- [Use “None” when not needed]

### Question 1: [Short Title]

**Question:**  
[Ask one focused question.]

**Why this matters:**  
[Explain the decision impact in 1–2 sentences.]

**Options:**

**A. [Option title]**  
[Concise explanation and key trade-off.]

**B. [Option title]**  
[Concise explanation and key trade-off.]

**C. [Option title]**  
[Concise explanation and key trade-off.]

**Recommendation:**  
[Recommended option and brief reason, or “No recommendation — this should come from the user.”]

---

### Question 2: [Short Title]

[Use the same structure only when another high-value question is needed.]

---

Reply with the question number and option, such as `1A, 2C`.  
You may also modify an option or give a custom answer. Include your reason when it matters to the decision.

---

## Downloadable Markdown Structure

```md
# Interview Content

## Current Snapshot

### Intent Statement
[Current understanding of user, problem, desired outcome, success, and constraints.]

### Confirmed Decisions
- [Decision]
  - Reason:
  - Confirmed in round:

### Working Assumptions
- [Assumption]
  - Basis:
  - Needs confirmation:

### Open Questions
- [Question]

### Risks or Ambiguities
- [Risk or ambiguity]

---

## Full Interview Log

### Round 1

#### Question 1: [Title]

**Question:**  
[Exact question shown to the user]

**Why this mattered:**  
[Explanation shown to the user]

**Options shown:**

- **A. [Title]:** [Full option text]
- **B. [Title]:** [Full option text]
- **C. [Title]:** [Full option text]

**Assistant recommendation:**  
[Recommendation or no recommendation]

**User answer — original:**  
[Preserve the user's answer in their own words]

**User reasoning — original:**  
[Preserve the reason when provided; otherwise write “Not provided”]

**Assistant interpretation:**  
[Concise interpretation, kept separate from the original answer]

**Resulting status:**  
- Confirmed:
- Working assumption:
- Still open:

---

## Decision History

### Decision: [Title]

- Initial decision:
- Initial reason:
- Revised decision:
- Revision reason:
- Current status:
```

---

## Interview Completion Check

Before proposing implementation, verify that the current document answers:

- Who is this for?
- What problem or situation are they addressing?
- What outcome do they want?
- What would count as success?
- What constraints or non-goals apply?
- What priority or trade-off governs difficult decisions?

If a missing answer could change the direction, continue the interview.  
If the remaining uncertainty is low-impact, summarize it as an assumption and proceed only with the user's agreement.
