# Autonomous AI Agent Instruction for Building Scenario-Driven Interactive Architecture Simulators

> **Document type:** This entire document is an executable instruction for an AI agent.  
> **Purpose:** Given a system, feature, pull request, incident flow, data pipeline, or distributed workflow, the agent must autonomously inspect the latest evidence, model the scenarios, implement a **single standalone interactive HTML simulator**, verify it, critique it, and improve it until the user can understand the system end to end.  
> **Reusable for:** backends, microservices, event-driven systems, Kafka/Outbox flows, payments, settlement, authentication, authorization, ordering, dispatch, data pipelines, AI-agent workflows, CI/CD, infrastructure, and failure-recovery systems.

## Document metadata

- Version: `3.0`
- Default deliverable: one self-contained HTML file with no external runtime dependency
- Core format: **Scenario-driven Interactive Architecture Simulator**
- Explanation modes: **Business Flow ↔ Technical Flow**
- Required views: **Architecture ↔ Sequence**
- Required controls: previous, next, play, pause, step slider, speed, panel toggles, focus mode
- Required component interactions: Hover, Focus, Click, Touch, Pin, Outside Click, `Esc`
- Quality bar: correctness, complete understanding, meaningful interaction, accessibility, evidence separation, autonomous verification, and self-improvement

---

# 0. How to use this instruction

Provide the agent with only the information that is specific to the task:

```text
System or feature:
Repository / PR / source documents:
Audience:
Must-understand goals:
Output filename:
Non-negotiable constraints:
```

Example:

```text
System or feature: Canonical Order Synchronization Hub
Repository / PR / source documents: https://github.com/example/repository/pull/123
Audience: domain owners, backend engineers, and new contributors
Must-understand goals:
- how a source-order change reaches every related target
- how authority, transactions, retries, rollbacks, and feedback work
Output filename: canonical-sync-interactive-simulator.html
Non-negotiable constraints:
- use the actual latest head
- produce one standalone HTML file
- never present an unimplemented feature as completed
```

Do not ask the user again for information that is already available. Investigate implementation details directly from the repository and supplied sources.

---

# 1. Final mission

Do not create merely a pretty architecture picture or a static summary.

The final simulator must become an **executable mental model** that allows the user to answer, from the screen alone:

1. What input, event, or state starts the flow?
2. Which components become active, and in what order?
3. Who sends what to whom at each step?
4. What data is read, persisted, or changed?
5. Which guards and conditions select each branch?
6. Which operations belong to the same transaction?
7. What rolls back on failure, and what remains durable?
8. How is asynchronous delivery made recoverable?
9. How do duplicates, stale work, reordering, delays, and races converge?
10. What is the difference between retry and replay?
11. Which source or component owns write authority?
12. Which behaviors are implemented, safely blocked, unimplemented, or externally gated?
13. How does the path change when the user changes an input or feature flag?
14. At the current step, where does the change originate and what does it affect?

Completion does not mean “an HTML file exists.” The user should be able to explain the system in language such as:

> “This input activates A first. B and C are persisted in the same transaction. D processes the durable work asynchronously. If the target is not ready, the work is retained. If the write fails, E rolls back and the item returns to state F for retry.”

---

# 2. Agent roles and autonomy contract

The agent must act simultaneously as a:

- Principal Engineer
- Domain Analyst
- Distributed Systems Reviewer
- Data and Transaction Model Reviewer
- Interaction Designer
- Frontend Implementer
- Accessibility Reviewer
- Test and Release Reviewer

## 2.1 Default operating principles

1. Own the user's final understanding goal from start to finish.
2. Do not push repository-discoverable decisions back to the user.
3. Make routine UI, layout, color, and implementation decisions autonomously, then verify them.
4. Follow the task through callers, events, persistence, migrations, configuration, tests, documentation, and operational boundaries when needed.
5. When confidence is sufficient, proceed and verify rather than asking unnecessary questions.
6. Do not stop at the first working output; find the weakest part and improve it.
7. Never invent facts, test results, CI state, head SHAs, runtime behavior, or completion evidence.
8. Mark uncertainty as `ASSUMPTION`, `PARTIAL`, `EXTERNAL`, or `UNKNOWN`.
9. Do not perform destructive actions, production changes, external sends, privilege expansion, data mutation, PR creation, or merge without explicit authorization.
10. The final response must include the artifact link, source version, checks performed, and remaining risks.

## 2.2 Ask only when necessary

Ask the user only for decisions that cannot be resolved from the sources, such as:

- business or product semantics
- security, privacy, permissions, or tenancy policy
- production activation or risk acceptance
- cost, capacity budgets, or rollout scope
- destructive migration or data repair
- external sends, deployment, or side effects

For everything else, investigate and proceed.

## 2.3 Autonomous execution loop

```text
Understand
→ Explore
→ Model
→ Design
→ Implement
→ Verify
→ Challenge
→ Improve
→ Re-verify
```

### Understand

- Restate the goal in one or two sentences.
- List the questions the user must be able to answer.
- Identify protected behavior and the proof boundary.

### Explore

- Read the actual latest code path.
- Inspect callers, events, adapters, repositories, schema, migrations, configuration, and flags.
- Read tests and current operational documentation.
- Inspect relevant companion repositories or consumers when the contract crosses repositories.

### Model

- Define components and lanes.
- Separate commands, events, reads, writes, feedback, controls, recovery, and topology changes.
- Model transactions, state machines, authority, retries, idempotency, and concurrency.
- Create normal, failure, blocked, and recovery scenarios.

### Design

- Design Architecture and Sequence views over one shared model.
- Design Business and Technical explanations over the same scenarios.
- Make the current source, target, message, and impact direction immediately visible.
- Design the navbar, panels, tooltip system, and responsive behavior.

### Implement

- Put CSS, JavaScript, component catalogs, and scenario data inside one HTML file.
- Make inputs and feature flags alter real branches and steps.
- Make previous, next, and slider navigation deterministic.

### Verify

- Run static checks, scenario matrices, interaction tests, responsive checks, and runtime checks.
- Exercise representative paths in a browser when possible.

### Challenge

Review the result once from each perspective:

1. domain correctness
2. distributed-systems and transaction safety
3. UX, accessibility, and cognitive load
4. test and release evidence honesty

### Improve

When a finding materially changes understanding, correctness, or safety, modify the HTML and re-run the checks.

---

# 3. Source-of-truth and evidence contract

## 3.1 Fact priority

Determine facts in this order:

```text
actual latest code
→ database migrations and constraints
→ executable tests
→ runtime configuration and dependency wiring
→ current design and operations documentation
→ PR description
→ old documentation and earlier discussion
```

When prose conflicts with code or migrations, use the implementation and executable evidence, and explicitly show the conflict.

## 3.2 Repository or PR tasks

Verify all of the following:

- repository and PR number
- actual head SHA
- base SHA
- draft, open, or merged state
- changed files and relevant diffs
- whether the PR body cites an older head
- behavioral changes in the latest commit
- whether workflow runs and combined statuses actually exist
- whether PR comments and human reviews actually exist
- companion repositories and downstream consumers
- verification timestamp

Show the source version and generated timestamp in the HTML.

## 3.3 Keep evidence levels separate

Never conflate:

- static code review
- local unit tests
- local integration tests
- local browser runtime checks
- CI
- staging
- production
- human review

A local passing test is not production verification. Test counts copied from a PR body are not evidence for the actual latest head unless the linkage is verified.

## 3.4 Implementation-status vocabulary

- `PASS`: complete within the explicitly stated proof boundary
- `PARTIAL`: important behavior is implemented or proven, but named gaps remain
- `BLOCKED`: progress is intentionally stopped because a safe implementation or required authority is missing
- `EXTERNAL`: requires CI, staging, production, or human evidence
- `ASSUMPTION`: interpretation not confirmed by the sources
- `NOT IMPLEMENTED`: absent from the implementation
- `SAFE GUARD`: dangerous behavior is rejected; this does not mean the full feature is complete

Never equate “safely blocked” with “fully implemented.”

---

# 4. Deliverable contract

## 4.1 Default artifact

- one standalone HTML file
- CSS, JavaScript, component catalog, edge catalog, and scenarios embedded in the file
- works offline when opened locally
- no CDN, external font, or external JavaScript by default
- no server or build step required
- UTF-8
- directly downloadable and usable by the user

Recommended filename:

```text
<system>-interactive-architecture-simulator.html
```

Optionally create a version-pinned copy:

```text
<system>-interactive-architecture-simulator-head-<short-sha>.html
```

## 4.2 Required metadata inside the HTML

- system / feature
- repository / PR / source document
- actual head SHA or version
- generated and verified date
- scope
- evidence boundary
- known limitations
- current verdict

## 4.3 Invalid outcomes

Do not deliver:

- one static image
- only a rendered Mermaid diagram
- a static SVG with decorative playback controls
- input controls that do not alter actual steps
- Architecture and Sequence views that tell different stories
- Business and Technical modes that use different underlying flows
- only the happy path
- invented future behavior used to fill implementation gaps
- mutable DOM state that cannot be reproduced when navigating backward

---

# 5. Content the user must understand

## 5.1 System-level content

Explain:

- system boundary
- responsibility of each lane
- major components
- inputs and outputs
- database and external-system boundaries

## 5.2 Every step must answer

1. What started now?
2. What are the source and target?
3. What command, event, or data moves?
4. Why is this step necessary?
5. Which guard is evaluated?
6. What is read?
7. What is written?
8. Which state changes?
9. What belongs to the same transaction?
10. What rolls back on failure?
11. What does this enable or constrain next?
12. Where is the relevant code?
13. What evidence supports the claim?
14. What limitation remains?

## 5.3 Compare success and failure

The same simulator must support comparisons such as:

```text
successful application
vs readiness hold
vs authority rejection
vs duplicate delivery
vs stale revision
vs retry
vs terminal
vs replay
vs rollback
```

---

# 6. Overall screen architecture

Default desktop layout:

```text
┌────────────────────────────────────────────────────────────────────────────┐
│ Fixed Navbar                                                              │
│ View · Language · Reset · Prev · Play · Next · Slider · Speed · Panels   │
├──────────────────┬──────────────────────────────────────┬──────────────────┤
│ Left Panel       │ Center Visualization                 │ Right Panel      │
│ Scenario         │ Architecture / Sequence              │ Current Step     │
│ Inputs / Flags   │ Direction / Active Nodes / Edges    │ Guards / Reads   │
│ Goal / Legend    │ Timeline / State / Execution Log    │ Writes / TX      │
├──────────────────┴──────────────────────────────────────┴──────────────────┤
│ Optional help / evidence / scenario conclusion                            │
└────────────────────────────────────────────────────────────────────────────┘
```

## 6.1 Center-first principle

- The center visualization is the primary workspace.
- Closing either panel must immediately expand the center.
- Provide a focus mode that hides both panels.
- Restore the exact previous panel state when focus mode is exited.
- Recalculate SVG connectors after panel changes and viewport resizing.
- Never rotate lane titles vertically merely to save space.

---

# 7. Fixed navbar requirements

The navbar must remain visible while the page scrolls.

## 7.1 Required controls

- Architecture / Sequence toggle
- Business Flow / Technical Flow toggle
- Reset / first step
- Previous
- Play / Pause
- Next
- Step range slider
- current step / total step counter
- playback speed
- current scenario name
- current step title or summary
- left panel toggle
- right panel toggle
- focus mode
- theme
- help or shortcut hint

## 7.2 Navbar behavior

- It may wrap into two rows on smaller screens.
- Core playback controls must remain discoverable.
- Current and total steps must always be visible.
- Panel toggle state must be reflected with `aria-pressed` or `aria-expanded`.
- User preferences may be stored in `localStorage`.

## 7.3 Keyboard shortcuts

Recommended:

```text
← / →      previous / next
Space      play / pause
Home       first step
End        last step
Esc        close tooltip, popover, or drawer
A          Architecture view
S          Sequence view
B          Business Flow
T          Technical Flow
F          focus mode
```

Global shortcuts must not interfere while the user is typing in a form field.

---

# 8. Left panel, right panel, and focus mode

## 8.1 Left panel

Include:

- scenario selector
- scenario description
- inputs
- feature flags
- failure injection
- enum, Boolean, and numeric conditions
- scenario goal
- current scenario verdict
- node-state legend
- edge-type legend
- explanation of which branch each input changes

When an input changes, rebuild the scenario and reset the step state.

## 8.2 Right panel

Include:

- current step number, phase, and title
- Business Flow explanation
- Technical Flow explanation
- Source → Target
- moving command, event, or data
- guard results
- reads
- writes
- transaction boundary
- rollback scope
- state diff
- next-step hint
- code reference
- evidence
- limitation

Use progressive disclosure where helpful:

```text
Default: title, summary, direction, key state
Expanded: guards, reads, writes, transaction, code, evidence
```

## 8.3 Desktop panel behavior

- Each panel opens and closes independently.
- Closing a panel expands the center grid.
- Focus mode remembers the prior panel state.
- Exiting focus mode restores it exactly.

## 8.4 Mobile behavior

- Present side panels as overlay drawers.
- Optionally allow only one drawer at a time.
- Close via backdrop click and `Esc`.
- Use intentional z-index ordering with the fixed navbar.
- Use a bottom sheet for component details when anchored tooltips would be too small.

---

# 9. Business Flow ↔ Technical Flow toggle

These modes are not separate simulations. They are two language layers over the **same scenario, step, and state model**.

## 9.1 Business Flow

Use natural language that a domain owner can understand without reading source code.

Good:

```text
When the Sendy order changes, the system saves a new version of the shared order record.
It also keeps a durable task that will bring the linked DX order to the same state.
```

```text
If the target system is not ready, the work is retained rather than discarded.
The same work continues once the target becomes ready.
```

Avoid:

```text
Enqueue a canonical propagation high-water fence.
```

Translate that into:

```text
Check how far the target has already been updated, and do not reapply an older change.
```

## 9.2 Technical Flow

Show actual classes, methods, events, tables, states, and transaction boundaries.

```text
GeneralTmsJdbcCanonicalSourceSynchronizer.appendRevision()
→ INSERT gt_canonical_order_revision
→ UPDATE gt_canonical_order head
→ INSERT gt_canonical_propagation
→ enqueueIdempotently(CanonicalOrderChangedV1)
```

## 9.3 Everything that must switch together

- scenario title and description
- component names and types
- lane labels
- node summaries
- edge labels
- current-direction summary
- step title and summary
- sequence messages
- guard wording
- reads and writes
- transaction explanation
- state-card labels
- timeline labels
- execution logs
- tooltip and popover content
- scenario conclusion

Code references should be prominent in Technical Flow and hidden or progressively disclosed in Business Flow.

## 9.4 Data-model example

```js
const componentCatalog = {
  canonicalSynchronizer: {
    businessName: "Shared order synchronization",
    technicalName: "Canonical Synchronizer",
    businessSummary: "Determines whether the shared order meaning changed and records a new version.",
    technicalSummary: "Reduces CanonicalSourceObservation and persists revision, cursor, and propagation."
  }
};
```

```js
const step = {
  businessTitle: "Save a new version of the shared order",
  technicalTitle: "Append CanonicalOrderRevision",
  businessMessage: "Persist the changed request as a new shared-order version",
  technicalMessage: "INSERT gt_canonical_order_revision"
};
```

---

# 10. Architecture view and lane design

## 10.1 Recommended implementation

- Render components as HTML elements.
- Render connectors as SVG paths.
- Overlay HTML nodes and SVG edges in one positioned container.
- Derive edge anchors from `getBoundingClientRect()`.
- Recalculate edges after `ResizeObserver`, window resize, panel toggles, or focus-mode changes.

## 10.2 Lanes

Example lanes:

```text
Source Systems
Ingress & Transport
Canonical Core
Target & Feedback
Operations & Recovery
```

Lane-label rules:

- Never rotate text vertically.
- Do not use `writing-mode: vertical-*`.
- Render horizontal text.
- Allow natural wrapping to approximately two lines.
- Center the text.
- Give the lane-label column enough width to remain readable.
- On mobile, the lane label may become a horizontal section header.

## 10.3 Node placement

- Keep stable base positions across scenarios.
- Do not move nodes on every step.
- Keep inactive nodes visible at low contrast.
- Distinguish Source, Target, Active, Visited, Success, Hold, Warning, Error, and Blocked.
- Add text badges for the current Source and Target.

## 10.4 Node content

At minimum show:

- business or technical name
- component type
- short responsibility
- current-step status
- information affordance

Put details in the tooltip or right panel.

---

# 11. Arrows and impact direction

Arrows are semantic, not decorative.

## 11.1 Edge types

```text
EVENT       notification that something changed
COMMAND     request to perform work
DATA_WRITE  persistence to a DB, Outbox, cache, or store
READ        data retrieval
FEEDBACK    result returning to the originating flow
CONTROL     feature flag, admission, or readiness evaluation
RECOVERY    retry, replay, or lease recovery
TOPOLOGY    membership, link, or ownership relationship change
```

## 11.2 Current-impact summary

Provide a dedicated direction summary above the visualization.

Business Flow:

```text
[Change starts] Sendy order
        ─────────▶
[Affected] Change notification

The order reports that its request details changed.
```

Technical Flow:

```text
FROM SendyOrder
     ─────────▶ canonical-impacting event
TO   Domain Event
```

## 11.3 Active-edge treatment

- thicker than other edges
- clear arrowhead
- source and target markers
- message label above the edge
- moving dash or particle
- color by edge type
- strong active edge, medium visited edge, faint inactive edge
- confirm marker orientation so the direction cannot appear reversed

## 11.4 Routing quality

- Do not pass through nodes.
- Do not overlap labels with nodes.
- Minimize meaningless crossings.
- Use elbow or curved routing when it improves clarity.
- Make feedback direction visibly return toward the source.
- Recalculate after panel toggles, focus mode, and resize.
- Limit the number of simultaneously highlighted edges.

## 11.5 Do not rely on color alone

Combine:

- solid and dashed styles
- arrowhead shape
- label
- badge
- icon
- opacity
- Source / Target text

---

# 12. Sequence view

## 12.1 Required elements

- actor header
- lifeline
- step row
- message arrow
- step number
- current / previous / future state
- actor tooltip

## 12.2 Shared state with Architecture

Architecture and Sequence must share:

```text
currentScenario
currentStep
componentCatalog
edgeCatalog
state
languageMode
componentContext
```

The active source and target in Architecture must be the same actors and message in Sequence.

## 12.3 Message labels

Business Flow:

```text
send the changed order details
store the work for later
check whether the target is ready
confirm that the update was applied
```

Technical Flow:

```text
appendCanonicalImpactingEvent()
INSERT gt_canonical_propagation
targetReady(SourceRef)
CanonicalDxOrderAppliedEvent
```

## 12.4 Sequence interactions

- click a row to navigate to that step
- hover/focus/click/touch actors for component details
- highlight the current message
- show previous messages as visited
- show future messages at low contrast

---

# 13. Component catalog and tooltip / popover system

Every visible Architecture node and Sequence actor must reference one shared `componentCatalog`.

## 13.1 Required fields

```js
{
  id,
  businessName,
  technicalName,
  kind,
  businessSummary,
  technicalSummary,
  responsibilities,
  inputs,
  outputs,
  reads,
  writes,
  transaction,
  guards,
  invariants,
  failureModes,
  recovery,
  codeRefs,
  evidence,
  limitations
}
```

## 13.2 Current-step context

Separate static component meaning from its action at the current step.

```js
step.componentContext = {
  canonicalSynchronizer: {
    actionBusiness: "Check whether the order meaning actually changed",
    actionTechnical: "Reduce current and incoming CanonicalDocument",
    status: "ACTIVE",
    guardResult: "source sequence is newer",
    stateChange: "revision 7 → 8"
  }
};
```

The tooltip should show both the static responsibility and the current-step action.

## 13.3 Required interactions

| Interaction | Result |
|---|---|
| Hover | show detail |
| Mouse leave | close unpinned detail |
| Focus | show detail for keyboard users |
| Enter / Space | pin or unpin |
| Click | pin or unpin |
| Touch | open mobile detail |
| Outside click | close pinned detail |
| `Esc` | close detail |

Do not rely only on the HTML `title` attribute.

## 13.4 Tooltip layout

- use a fixed-position or portal layer
- calculate anchor position with `getBoundingClientRect()`
- flip left when there is not enough room on the right
- flip below when there is not enough room above
- clamp inside the viewport
- avoid the fixed navbar
- reposition on scroll, resize, and panel toggles
- use a close delay to avoid flicker when moving into the tooltip
- pinned detail must survive mouse leave

## 13.5 Mobile

- prefer a bottom sheet or readable large popover
- provide a close button
- support backdrop and `Esc`
- allow internal scrolling
- use adequately sized touch targets

## 13.6 Catalog-completeness check

```js
const visibleIds = new Set([
  ...document.querySelectorAll('[data-component]')
].map(element => element.dataset.component));

const missing = [...visibleIds].filter(id => !componentCatalog[id]);
if (missing.length) throw new Error(`Missing component catalog: ${missing.join(', ')}`);
```

Architecture and Sequence must not maintain separate component descriptions.

---

# 14. Scenario, step, and state model

Separate scenario definitions from DOM rendering.

## 14.1 Scenario schema

```js
{
  id,
  category,
  businessTitle,
  technicalTitle,
  businessDescription,
  technicalDescription,
  goal,
  verdict,
  inputs,
  initialState,
  actors,
  buildSteps(input)
}
```

## 14.2 Step schema

```js
{
  id,
  phase,
  from,
  to,
  edgeId,
  edgeType,

  businessTitle,
  technicalTitle,
  businessSummary,
  technicalSummary,
  businessMessage,
  technicalMessage,

  activeNodes,
  visitedNodes,
  nodeStates,
  componentContext,

  guards,
  reads,
  writes,
  transaction,
  rollback,

  statePatch,
  logs,
  codeRefs,
  evidence,
  limitations,
  nextHint
}
```

## 14.3 Deterministic state reconstruction

Do not reverse-mutate the DOM when navigating backward.

```text
current state = initialState
              + step 1 statePatch
              + step 2 statePatch
              + ...
              + current step statePatch
```

```js
function stateAt(scenario, stepIndex) {
  return scenario.steps
    .slice(0, stepIndex + 1)
    .reduce(applyPatch, structuredClone(scenario.initialState));
}
```

Previous, Next, Slider, and Timeline navigation must produce the same state.

## 14.4 Real branching

Inputs must alter the actual step sequence.

```js
function buildSteps(input) {
  const steps = [captureStep, revisionStep];

  if (!input.fanOutEnabled) {
    steps.push(holdPropagationStep);
  } else if (!input.targetReady) {
    steps.push(controlHoldStep);
  } else {
    steps.push(claimStep, targetWriteStep, feedbackStep);
  }

  return steps;
}
```

Do not create fake interactivity where controls change but the execution trace does not.

---

# 15. Required scenario coverage

Adapt names to the system, but evaluate all of these classes of behavior.

## 15.1 Normal end-to-end flow

```text
Input
→ Capture
→ Validate / Reduce
→ Persist
→ Async delivery
→ Target write
→ Confirmation / Feedback
→ Completion
```

## 15.2 Feature disabled and catch-up

- Is durable intent still stored?
- Is only execution held?
- Does enabling the feature continue without a new source mutation?

## 15.3 Readiness hold

- target readiness or admission is false
- work is retained
- retry budget is not consumed
- a bounded future recheck is scheduled
- later ready work in the same batch can continue

## 15.4 Unauthorized or non-authoritative change

- non-authoritative source
- read-only source
- divergence or audit record
- shared truth is not overwritten

## 15.5 Semantic no-change

- source sequence advances
- meaning is unchanged
- cursor and processing evidence may advance
- no new revision is appended

## 15.6 Transaction rollback

- intermediate DB failure
- Outbox append failure
- feedback-durability failure
- show which business rows, target state, and attempts roll back together

## 15.7 Duplicate and idempotency

- same delivery
- same revision
- same message ID
- no duplicate semantic side effect

## 15.8 Out-of-order and stale work

- N arrives after N+1
- stale source sequence
- older target revision
- distinguish `SUPERSEDED`, `STALE`, and `NO_CHANGE`

## 15.9 Retry, terminal, and replay

- retryable failure
- backoff
- bounded attempts
- terminal state
- authorized replay
- replay preserves payload and work identity

## 15.10 Delayed feedback and loop termination

- target-applied event
- immutable historical evidence verification
- feedback for N after the head reached N+1
- no equivalent new revision is created

## 15.11 Concurrency

- two transactions for the same source
- claim competition
- first-write high-water race
- lock ordering
- stale completion fencing

## 15.12 Membership and topology

- attach
- backfill
- merge
- remap
- deactivate
- fail closed when state reconciliation is unavailable

## 15.13 Structural identity changes

- insertion
- deletion
- reorder
- duplicate address or duplicate key
- do not preserve metadata against an unproven identity

## 15.14 Equal numeric IDs across source types

```text
SOURCE_A:123 ≠ SOURCE_B:123
```

Show fail-closed behavior or explicit source routing when a bare numeric ID is ambiguous.

## 15.15 Broker success followed by local mark loss

- external broker accepted the message
- local SENT mark failed
- lease recovery
- identical immutable-message retransmission
- consumer idempotency

## 15.16 External gates

- mixed-binary rollout
- staging
- production
- load tests
- rollback drills
- human approval

Never represent a local simulation as proof of an external gate.

---

# 16. Representing transactions, concurrency, and async boundaries

## 16.1 Transaction groups

Visually group writes that commit or roll back together.

```text
Transaction A
├─ Business row
├─ SourceCapture
└─ Outbox row
```

```text
Transaction B
├─ Target business rows
├─ Applied high-water
├─ Attempt outcome
└─ Feedback Outbox
```

Show the whole group reverting on rollback.

## 16.2 Async boundary

- separate DB commit from broker send
- distinguish an Outbox row from actual delivery
- distinguish at-least-once from exactly-once
- show when the consumer acknowledges

## 16.3 Locks and claims

Show:

- lock key
- lock ordering
- `FOR UPDATE`, advisory locks, and `SKIP LOCKED`
- claim token or attempt ID
- lease expiration
- rejection of stale worker completion

## 16.4 High-water and lineage

When relevant, show a coordinate rather than a bare revision number:

```text
Canonical / Aggregate ID
Mapping / Membership version
Revision
Hash / ETag
Causation ID
```

Explain that revision numbers from different lineages are not inherently comparable.

---

# 17. Visual language and animation

## 17.1 States

- Idle
- Active
- Visited
- Success
- Hold
- Warning
- Error
- Blocked
- External

Use borders, icons, badges, opacity, and text in addition to color.

## 17.2 Animation principles

- Use only enough animation to explain the current step.
- Animate active-edge dashes or particles.
- Apply a subtle highlight to active nodes.
- Use short state-transition animation.
- Avoid excessive pulsing, glowing, shaking, or decorative motion.
- Support `prefers-reduced-motion`.
- Allow the user to pause immediately during autoplay.

## 17.3 Cognitive-load limits

- Prefer one primary source-target pair per step.
- For transaction steps with multiple writes, explain them as one group.
- Keep node labels short and move detail to the right panel.
- Do not expand every detail simultaneously.
- Clearly differentiate current, previous, and future steps.

---

# 18. Timeline, state cards, and execution log

## 18.1 Timeline

- horizontal or responsive track
- step number, phase, short title
- current, previous, and future styling
- click to navigate
- auto-scroll the current step into view

## 18.2 State cards

Choose states appropriate to the system, for example:

```text
Canonical Revision
Propagation Status
Target Applied Revision
Attempt Count
Replay Count
Mapping Version
Outbox Status
Authority
Transaction Result
```

Use human labels in Business Flow:

```text
Shared-order version
Target update status
Retry count
Current write authority
```

## 18.3 Execution log

- step number
- source component
- message
- severity
- timestamp only when meaningful
- highlight the current step
- wording follows Business or Technical mode
- do not expose unnecessary PII, secrets, addresses, phone numbers, or raw payloads

---

# 19. Responsive behavior, accessibility, and usability

## 19.1 Desktop

- three-column layout
- fixed navbar
- independent panels
- focus mode
- horizontal lane labels
- intentional center minimum width and scrolling

## 19.2 Tablet

- narrower panels
- wrapping toolbar
- optionally close one panel by default

## 19.3 Mobile

- center-first layout
- side panels as drawers
- component detail as bottom sheet
- touch targets around 44px or larger
- backdrop and body-scroll lock
- recalculate edges after orientation changes

## 19.4 Accessibility

- semantic buttons, labels, and fieldsets
- `aria-label`, `aria-pressed`, `aria-expanded`
- visible keyboard focus
- tooltip accessible by focus
- Enter and Space activation
- `Esc` closing
- states expressed beyond color
- sufficient contrast
- optional live region for the current step
- reduced-motion support

---

# 20. Technical implementation guidance

## 20.1 Suggested structure

```html
<header id="navbar">...</header>
<div id="app">
  <aside id="left-panel">...</aside>
  <main id="simulator">
    <section id="direction-summary">...</section>
    <section id="architecture-view">...</section>
    <section id="sequence-view">...</section>
    <section id="timeline">...</section>
    <section id="state-cards">...</section>
    <section id="execution-log">...</section>
  </main>
  <aside id="right-panel">...</aside>
</div>
<div id="tooltip-layer"></div>
<div id="mobile-backdrop"></div>
```

JavaScript state:

```js
const componentCatalog = {...};
const edgeCatalog = {...};
const scenarioCatalog = {...};

let uiState = {
  scenarioId,
  stepIndex,
  viewMode: 'architecture',
  languageMode: 'business',
  leftOpen: true,
  rightOpen: true,
  focusMode: false,
  playing: false,
  speed: 1
};
```

## 20.2 Rendering pipeline

```text
read inputs
→ build scenario steps
→ calculate current state
→ render navbar
→ render panels
→ render architecture nodes
→ calculate SVG edges
→ render sequence
→ render timeline / state / log
→ position open tooltip
```

## 20.3 Edge routing

- choose anchors based on edge direction
- support self-loops
- support feedback loops
- render a background behind edge labels
- calculate after layout using `requestAnimationFrame`
- observe container and node resizing with `ResizeObserver`

## 20.4 Security

- HTML-escape dynamic strings
- minimize unsafe `innerHTML`
- never execute user-supplied text as script
- do not include real secrets, PII, or raw payloads
- do not embed automatic network fetches in the generated simulator

## 20.5 Performance

- update classes and text instead of rebuilding the entire graph when possible
- clean up autoplay timers
- avoid event-listener leaks
- consider delegated listeners for component detail
- simplify or virtualize timelines with hundreds of steps

---

# 21. Verification protocol

Do not claim completion before running these checks.

## 21.1 Static checks

- HTML parsing
- JavaScript syntax
- CSS parseability
- duplicate IDs
- missing internal anchors
- external dependencies
- every visible node and actor exists in `componentCatalog`
- every edge references existing nodes
- every step references valid source, target, and edge IDs
- every step has Business and Technical text

## 21.2 Scenario-builder checks

- default input for every scenario
- Boolean combinations
- enum combinations
- failure-injection combinations
- step counts for each branch
- final state
- illegal-state detection
- verdict matches the actual path

```js
for (const scenario of scenarios) {
  for (const input of generateInputMatrix(scenario.inputs)) {
    const built = scenario.buildSteps(input);
    assertValidScenario(built);
  }
}
```

## 21.3 Deterministic navigation

For every step verify that:

```text
Reset → step N
Next → step N+1
Previous → step N
Slider → step N
```

produces the same state and DOM.

## 21.4 Interaction checks

- Architecture / Sequence
- Business / Technical
- Reset / Previous / Play / Pause / Next
- Slider
- Speed
- left panel toggle
- right panel toggle
- focus-mode enter and restore
- keyboard shortcuts
- timeline click
- scenario input changes

## 21.5 Component-detail checks

- Architecture node hover
- Sequence actor hover
- focus
- click pin/unpin
- touch
- outside click
- `Esc`
- reposition on scroll and resize
- viewport flip and clamp
- fixed-navbar avoidance
- mobile bottom sheet
- 100% catalog coverage

## 21.6 Layout checks

- wide desktop
- narrow desktop
- tablet
- mobile portrait
- mobile landscape
- every panel-open combination
- focus mode
- horizontal lane labels
- node overlap
- edge/node collision
- edge-label collision
- long English and Korean strings

## 21.7 Runtime checks

When possible, run representative flows in a real browser or headless browser:

- zero console errors
- zero uncaught exceptions
- no broken event handlers
- autoplay timer stops correctly
- edges remain correct after resize
- mobile drawers work
- theme works
- HTML works from a local file or injected document

When browser rendering cannot be run, state that limitation and report the exact static checks performed instead.

---

# 22. Autonomous self-review and improvement

After the initial implementation, review it with these questions.

## 22.1 Domain review

- Are the source and target correct?
- Is authority or ownership overstated?
- Are operational state and shared intent separated?
- Are persistence and delivery timings accurate?

## 22.2 Distributed-systems review

- Are transaction boundaries correct?
- Is the broker boundary incorrectly shown as part of a DB transaction?
- Are duplicate, stale, retry, replay, lease, and claim distinct?
- Are safe guards distinct from completed features?

## 22.3 UX review

- Can the user find the current source and target within two seconds?
- Is the arrow direction obvious?
- Are lane labels horizontal and readable?
- Is the center squeezed by side panels?
- Does Business Flow sound natural rather than AI-generated?
- Do tooltips obscure the critical path?

## 22.4 Evidence review

- Is the actual latest head used?
- Did stale PR-body claims leak into the result?
- Are local tests overstated as CI or production proof?
- Are untested interactions claimed as complete?

## 22.5 Improvement rule

Modify the actual file when the review finds:

- an incorrect fact
- a missing critical scenario
- a wrong edge or direction
- language that is too technical or vague
- panels or tooltips that hide the visualization
- unusable mobile behavior
- non-deterministic state
- overstated evidence

Re-run the checks after every material improvement.

---

# 23. Acceptance checklist

## 23.1 Correctness

- [ ] The actual latest source/version was verified.
- [ ] Code, migrations, tests, and configuration were cross-checked.
- [ ] Implementation status and external evidence are separated.
- [ ] Unimplemented behavior is not presented as completed.
- [ ] Business and Technical modes share one underlying model.

## 23.2 Scenarios

- [ ] normal flow
- [ ] feature disabled and catch-up
- [ ] readiness hold
- [ ] authority rejection
- [ ] rollback
- [ ] duplicate and stale work
- [ ] retry, terminal, replay
- [ ] delayed feedback
- [ ] concurrency
- [ ] topology or structural-identity edge case
- [ ] external gate

## 23.3 UI

- [ ] fixed navbar
- [ ] Architecture / Sequence
- [ ] Business / Technical
- [ ] Reset / Previous / Play / Next
- [ ] Slider / Speed
- [ ] independent panel toggles
- [ ] focus mode
- [ ] current-direction summary
- [ ] horizontal lane labels
- [ ] active Source / Target badges
- [ ] timeline
- [ ] state cards
- [ ] execution log

## 23.4 Component details

- [ ] every node and actor is in the catalog
- [ ] hover works
- [ ] focus works
- [ ] click pin works
- [ ] touch works
- [ ] outside click and `Esc` work
- [ ] viewport collision is handled
- [ ] Architecture and Sequence share descriptions

## 23.5 Responsive and accessibility

- [ ] desktop
- [ ] tablet
- [ ] mobile
- [ ] drawers and backdrop
- [ ] keyboard navigation
- [ ] ARIA state
- [ ] state is not color-only
- [ ] reduced motion

## 23.6 Verification

- [ ] JavaScript syntax passes
- [ ] duplicate ID count is zero
- [ ] missing node/edge references are zero
- [ ] scenario matrix passes
- [ ] deterministic navigation passes
- [ ] browser console has zero errors, or the limitation is disclosed
- [ ] exact output path exists

---

# 24. Anti-patterns to avoid

1. Adding playback buttons to a static picture
2. Highlighting every component at once
3. Moving components on every step
4. Modeling only the happy path
5. Listing code names without explaining their meaning
6. Using technical jargon in Business Flow
7. Letting Business and Technical modes use different scenarios
8. Drawing arrows through nodes or making direction ambiguous
9. Rotating lane labels vertically
10. Encoding state using color only
11. Letting side panels crush the center visualization
12. Losing panel state when focus mode exits
13. Using only the `title` attribute for tooltips
14. Letting tooltips escape the viewport or hide behind the navbar
15. Maintaining different component explanations in Architecture and Sequence
16. Making inputs that do not alter the execution trace
17. Accumulating mutable DOM state that breaks backward navigation
18. Presenting a safely blocked operation as a completed capability
19. Presenting local tests as CI or production evidence
20. Claiming completion without creating and verifying the file

---

# 25. Copy-ready master request

Replace the bracketed values and give this block to the AI agent.

```text
Create a “Scenario-driven Interactive Architecture Simulator” that allows users to completely understand [system / feature / PR].
The final deliverable must be one standalone HTML file with no external runtime dependencies.

Source:
- [Repository / PR / documents]

Audience:
- [Audience]

Must-understand goals:
- [What the user must be able to explain]

Output:
- [Filename]

Operating method:
1. Inspect the actual latest head/version, code paths, migrations, constraints, runtime wiring, tests, and current documentation.
2. When the PR body and actual head differ, use the actual head.
3. Do not ask the user to provide details that can be found in the sources; proceed autonomously.
4. Model normal, failure, feature-disabled, hold, rollback, duplicate, stale, retry, terminal, replay, delayed-feedback, concurrency, topology, identity-collision, and external-gate scenarios.
5. After implementation, run static checks, a scenario matrix, interaction checks, responsive checks, and browser-runtime checks.
6. Review the output for domain correctness, distributed-systems safety, UX/accessibility, and evidence honesty; improve the actual HTML and re-verify it.
7. Do not perform production changes, external sends, data mutations, PR creation, or merge without explicit authorization.

Required UI:
- a navbar fixed during scrolling
- Architecture / Sequence toggle
- Business Flow / Technical Flow toggle
- Reset, Previous, Play/Pause, Next
- step slider, counter, and playback speed
- current scenario and current-step summary
- independent left and right panel toggles
- focus mode that hides both panels and restores their previous state
- mobile drawers, backdrop, and Esc support
- dark/light theme

Business Flow mode:
- use natural language that sounds like a knowledgeable human, not generated technical prose
- example: “When A is saved, the related B and C information is updated to the same current state.”
- translate implementation terms such as class names, high-water marks, and claim tokens into clear domain language

Technical Flow mode:
- show actual classes, methods, events, tables, states, transactions, locks, and feature flags
- provide code references and evidence

Both explanation modes must use the same scenarios, steps, and state. The following must switch together:
- scenario title and description
- component name and summary
- lane label
- edge label
- direction summary
- step title and summary
- sequence message
- guards, reads, writes, and transaction
- timeline, state cards, and execution log
- tooltip / popover
- final conclusion

Architecture view:
- use HTML nodes plus SVG connectors
- use stable lane-based placement
- lane labels must be horizontal, never vertically rotated
- distinguish Source, Target, Active, Visited, Success, Hold, Warning, Error, and Blocked
- show a dedicated FROM/TO or Change Starts/Affected direction summary
- distinguish EVENT, COMMAND, DATA_WRITE, READ, FEEDBACK, CONTROL, RECOVERY, and TOPOLOGY edges
- show a clear arrowhead, message label, and restrained animation for the active edge
- minimize node penetration and unnecessary crossings
- recalculate edges after panel toggles, focus mode, and resizing

Sequence view:
- share the same component catalog, scenarios, and current step as Architecture
- show actors, lifelines, step rows, message arrows, and current/previous/future state
- allow row clicks to navigate

Component detail:
- register every Architecture node and Sequence actor in one componentCatalog
- provide role, responsibilities, inputs, outputs, reads, writes, transaction, guards, invariants, failures, recovery, code refs, evidence, and limitations
- add current action and state through step-specific componentContext
- support Hover, mouse leave, Focus, Enter/Space, Click pin/unpin, Touch, Outside click, and Esc
- do not rely on the title attribute
- flip and clamp tooltips within the viewport and avoid the fixed navbar
- reposition on scroll, resize, and panel toggles
- use a bottom sheet or large readable popover on mobile

State model:
- separate scenario definitions from rendering
- reconstruct state from initialState plus statePatch values through the current step
- Previous, Next, Slider, and Timeline navigation must be deterministic
- inputs must change actual branches and step sequences

For the current step, the right panel must show:
- Source → Target
- moving event, command, or data
- why the step exists
- guards and results
- reads
- writes
- transaction boundary
- rollback scope
- state diff
- next step
- code references
- evidence
- limitation

Correctness:
- distinguish PASS, PARTIAL, SAFE GUARD, BLOCKED, NOT IMPLEMENTED, and EXTERNAL
- distinguish static review, local tests, CI, staging, production, and human review
- do not invent unverified facts
- do not expose PII, secrets, or raw payloads

Verification:
- HTML, JavaScript, and CSS syntax
- duplicate IDs
- internal anchors
- component-catalog coverage
- node, edge, and step reference integrity
- Business/Technical text coverage
- all scenario builders
- important input branch matrices
- deterministic Previous/Next/Slider behavior
- Architecture/Sequence switching
- Business/Technical switching
- fixed navbar, panel toggles, and focus-mode restoration
- Hover/Focus/Click/Touch/Outside click/Esc
- tooltip collision and resize repositioning
- desktop, tablet, and mobile layout
- horizontal lane labels
- edge direction and label collision
- browser console errors
- reduced-motion behavior

Final response:
- link to the generated HTML
- actual source head/version
- major scenarios and interactions
- checks performed
- assumptions
- remaining risks
- confidence
- verdict
```

---

# 26. Final reporting format

```text
Summary:
- what was built
- actual source/version/head

Answer:
- artifact link
- major scenarios
- major interactions
- Business/Technical mode behavior

Assumptions:
- interpretations not fully confirmed

Key Checks:
- syntax
- catalog and reference integrity
- scenario matrix
- deterministic navigation
- desktop/mobile runtime
- interaction verification

Risks:
- unimplemented flows
- external evidence still required
- rendering or environment limitations

Confidence: 0.0–1.0
Verdict: done / needs follow-up / blocked
```

Do not use `done` unless the file actually exists and its exact downloadable path has been verified.

---

# 27. Final principles

A strong interactive architecture simulator must be all of the following:

1. **Correct.** It does not distort code, data, transaction, or failure boundaries.
2. **Completely understandable.** A domain user can follow Business Flow, while an engineer can descend into Technical Flow.
3. **Explorable.** The user controls scenarios, inputs, views, language, steps, speed, and panels.
4. **Directionally clear.** The current origin, target, message, and impact are immediately visible.
5. **Explainable component by component.** No node or actor remains an unexplained box.
6. **Honest about failure and recovery.** It does not beautify only the happy path.
7. **Honest about evidence.** Local verification is separated from operational proof.
8. **Autonomously improved.** The agent identifies the weakest parts and changes the actual artifact.

The goal is not to show a diagram. The goal is to let the user explain, challenge, and review the system independently.
