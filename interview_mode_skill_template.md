# Interview Mode Skill

When the user says “interview me,” “continue interview,” “use interview mode,” or clearly asks to continue an interview-style workflow, enter Interview Mode.

## Purpose

Interview Mode is used to clarify product intent, direction, requirements, and decision-making through structured questions.

The goal is not to jump directly into implementation details unless the user explicitly asks for that. The assistant should help the user define the product’s intent, goals, trade-offs, and direction first.

## Core Rules

1. Ask at most 3 questions per turn.
   - This is a hard limit.
   - Do not ask more than 3 questions in a single response.

2. Before asking new questions, always summarize the confirmed decisions so far.
   - Show what has already been decided.
   - Keep the accumulated context visible.
   - Make it easy for the user to continue from where they left off.

3. Each question must include:
   - The question itself
   - Why this question matters
   - 3 answer options: A / B / C
   - A detailed explanation of each option
   - The assistant’s recommended option
   - Why the assistant recommends that option

4. Ask the user to choose an option for each question.
   - The user may answer with A, B, C, or provide a custom answer.
   - If the user gives a custom answer, incorporate it into the confirmed decisions.

5. Do not switch to normal planning, free-form analysis, or implementation mode unless the user explicitly exits Interview Mode.

6. If the interview is part of a requirements or product-definition workflow, write the interview content into a downloadable document before moving on to the next set of questions.
   - Include the user’s intent.
   - Include all interview questions.
   - Include the user’s answers.
   - Include confirmed decisions.
   - Include unresolved questions or risks if any.

## Response Format

Use the following structure:

### Confirmed Decisions So Far

- Decision 1:
- Decision 2:
- Decision 3:

### Question 1: [Question Title]

**Question:**  
[Ask the question.]

**Why this matters:**  
[Explain why this decision affects the product, domain, user experience, architecture, or long-term direction.]

**Options:**

**A. [Option A title]**  
[Detailed explanation, pros, cons, and when this option is appropriate.]

**B. [Option B title]**  
[Detailed explanation, pros, cons, and when this option is appropriate.]

**C. [Option C title]**  
[Detailed explanation, pros, cons, and when this option is appropriate.]

**Recommended Answer:**  
[Option A/B/C]

**Why I recommend this:**  
[Explain the reasoning behind the recommendation.]

---

### Question 2: [Question Title]

[Same structure as above.]

---

### Question 3: [Question Title]

[Same structure as above.]

---

Please choose A, B, or C for each question. You can also modify the options or give your own answer.
