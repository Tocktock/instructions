# Final State Human Review HTML Prompt

## 0. Purpose

These instructions define how to create an interactive HTML review document based on the **current Final State** of a GitHub PR or branch.
The document must focus on **product, technical, security, and operational decisions that require human judgment and cannot be decided by automated tests**.

This document is not intended to:

- Explain the change history commit by commit
- Compare past designs with the current design
- List the entire diff in file order
- Merely repeat that tests passed
- Automatically approve a PR
- Automatically approve activation in staging or production

The final deliverable must help reviewers answer the following questions:

> Was the code implemented as intended?<br>
> Is the implemented behavior itself correct as a product and operational policy?<br>
> Is it ready to merge now?<br>
> Is it ready to be enabled in staging or production?<br>
> If problems occur after activation, who will stop or roll it back, using what criteria and procedure?

---

# 1. Inputs

Use the variables below. Apply reasonable defaults to optional inputs that are not provided.

```text
REPOSITORY={{owner/repository}}
TARGET={{PR URL | PR number | branch | commit}}
BUSINESS_OBJECTIVE={{problem this change is intended to solve}}
AUDIENCE={{primary reviewers or organization}}
OUTPUT_PATH={{path for the generated self-contained HTML}}

OPTIONAL_ISSUE_KEY={{Jira or internal issue key}}
OPTIONAL_NOTION_QUERIES={{list of Notion search queries}}
OPTIONAL_SLACK_QUERIES={{list of Slack search queries}}
OPTIONAL_REQUIRED_ROLES={{list of required review roles}}
OPTIONAL_SPECIAL_RISKS={{risks that must be reviewed}}
OPTIONAL_LANGUAGE={{default: Korean}}
OPTIONAL_THEME={{default: dark}}
```

## Default handling when inputs are incomplete

- If `TARGET` is a PR, use the **latest head SHA** of the current PR.
- If `TARGET` is a branch, use the current remote branch head.
- If `BUSINESS_OBJECTIVE` is missing, derive it from the PR description and the current authority documents.
- If `AUDIENCE` is missing, default to product, architecture, backend, database, security, Ops, and QA reviewers.
- When Notion and Slack connectors are available, search using the issue key, PR title, and key domain terms.
- Do not ask a follow-up question when there is enough information to investigate and proceed.

---

# 2. Non-Negotiable Principles

## 2.1 Describe only the Final State

Use the code, documentation, configuration, migrations, and test contracts at the current head.

Unless the user explicitly requests them, do not include:

- Lists of past commits
- Intermediate implementations
- Abandoned designs
- Metrics or numbers from a previous head
- Narratives such as “It started as A and was later changed to B”
- Before-and-after comparisons
- Retrospectives on the development process

Use past information only when necessary to:

- Internally correct a PR description that no longer matches the current head
- Verify migration compatibility or rollback boundaries needed to understand the safety of the current code
- Assess remaining residue or external-environment risks in the current state

Even in these cases, write the output around the **current final contract and the decisions that must be made now**.

## 2.2 Pin the Final SHA

Check the latest head when the investigation begins, and check it again immediately before generating the HTML.

- If the head is unchanged, pin the review to that SHA.
- If the head changed, re-review the changed files and their impact.
- Wherever possible, use immutable GitHub code links in the following form:

```text
https://github.com/{{REPOSITORY}}/blob/{{FINAL_SHA}}/{{PATH}}
```

Display the following at the top of the deliverable:

```text
Final head: {{FULL_SHA}}
Review generated at: {{ABSOLUTE_DATETIME_AND_TIMEZONE}}
```

## 2.3 Evidence priority

Use the following source-of-truth order for implemented behavior:

1. Production code at the current Final SHA
2. Database migrations and runtime configuration at the current Final SHA
3. Authority documents at the current Final SHA
4. Tests and scenario ledgers at the current Final SHA
5. PR description
6. Product and organizational context from Notion and Slack

Use Notion and Slack to:

- Confirm the natural terminology the organization actually uses
- Confirm product intent and ownership boundaries
- Identify areas that were difficult to explain or understand
- Identify real operational and collaboration concerns
- Identify review roles and communication stakeholders

Do not use Notion or Slack to assert implementation facts over production code.

## 2.4 Separate automated verification from human judgment

Even when tests pass, do not automatically approve:

- Product meaning
- Source-authority and fallback policy
- The effect of failures on existing business operations
- The scope of stored personal information
- Data-retention periods
- Actual environment configuration
- Acceptable latency, error, and load thresholds
- Activation and shutdown order
- Rollback artifacts
- Operator permissions
- Owners and approvers
- Production observation results

## 2.5 Separate approval stages

Do not use a single “approval” to pass all three stages below at once.

```text
1. Code merge decision
2. Staging or shared-environment activation decision
3. Production activation decision
```

Write separate questions, evidence, and blocking conditions for each stage.

## 2.6 Do not imply formal approval

When a user selects `Agree` in the HTML, it does not mean:

- A GitHub APPROVE review has been submitted
- Staging activation has been approved
- Production activation has been approved
- Security approval has been granted
- Legal approval has been granted

State this boundary explicitly inside the HTML.

---

# 3. Investigation Procedure

## 3.1 Freeze the current state

Collect the following:

```text
PR state
merge status
draft status
base branch and base SHA
head branch and head SHA
number of changed files
lines added and deleted
current review and comment status
status/check/workflow results attached to the current head
```

If the PR description contains a SHA or test results that differ from the current head, use the current head in the deliverable.

## 3.2 Read authority documents

When present in the repository, locate them in the following order:

```text
README or feature overview
design / architecture
MUST PASS or acceptance contract
scenario ledger
rollout / operation / runbook
ADR / decision record
migration
runtime configuration
```

Classify the role of each document:

- Behavioral contract
- Current verdict
- Operational procedure
- Historical decision record
- Test evidence

When documents conflict, prioritize production code and the latest authority document. Raise the conflict itself as a human-review question.

## 3.3 Trace the code flow

Trace the flow appropriate to the feature:

```text
business entry point
→ transaction owner
→ domain event or command
→ synchronous persistence
→ outbox / queue / transport
→ consumer / worker
→ transformation or business processing
→ persistence
→ query
→ retry / replay / reconciliation
→ feature flag / runtime control
→ metrics / alert
```

For every major component, determine:

```text
What role does it play?
When does it run?
What activates it?
What input does it receive?
What does it store or output?
Which transaction does it belong to?
How far does a failure roll back?
How does it handle duplicates and delays?
What does it deliberately not do?
```

## 3.4 Investigate Notion

Combine the following search terms:

```text
issue key
PR title
feature name
domain name
key user terminology
“discussion”, “design”, “policy”, “meeting”, “decision”
```

Extract the following from Notion:

- A one-line product definition
- What the product will and will not do
- Expressions users found difficult to understand
- Terminology actually used inside the organization
- Ownership boundaries
- Items requiring a decision
- Analogies used to explain different systems

Do not copy long source passages. Summarize only the necessary meaning in natural language.

## 3.5 Investigate Slack

Combine the following search terms:

```text
issue key
PR title
feature name
deployment
test
staging
rollback
incident
write path
owner name
```

Extract the following from Slack:

- Actual development and deployment context
- Affected teams and write paths
- Who must be informed or asked to approve
- Operational concerns
- Known collaboration dependencies
- Natural internal wording

If no search results exist, state that fact and do not guess.

## 3.6 Recheck the latest head

Query the latest head again immediately before generating the HTML.

If the head changed, recheck at least:

```text
changed files
authority documents
runtime configuration
migrations
code that affects the review verdict
```

---
# 4. How to Create Human Review Items

Every review item must use the following structure:

```json
{
  "id": "A01",
  "role": "architecture",
  "gate": "merge",
  "risk": "critical",
  "title": "One topic that requires human judgment",
  "question": "A concrete judgment question that cannot be reduced to a simple yes or no",
  "approve_condition": "The state in which the reviewer can agree",
  "change_condition": "The state in which the reviewer should request changes",
  "evidence_links": [
    {"label": "Relevant code", "url": "immutable final SHA link"}
  ]
}
```

## 4.1 Requirements for a good question

A good question:

- Contains one decision per card.
- Asks about a **choice that requires agreement**, not merely an implementation fact.
- Clearly states the conditions for agreement and for requesting changes.
- Connects to an actual code path or operational boundary.
- Leaves a real risk unresolved until a person answers it.

Bad questions:

```text
Did the tests pass?
Is the code clean?
Is the documentation sufficient?
Does it seem unlikely to cause problems?
```

Examples of good questions:

```text
If input generation for this asynchronous feature fails, do we accept a policy that rolls back the existing business transaction as well?

If the source flag is turned OFF while queued messages remain, unprocessed messages may be ACKed.
Is there an operating procedure that prevents this flag from being used as a kill switch?

Original request values may remain in the outbox, Kafka, DLT, jobs, and snapshots.
Have the access controls and retention periods for those locations been approved?
```

## 4.2 Default review roles

Roles may be combined or added to suit the project, but the following six categories are the default.

### Product and Domain

Review areas:

- Scope owned by the model
- Source authority
- Fallback and conflict handling
- Invalid but safely retainable values
- Identity policy
- Out-of-scope behavior
- Alignment with user and business expectations

### Architecture and Backend

Review areas:

- Impact on existing business transactions
- Completeness across every production ingress and write path
- Capture, serialization, and query cost
- Preservation of event-time input
- Idempotency
- Distinguishing semantic duplicates from duplicate delivery
- Prevention of stale overwrites
- Lock ordering
- Retry and recovery
- ACK/NACK/DLT behavior
- Boundary between querying and workflow consumption

### Database and Data

Review areas:

- Final migration strategy
- Whether backward compatibility is intentionally abandoned
- Schema residue
- Immutable data
- Revision and current-row constraints
- Rollback compatibility
- Retention
- Storage growth
- Cleanup owner

### Security and Privacy

Review areas:

- Every location where original values are stored
- Field allowlist
- Unnecessary row dumps
- Kafka, DLT, and database ACLs
- Encryption
- Retention and deletion
- Operator permissions
- Impact of wildcard or administrator privileges
- Exposure of original values in logs and alerts

### Ops and SRE

Review areas:

- Safe default of OFF
- Initial activation order
- Normal shutdown order
- Emergency isolation order
- Drain and quarantine
- Actual Kafka, queue, and database configuration
- Rollback artifacts
- Write fences
- Dashboards and alerts
- Accounting for residual work
- On-call owner

### QA and Performance

Review areas:

- Numeric criteria defined before execution
- OFF-versus-ON comparison
- Production-like load
- Actual payload distribution
- p95/p99
- Error rate
- Database query plans
- Connection pool
- Queue lag
- DLT
- Failure-response drills
- Production observation
- Continue / stop / rollback criteria

## 4.3 Gate classification

Assign one of the following gates to every review item:

```text
merge        Decision required before code merge
ratification Final human approval and agreement on numeric thresholds
activation   Decision required before staging or shared-environment activation
production   Decision required before production activation
```

## 4.4 Risk level

```text
critical  Must block merge or activation when agreement or evidence is missing
high      Must be reviewed and explicitly included in any conditional approval
medium    May be managed as follow-up work, but requires an owner and a plan
```

---

# 5. Core Review Questions That Must Be Included

Exclude an item only when it is genuinely irrelevant to the feature.

## 5.1 Product and Domain

```text
Do we agree on the business scope the model owns and does not own?
Is the source authority for each field correct?
Is fallback used only when the primary value is absent?
When two sources disagree, is the conflict exposed rather than hidden?
Is the policy for discarding or retaining invalid but safely storable values correct?
Is the policy for merging or separating linked source identities correct?
Do reviewers understand and accept the functionality that is intentionally not provided?
```

## 5.2 Impact on synchronous transactions

```text
May a failure in the supporting feature cause the existing business save to fail?
Are all real write paths connected without omissions?
Do we accept the additional query, serialization, and hashing cost inside the transaction?
When a failure occurs, are the rollback alert and original exception both preserved correctly?
```

## 5.3 Asynchronous processing

```text
Do events, retries, and replays use the input captured at the time of the original event?
Does the system distinguish duplicate delivery from the same business meaning?
Can an older message ever overwrite a newer result?
Does ACK occur only after a safe result has been stored in the database?
If delivery to the DLT fails, does the consumer avoid advancing the original offset?
Are the retryable error scope and maximum retry count correct?
```

## 5.4 Data and Database

```text
Does the final migration match the reality of the current data?
Is it acceptable to abandon compatibility with historical data?
Are immutable results preserved by creating a new revision rather than modifying an old one?
Do both code and database constraints guarantee that only one current result exists?
Is there a concrete plan to verify staging reset and prerequisites?
Have the retention period and expected storage growth been defined?
```

## 5.5 Security and Permissions

```text
Have all locations that store original request values been identified?
Does the stored data avoid unnecessary personal information or business state?
Are ACLs and retention configured for Kafka, DLT, and the database?
Are operator reprocessing permissions restricted by time range and user scope?
Has the impact of administrator or wildcard permissions been reviewed?
Are original values excluded from logs and Slack alerts?
```

## 5.6 Activation and rollback

```text
Is the activation sequence documented in an executable runbook?
Are normal shutdown and emergency isolation treated as separate procedures?
Is there a flag that can ACK queued work without processing it?
Does the runbook prevent source flags or sampling controls from being misused as kill switches?
Do the actual queue, topic, group, and ACL settings match the code contract?
Does the rollback artifact retain the writer responsibilities required for rollback?
Can residual work be counted and preserved?
```

## 5.7 Metrics and Observation

```text
Were numeric thresholds defined before execution?
What increases in existing business p95, p99, and error rate are acceptable?
What is the payload p99 size?
What outbox age and queue lag are acceptable?
At how many DLT records must the rollout stop immediately?
What database and connection-pool limits are acceptable?
How much daily storage growth is acceptable?
What are the production observation period and exit criteria?
```

---

# 6. Automated Tests and Human Judgment Section

Place two boxes side by side in the HTML.

## What automation verified

Include only items that were actually confirmed as PASS in the project.

Examples:

```text
model structure and types
field mappings
transaction commit/rollback
duplicate and stale handling
Kafka ACK/DLT code boundaries
migration creation and rerun behavior
```

## What humans must decide

Examples:

```text
whether the business meaning is correct
whether failure of the existing business flow is acceptable
whether personal-data retention is acceptable
what the operational thresholds are
whether the actual environment is ready
who approves and responds
```

Do not write, “The automated tests passed, therefore it is safe.”

---

# 7. How to Present Approval Stages

Place the following three decision cards near the top of the HTML:

```text
Decision 1: Is the code ready to merge?
Decision 2: Is it ready to activate in staging or a shared environment?
Decision 3: Is it ready to activate in production?
```

Each card must contain three to six representative questions for that stage.

When a conditional merge is possible, state it clearly:

```text
The code may be approved for merge, but
all runtime and operator capabilities must remain OFF until {{HUMAN_AND_EXTERNAL_GATES}} are complete.
```

---

# 8. Stop Conditions

Create “Immediate Stop Conditions” based on risks confirmed in the project.

Default stop conditions:

```text
Owners disagree on product meaning or source authority
The impact of supporting-feature failure on existing business operations is not accepted
The scope, ACLs, or retention policy for stored personal information is undefined
Unknown schema, row, job, or message residue exists in a shared environment
Rollback artifacts no longer preserve required writer responsibilities
Numeric thresholds are being defined only after observing execution results
An exclusion flag that may ACK queued work is turned off while queued work remains
DLT or recovery failure may cause the original message to be treated as complete
Actual environment configuration differs from documented configuration
No accountable owner or approver exists
```

Explain in one sentence why each condition requires the rollout to stop.

---
# 9. Numeric Threshold Inputs

Provide empty input fields in the HTML.

Default fields:

```text
initial sampling rate
acceptable increase in existing business p95
acceptable increase in existing business p99
acceptable increase in existing business error rate
payload or capture p99 size
maximum outbox age
queue consumer lag
maximum DLT count
database connection or pool utilization
maximum repair or reconciliation throughput
daily and weekly storage growth
production observation period
immediate-stop threshold
```

Remove fields that do not apply to the project.

If required numeric values are still blank, do not mark final human approval as complete.

---

# 10. Required HTML Structure

Use the following order by default:

```text
Hero
1. Review purpose and Final SHA
2. Separate merge / Staging / Production decisions
3. Separate automated verification from human judgment
4. Responsibilities by role
5. Interactive human-review checklist
6. Official incomplete gates or blockers
7. Immediate stop conditions
8. Numeric threshold inputs
9. Review decision record
10. Recommended review order
11. Key evidence links
```

Add a very short Final State flow only when it is necessary to understand the project.
Do not let detailed architecture explanation become the primary purpose of the document.

---

# 11. Interaction Requirements

The HTML must be self-contained and use no external libraries.

Required functionality:

```text
filter by role
filter by gate
search
show only unreviewed items and items requiring changes
select a status for each item
add review notes to each item
overall progress
progress by role
automatic browser localStorage persistence
generate review summary
copy review summary
print / save as PDF
reset saved inputs
```

Item statuses:

```text
Not reviewed
Agree
Changes required
Not applicable
```

Required notice:

```text
Selecting “Agree” in this HTML is not an official GitHub approval or an approval to activate any environment.
```

## localStorage

- Include the repository and PR or branch identity in the storage key.
- When the Final SHA changes, do not automatically carry forward review state from the previous SHA.
- Require a confirmation dialog before resetting stored data.

---

# 12. Review Decision Record

Provide the following inputs:

```text
Final SHA
review date
reviewer name and role
decision scope
final decision
risk owner
conditions
blocking items
follow-up evidence
```

Final-decision options:

```text
On hold
Approved
Conditionally approved
Changes requested
```

The generated summary must include:

```text
Final SHA
review date
reviewer
decision scope
final decision
counts for Agree / Changes required / Not reviewed / Not applicable
items requiring changes and their notes
numeric thresholds
conditions and blocking items
risk owner
```

---

# 13. Writing and Terminology Rules

## 13.1 Write natural language

- Avoid writing that sounds translated or mechanically generated.
- Express one idea per sentence.
- Limit each paragraph to one to three sentences.
- Prefer active voice over passive voice.
- Prefer “who does what” over long sequences of nouns.
- Do not mix multiple names for the same concept.
- Prefer terminology actually used by the organization.

When `OPTIONAL_LANGUAGE` is Korean, apply these rules specifically to natural Korean. When another language is requested, apply the same clarity principles in that language.

## 13.2 Explain technical terms nearby

Good example:

```text
An Outbox (message holding area) stores a message in the database before sending it to Kafka.
```

Bad example:

```text
The Outbox pattern guarantees eventual consistency.
```

## 13.3 Turn difficult expressions into plain questions

Bad expression:

```text
Does the system prevent partial persistence of the snapshot and watermark?
```

Better expression:

```text
Can the transformation result be saved without the last-processed sequence number,
or can the sequence number advance without an actual result being stored?
```

Bad expression:

```text
Are source authority and absence fallback appropriate?
```

Better expression:

```text
Which system is authoritative for this value?
Is another value used only when the authoritative value is absent?
When the two values differ, is that conflict recorded?
```

## 13.4 Separate confirmed facts from review questions

```text
Confirmed behavior:
When capture is enabled, a failure to save the outbox record also rolls back the existing transaction.

Human review question:
To prevent downstream message loss, do we agree that the existing order save may fail as well?
```

## 13.5 Expressions to avoid

Do not use the following without an explanation:

```text
ledger
partial state
watermark
projection
canonical
durable
idempotent
eventual consistency
fail closed
cohort
residue
```

When necessary, preserve the code term in parentheses after a plain-language explanation.

---

# 14. Visual Design

Dark mode is the default.

## Base design

- Dark background with sufficient contrast
- A constrained maximum width for long-form text
- Generous spacing between cards
- Badges that distinguish risk level and role
- Red-family styling for `critical`
- Yellow-family styling for `high`
- Green styling for agreed items
- Red styling for items requiring changes
- Neutral styling for unreviewed items
- Single-column mobile layout
- Horizontal scrolling for tables only within their own containers
- No external CDN, web font, or JavaScript library

## Accessibility

- Provide a label for every input.
- Show a visible keyboard-focus indicator.
- Do not communicate state through color alone.
- Use `aria-live` for progress updates or copy-result notifications.
- Preserve a logical heading order.
- Provide an internal table of contents.
- Provide print styles.

---

# 15. Evidence Link Rules

Connect at least one direct piece of evidence to every review item.

Priority:

```text
relevant production code
relevant runtime configuration
relevant migration
relevant authority document
relevant test
relevant Notion or Slack context
```

Use Final-SHA-pinned links for implementation evidence.

Notion and Slack links may appear in a separate “Language and Organizational Context” area.

Do not quote internal messages or documents excessively. Summarize only the meaning needed for the review.

Do not copy secrets, tokens, personal contact information, or credentials into the HTML.

---

# 16. Official Gates and Current Verdict

If the repository contains a MUST PASS document, scenario ledger, or rollout gate, read and preserve its current verdict exactly.

Examples:

```text
PASS
HUMAN_REQUIRED
EXTERNAL_ENVIRONMENT
PARTIAL
REVERIFY_REQUIRED
BLOCKED
```

Do not promote a verdict to PASS on your own.

Distinguish the following statements:

```text
The code exists
A test is defined
The test was executed at the current Final SHA
The behavior was verified in actual staging
The behavior was verified in production
A human approved it
```

These are different kinds of evidence.

---

# 17. Verification Procedure

After generating the HTML, perform the following checks.

## Required static verification

```text
the file exists at the actual OUTPUT_PATH
DOCTYPE and UTF-8 are present
no duplicate HTML ids
all internal anchors exist
JavaScript syntax check passes
no previous or stale SHA remains
all GitHub code links are pinned to the Final SHA
review-item count and role count are calculated
long body paragraphs are reviewed
no external library dependency exists
```

Run the following when possible:

```bash
node --check extracted-script.js
```

## Browser verification

When possible, render at these viewport widths:

```text
Desktop: 1440px
Mobile: 390px
```

Check:

```text
no horizontal overflow
no console errors
filters work
search works
status selection works
localStorage saves and restores
the summary is generated
copy works
print styles work
```

If browser verification could not be performed, state that fact in the final response.

## Final Final-SHA check

Immediately before saving the file, query the latest head one last time.

If it changed, regenerate the deliverable for the latest head.

---
# 18. Final Response Format

Use the following structure by default for the final response:

```text
Summary:
- what was created
- Final SHA used as the basis
- number of human-review items
- number of roles

Answer:
- HTML download link
- key functionality
- the most important judgments requiring human attention

Assumptions:
- how Notion and Slack were used
- environment verification that could not be performed

Key Checks:
- ID / anchor / JavaScript / render verification
- whether any stale SHA remains
- number of code links

Risks:
- agreement in this HTML is not formal approval
- a local PASS is not a staging or production PASS

Confidence:
- 0.0–1.0

Verdict:
- done / needs follow-up / blocked
```

Always provide the generated file as a sandbox link.

---

# 19. Deliverable Quality Criteria

The completed document must answer all of the following questions:

```text
What is the current Final State?
What have automated tests already verified?
What can only a human decide?
Who should review each item?
Under what conditions can a reviewer agree?
Under what conditions should a reviewer request changes?
How is merge approval different from activation approval?
Which conditions require an immediate stop?
Which numeric thresholds are still undefined?
Who is the risk owner?
Where can the decision be recorded?
Can the reviewer open the relevant code directly?
```

If any answer is unclear, improve the document before marking the work complete.

---

# 20. Reusable Execution Request Template

Provide the following block together with these instructions.

```text
Use these instructions to create a Final State human-review HTML document.

REPOSITORY={{owner/repository}}
TARGET={{PR URL or branch}}
BUSINESS_OBJECTIVE={{problem to solve}}
AUDIENCE={{review audience}}
OUTPUT_PATH={{/mnt/data/...html}}

OPTIONAL_ISSUE_KEY={{issue key}}
OPTIONAL_NOTION_QUERIES={{
- search query 1
- search query 2
}}
OPTIONAL_SLACK_QUERIES={{
- search query 1
- search query 2
}}
OPTIONAL_REQUIRED_ROLES={{
- product
- architecture
- backend
- database
- security
- Ops
- QA
}}
OPTIONAL_SPECIAL_RISKS={{
- risk that must be reviewed 1
- risk that must be reviewed 2
}}

Additional requirements:
- Do not explain past commits or change history. Focus only on the current Final State.
- Focus on policies, responsibilities, and operational criteria that require human judgment rather than automated-test results.
- Write in clear, natural language.
- Create a self-contained dark-mode HTML file.
- Include role filters, gate filters, search, review status, notes, progress, localStorage,
  numeric-threshold inputs, review-summary generation, copy, and print functionality.
- Pin every implementation link to the Final SHA checked immediately before generation.
```

---

# 21. Condensed Execution Request

For quick use, the following request is sufficient:

```text
@GitHub @Notion @Slack

Using the current Final State of {{PR_OR_BRANCH}},
create a dark-mode HTML review guide focused on decisions that humans must make directly.

Exclude past commits and change history.
Treat GitHub code as the source of truth for implementation facts.
Use Notion and Slack to align product terminology and organizational context naturally.

Separate code merge, staging activation, and production activation.
Create interactive checklists for product, architecture, database, security, Ops, and QA reviewers.

Each item must include:
- a question requiring human judgment
- the conditions under which a reviewer can agree
- the conditions under which a reviewer should request changes
- code and document links pinned to the Final SHA
- review status and notes

Also include official incomplete gates, immediate stop conditions, numeric-threshold inputs,
a review decision record, and a copyable generated summary.
```
