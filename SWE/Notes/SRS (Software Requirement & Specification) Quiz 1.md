# Software Requirement & Specification (SRS)

**Course:** SWE 4401 | **Textbook ref:** Sommerville Ch.4

---

## Why SRS?

> Simple miscommunication → major project failures. SRS forces everyone to agree on _what_ is being built _before_ building it.

**Cost of fixing a bug by SDLC phase:**

|Phase|Relative Cost|
|---|---|
|Requirement Analysis|$1|
|Testing|$100|
|Deployment & Maintenance|$1000|

A mistake caught at requirements costs $1. The same mistake found after deployment costs $1000.

---

## SDLC Phases

```
1. Planning
2. Requirement Analysis  ──► produces SRS
3. Design                ──► produces DDS (Design Document Specification)
4. Implementation  ┐
5. Testing         ┘  (handled in parallel; Unit Testing = TDD, done by devs)
6. Deployment + Maintenance
```

---

## Key Terms (Often Confused)

|Term|Meaning|Example|
|---|---|---|
|**Requirement**|What/Why is needed|User needs to log in|
|**Feature**|How to fulfill it|Login form|
|**Specification**|Details about the feature|Username + password for authentication|

These three are almost similar but have slight differences. Specification also acts as a constraint on the process.

---

## Two Types of Requirements

### Functional Requirement (FR)

- **What the system does** — specific services, inputs, outputs
- Defines features of the system
- Must be **Complete** and **Consistent**

**Completeness:** Specified clearly enough that developers can implement and testers can verify it without ambiguity or missing info.

**Consistency:** No two FRs contradict each other.

> In practice, for large complex systems, achieving full completeness and consistency is nearly impossible — too many stakeholders with conflicting needs.

### Non-Functional Requirement (NFR)

- **How the system behaves** — quality attributes, constraints
- Keyword hint: often ends in **"-ty"** (security, scalability, usability, reliability...)
- Must be **quantified** — vague adjectives are rejected
- Often applies to the system as a whole, not just one feature
- **More critical than individual FRs** — failing an NFR can make the whole system unusable

**NFR vs FR on workarounds:**

- FR not met → usually a workaround exists
- NFR not met → usually **no workaround**; the whole system must comply

> Example: An aircraft that fails its reliability NFR won't be certified for flight. There's no workaround.

---

## NFR Categories (3 Types)

```
NFR
├── Product NFR
│     Performance, Usability, Security, Reliability, Space
│     → Quality of the system itself
│
├── Organizational NFR
│     Internal policies, dev process standards, tech stack constraints
│     → Internal company rules (e.g. "must use Python", "must follow ISO standard")
│
└── External NFR
      Legal requirements, safety regulations, environmental constraints
      → Imposed from outside (e.g. privacy law, banking regulations)
```

**NFR Metrics — how to quantify them:**

|Property|Measure|
|---|---|
|Speed|Transactions/second, response time|
|Size|MB, number of storage chips|
|Ease of use|Training time, number of errors per hour|
|Reliability|Mean time to failure, probability of unavailability|
|Robustness|Time to restart after failure|
|Portability|% of target-dependent statements|

**Example (MHC-PMS system):**

- **Product NFR:** The system shall be available Mon–Fri 08:30–17:30. Downtime shall not exceed 5 seconds per day.
- **Organizational NFR:** Users shall authenticate using their health authority identity card.
- **External NFR:** The system shall implement patient privacy provisions as set out in HStan-03-2006-priv.

---

## User Requirements vs System Requirements

This is one of the most important distinctions in SRS. They serve **different audiences** and are written in **different styles**.

**Analogy — think of a building project:**

- **User requirement** = "I want a house with 3 bedrooms and a garden." (what the client says)
- **System requirement** = The architect's blueprint: exact room dimensions, load-bearing wall specs, wiring plans. (what the builder actually needs to construct it)

The client doesn't need to understand load calculations. The builder can't work from "I want 3 bedrooms." Both documents are necessary.

|Aspect|User Requirements|System Requirements|
|---|---|---|
|**Who reads it**|Client, manager, non-technical stakeholders|Developers, architects, testers|
|**Written in**|Natural language + simple diagrams|Detailed, precise technical description|
|**Source**|Directly from stakeholders|Derived and expanded from user requirements|
|**Length per point**|Max 1–2 sentences|Multiple sub-points per user requirement|
|**Jargon**|None — must be understandable without technical background|Allowed — written for developers|
|**Ambiguity**|Some acceptable|Must be eliminated|

**Why the distinction matters:** A manager approving budget reads user requirements. A developer building the system reads system requirements. Mixing them in one document causes confusion — either too vague for devs, or too technical for clients.

**Numbering convention:**

```
User Req 1     ──► System Req 1.1, 1.2, 1.3 ...
User Req 2     ──► System Req 2.1, 2.2, 2.3 ...
```

Each user req is broken into multiple atomic system reqs.

**Concrete example (MHC-PMS):**

> **User Req (1 sentence):** The MHC-PMS shall generate monthly management reports showing the cost of drugs prescribed by each clinic during that month.

Expanded into **System Reqs:**

- 1.1 — On the last working day of each month, a summary of drugs prescribed, their cost and clinics shall be generated.
- 1.2 — The system shall automatically generate the report for printing after 17:30 on the last working day.
- 1.3 — A report shall be created for each clinic listing drug names, total prescriptions, doses, and total cost.
- 1.4 — If drugs have different dose units (10mg, 20mg...), separate reports shall be created per dose unit.
- 1.5 — Access to cost reports shall be restricted to authorized users on a management access control list.

> One vague user-level statement becomes 5 precise, testable system-level requirements.

---

## Stakeholders

**Definition:** Anyone affected by or involved in the system — not just the end-users.

**Example (nationwide medical system):** Stakeholders = patients, doctors, nurses, receptionists, hospital management, accountants, IT staff, legal department, Bangladesh legal system.

### 4 Types

|Type|Who|Example|
|---|---|---|
|**Internal**|Part of the company building/commissioning the system|Employees, managers, owners|
|**External**|Outside the company; affected by the system|Users, clients, suppliers|
|**Primary**|Directly impacted (positive or negative) by the system|Patients using the medical system|
|**Secondary**|Indirectly impacted|Legal regulators, the general public|

**Priority order for gathering requirements:** `Primary → Secondary → External → Internal`

Start with who's most directly affected. Their requirements drive everything else.

---

## Requirement Engineering (RE) Process

RE is the formal process of defining what a system must do, done **before** any tech stack decisions.

**3 core activities (iterative/spiral — no definite endpoint):**

```
        ┌──────────────────┐
   ┌───►│   Elicitation    │  ← Gathering from stakeholders
   │    └────────┬─────────┘
   │             │
   │    ┌────────▼─────────┐
   │    │  Specification   │  ← Converts gathered needs into SRS
   │    └────────┬─────────┘
   │             │
   │    ┌────────▼─────────┐
   └────│   Validation     │  ← Checks correctness & acceptability
        └──────────────────┘
         (cycle repeats — incremental)
```

It's a spiral model — you don't finish elicitation before starting specification. You loop back as understanding deepens.

**Full RE Process (Sommerville):**

1. Feasibility Study — is this system worth building? (short focused study: does it meet org objectives? feasible within budget/tech? can it integrate with existing systems?)
2. Requirements Elicitation & Analysis
3. Requirements Specification
4. Requirements Validation
5. Requirements Management (ongoing)

---

## Elicitation (Gathering Requirements)

### Steps:

1. **Identify stakeholders**
2. **Do basic research** — analyze competitors, product environment
3. **JAD (Joint Application Development)** — invite stakeholders for open discussion. Different backgrounds → conflicts → need **deep elicitation**

### Sub-activities inside Elicitation & Analysis:

1. **Requirements Discovery** — interact with stakeholders, gather raw requirements
2. **Classification & Organization** — group related requirements, link to system architecture
3. **Prioritization & Negotiation** — resolve conflicts between stakeholders through compromise
4. **Specification** — document what has been gathered

### Why Elicitation is Hard:

- Stakeholders often cannot articulate what they actually want
- They use domain-specific jargon the analyst may not know
- Different stakeholders have conflicting needs
- Political factors influence what gets said
- Business environment changes during the analysis process

### Elicitation Techniques

|Technique|Description|Notes|
|---|---|---|
|**One-on-one interviews**|Direct Q&A with a stakeholder|Usually with product owner; not scalable|
|**Focus groups**|Small group discussion|Good for surfacing conflicting views|
|**Surveys & questionnaires**|Written questions sent broadly|Good reach, but shallow answers|
|**Observation (Passive)**|Watch users work without interfering|Reveals actual vs. stated behavior|
|**Observation (Active / Participatory)**|Analyst joins and participates in the work|Deeper insight|
|**Ethnography**|Immersive long-term observation in the work environment|Best for discovering implicit/unstated requirements|
|**Scenarios**|Walk through a real-life usage story|Makes abstract needs concrete|
|**Use Cases**|Formal diagram of actor-system interactions|Captures all interaction types systematically|
|**Prototyping**|Build a rough version for users to react to|Great for validation, not just discovery|

**Ethnography** is particularly powerful for finding:

- Requirements hidden in _how people actually work_ (not how procedures say they should)
- Requirements that emerge from cooperation and team awareness

### Interview Question Types:

- **Open-ended:** "What will the user do in the system?" (broad, exploratory)
- **Closed:** "The password must be 5 characters — is this right?" (yes/no)
- **Probing:** "Why is that important to you?" (follow-up dig)
- **Scenario-based:** "What happens if the server goes down mid-transaction?"

> Exam note: Questions are scenario-based — be ready to identify FR/NFR from a given paragraph or case study.

---

## Scenarios & Use Cases

### Scenarios

A scenario walks through one specific interaction from start to finish. Includes:

1. Starting state (who is logged in, what data exists)
2. Normal flow of events
3. What can go wrong and how it is handled
4. Other concurrent activities
5. System state at completion

Used to add detail to outline requirements and make them concrete for stakeholders.

### Use Cases (UML)

Formally captures every possible actor-system interaction type.

- **Actors** = stick figures (humans or other systems)
- **Use cases** = named ellipses (e.g., "Register Patient", "Generate Report")
- Lines connect actors to the interactions they are involved in

Use cases are great for discovering FR but not very useful for NFR, organizational, or domain constraints.

---

## Specification (Writing Requirements)

### Formats

|Format|Best For|Audience|
|---|---|---|
|**Natural Language**|User requirements; simple, accessible|Non-technical stakeholders|
|**Structured Natural Language (SNL)**|System requirements; standard template|Developers|
|**Graphical (UML, Use Cases, Sequence Diagrams)**|Showing state changes, action sequences|Developers, architects|
|**Mathematical (FSM, sets)**|Safety-critical systems; formally unambiguous|Specialists only|

### Natural Language Guidelines (Sommerville):

1. Use a **standard format** — one requirement per sentence
2. Use **shall** for mandatory, **should** for desirable
3. Use **text highlighting** to emphasize key parts
4. Avoid **software jargon** when writing user requirements
5. Include a **rationale** — why this requirement exists, who asked for it

### Structured Natural Language (SNL) Template

```
Function      : ________________________
Description   : ________________________
Inputs        : ________________________
Source        : ________________________
Outputs       : ________________________
Destination   : ________________________
Action        : ________________________
Pre-condition : ________________________
Post-condition: ________________________
Side effects  : ________________________
```

Used for developer-facing system requirements. Eliminates ambiguity by forcing explicit answers to every field.

---

## Modal Verbs — Shall / Should / May

|Verb|Meaning|Example|
|---|---|---|
|**SHALL**|Mandatory — must be implemented|`The system SHALL encrypt passwords before storage.`|
|**SHOULD**|Desirable — expected if feasible|`The system SHOULD provide dark mode.`|
|**MAY**|Optional — allowed but not required|`The system MAY allow profile picture uploads.`|

Avoid `can` (implies already implemented) and ambiguous `will`. Use only shall/should/may.

---

## Vague Words — Replace With Measurements

|Vague Word|Replace With|
|---|---|
|fast|within 2 seconds|
|easy|complete task within 3 minutes|
|secure|MFA for admin login|
|reliable|99.5% uptime|
|soon|within 1 minute|
|large|up to 10 MB|
|quickly|within X seconds for Y records|

**Example:**

> ❌ `The system shall generate reports quickly.` ✅ `The system shall generate the semester performance report within 5 seconds for up to 2,000 student records.`

---

## EARS Format (Easy Approach to Requirement Syntax)

|Pattern|Template|Example|
|---|---|---|
|**Ubiquitous (Always)**|`The system shall [response].`|`The system shall store student records persistently.`|
|**Event-driven**|`When [event], the system shall [response].`|`When a student submits leave, the system shall notify the advisor.`|
|**State-driven**|`While [state], the system shall [response].`|`While registration is closed, the system shall prevent course add/drop.`|
|**Optional Feature**|`Where [feature included], the system shall [response].`|`Where analytics is enabled, the system shall log all user sessions.`|
|**Unwanted Behavior**|`If [unwanted condition], then the system shall [response].`|`If a user enters the wrong password 5 times, the system shall lock the account for 15 minutes.`|

---

## Requirements Validation

After writing requirements, you validate them — checking that they define the system the client actually wants.

### Validation Checks:

1. **Validity** — Does this reflect real user needs?
2. **Consistency** — Does it contradict any other requirement?
3. **Completeness** — Are all required behaviors covered?
4. **Realism** — Can this be implemented within budget and tech constraints?
5. **Verifiability** — Can a tester write a test to prove this is met?

### Validation Techniques:

- **Requirements Reviews** — A team reads the document and checks for errors/inconsistencies
- **Prototyping** — Show a working model to stakeholders for feedback
- **Test-case generation** — Write tests for each requirement; if a test cannot be written, the requirement is unclear

Validation rarely catches all problems. Some issues only surface after deployment — this is why requirements management continues throughout.

---

## Requirements Management

Requirements always change — new hardware, legislation, shifting business priorities, new stakeholders. Requirements management tracks and controls those changes.

### Why Requirements Change:

- Business/technical environment changes post-installation
- The people paying ≠ the people using (different needs)
- Large systems have diverse users with conflicting requirements
- Stakeholders develop better understanding as development progresses

### Change Management Process (3 steps):

```
Identified Problem
       │
       ▼
1. Problem Analysis & Change Specification
   └── Is the change valid? Clarify the proposal.
       │
       ▼
2. Change Analysis & Costing
   └── What is the impact? Which requirements/design/code are affected? Cost?
       │
       ▼
3. Change Implementation
   └── Update requirements doc → update design → update code → re-test
       │
       ▼
Revised Requirements
```

### Planning Needs:

- **Unique IDs** for every requirement (FR-001, NFR-003...) — needed for cross-referencing and traceability
- **Traceability policies** — define relationships between requirements and between requirements and design
- **Tool support** — spreadsheets for small projects; dedicated RM tools for large ones

> Never modify the system first and update requirements later — they will get out of sync. Update requirements first, then implement.

---

## The SRS Document Structure (IEEE Standard)

The SRS is the official statement of what the system must do. It serves multiple audiences simultaneously.

**Who reads it and why:**

|Reader|Uses SRS to...|
|---|---|
|System customers|Specify/review requirements; propose changes|
|Managers|Plan bids and development timelines|
|System engineers|Understand what to build|
|Test engineers|Develop validation tests|
|Maintenance engineers|Understand system structure for future changes|

**Standard SRS structure (IEEE-based):**

|Section|Contents|
|---|---|
|Preface|Readership, version history, change rationale|
|Introduction|Why the system is needed, how it fits business goals|
|Glossary|Technical terms defined — assume no expertise from reader|
|User Requirements|Natural language descriptions of services for users|
|System Architecture|High-level overview of system modules|
|System Requirements|Detailed FR and NFR; interface definitions|
|System Models|UML, data-flow, or semantic models|
|System Evolution|Anticipated future changes; helps designers avoid restrictive decisions|
|Appendices|Hardware specs, database schemas, etc.|
|Index|Alphabetic, function, diagram indexes|

---

## Bad Requirements — 4 Ways They Fail

|Failure Type|What Happens|Result|
|---|---|---|
|**Ambiguous**|Different people imagine different systems|Dev builds the wrong thing|
|**Incomplete**|Developers fill gaps with assumptions|Key behavior is undefined|
|**Inconsistent**|Requirements contradict each other|No single correct implementation|
|**Untestable**|Testers cannot prove completion|System can never be formally accepted|

> "If two intelligent people can read the same requirement and imagine two different systems, the requirement is not ready."

---

## 10 Characteristics of a Good Requirement

|#|Characteristic|What it means|
|---|---|---|
|1|**Clear**|No ambiguous language|
|2|**Unambiguous**|Only one possible interpretation|
|3|**Complete**|All necessary info included; developer must not have to guess any behavior|
|4|**Consistent**|No contradictions with other requirements|
|5|**Verifiable**|Can be tested objectively|
|6|**Feasible**|Technically and financially achievable|
|7|**Necessary**|Stakeholder actually needs it|
|8|**Atomic**|One requirement per sentence|
|9|**Traceable**|Has ID, source, reason, priority|
|10|**Modifiable**|Easy to change as the project evolves|

**Traceability fields:**

```
FR-001 | Requirement text | Source | Reason | Priority
```

---

## Rules for Writing Requirements (Checklist)

- [ ] Use `shall` (mandatory) or `should` (optional) — not vague verbs
- [ ] No vague words: quickly, easily, user-friendly, attractive, soon...
- [ ] One requirement per sentence — atomic
- [ ] Don't mix FR and NFR in one statement
- [ ] Specify the **actor** (student, admin, registered user...)
- [ ] Specify the **condition** when needed ("After payment is done, the system shall...")
- [ ] Specify the **data** ("shall store student name, ID, and email" — not just "student info")
- [ ] Avoid design decisions unless they are real constraints
- [ ] Add rationale — who asked for it and why

---

## Good vs Bad Requirements — All Examples

### Ambiguity

> ❌ `The system shall allow users to search all appointments.` → All clinics? One selected clinic? Future only? By name or date? ✅ `The system shall allow a receptionist to search appointments across all clinics by patient name, date, doctor, and clinic.`

### Completeness — Password Reset

> ❌ `The system shall allow users to reset passwords.` → Missing: Who can reset? Which identifier? How long is the link valid? Can it be reused? What if email not registered? What are password rules? ✅ `The system shall allow registered students to reset their own passwords via a one-time verification link sent to their registered email, valid for 30 minutes.`

### Consistency Conflict

> FR-12: All faculty members can view student marks. NFR-08: Only the assigned course teacher can view student marks. → These contradict. Go back to stakeholders to resolve.

### Verifiability

> ❌ `The system shall be reliable.` ✅ `The system shall maintain 99.5% uptime during each academic semester, excluding scheduled maintenance windows.`

### Atomic — Don't bundle actions

> ❌ `The system shall allow students to register, login, reset password, and update profile.` ✅ Split into:
> 
> - FR-01: The system shall allow new students to register.
> - FR-02: The system shall allow registered students to log in.
> - FR-03: The system shall allow registered students to reset passwords.
> - FR-04: The system shall allow logged-in students to update their profiles.
> 
> Why? If login works but reset fails, which requirement is "complete"? Atomic requirements enable clean testing and tracing.

### Separating FR and NFR

> ❌ `The system shall allow users to search books quickly.` ✅ Split into:
> 
> - FR-10: The system shall allow users to search books by title, author, or ISBN.
> - NFR-04: The system shall display search results within 2 seconds for 95% of requests under 300 concurrent users.

### Vague time

> ❌ `The report should be generated quickly.` ✅ `The system shall generate the report within 3 seconds of the user's request.`

### Vague notification

> ❌ `The system will notify users.` ✅ `The system shall send a pop-up notification to the assigned student when a new task is created by the advisor.`

### Vague usability

> ❌ `The system should be user-friendly.` ✅ `The system shall allow a first-time user to submit a leave application within 3 minutes after logging in, without external assistance.`

### Design decision disguised as NFR

> ❌ `The system shall use MongoDB to store student records.` → This is a tech/architecture decision. Only valid as a requirement if the client explicitly mandates it as a constraint.

### Unquantifiable quality

> ❌ `The system shall have an attractive UI.` → Attractiveness cannot be measured. Rephrase as a usability metric (e.g., task completion rate, error count).

---

## Quick Reference: FR vs NFR

|Aspect|FR|NFR|
|---|---|---|
|Focus|What system does|How system behaves / quality|
|Keyword hint|Action verbs (search, generate, authenticate)|"-ty" words (security, reliability...)|
|Must be|Complete + Consistent|Quantified + Testable|
|Workaround if unmet|Usually possible|Usually not possible|
|Example|System authenticates users via username + password|System shall respond to login within 2 seconds|

---

_Sources: Sommerville — Software Engineering 9th Ed. Ch.4 | SWE 4401 class notes | FR Writing Styles lecture slides_
## Requirement Engineering (RE) Process

RE is the formal process of defining system needs. Done **before** any tech stack decisions.

```
        ┌─────────────┐
   ┌───►│  Elicitation│ ← gathering from stakeholders
   │    └──────┬──────┘
   │           │
   │    ┌──────▼──────┐
   │    │Specification│ ← converts needs to SRS (user → system reqs)
   │    └──────┬──────┘
   │           │
   │    ┌──────▼──────┐
   └────│  Validation │ ← checks correctness & acceptability
        └─────────────┘
          (incremental / spiral — no definite end)
```

---

## Elicitation

How to identify requirements:

1. **Identify stakeholders**
2. **Do basic research** — analyze competitors, product environment, etc.
3. **JAD (Joint Application Development)** — invite stakeholders for open discussion. Different backgrounds → potential conflicts → need **deep elicitation**

### Elicitation Techniques

|Technique|Description|
|---|---|
|One-on-one interviews|Usually done with product owner only (not scalable)|
|Focus groups|Small group discussion|
|Surveys & questionnaires|Broad reach|
|Observation (Passive/Active)|Watch users work|
|Participatory Observation|Analyst works alongside users|
|Visual Methods|Diagrams, prototypes|

**Interview question types:**

- **Open-ended:** "What will the user do in the system?" (broad, exploratory)
- **Closed:** "The password must be 5 characters — is this right?" (yes/no)
- **Probing:** Follow-up to dig deeper
- **Scenario-based:** Not just direct questions — situational ("what if…")

> ⚠️ Exam style: scenario-based, not just direct questions. Emphasis on identifying FR/NFR from a given scenario.

---

## Specification

How to write requirements:

|Format|Use Case|
|---|---|
|**Natural Language**|Numbered sentences; for non-technical stakeholders|
|**Structured Natural Language**|Template-based; for developers|
|**Graphical Notations**|UML, use cases, sequence diagrams|
|**Mathematical Specifications**|Finite state machines, sets (formal/precise)|

### Structured Natural Language (SNL)

Standard template format:

```
Function : ____________
Input    : ____________
Output   : ____________
```

Uses **"shall"** or **"will"** — avoids ambiguity.

**Keyword rules:**

- `shall` = **mandatory**
- `should` = **desired / optional**
- Avoid `can` (implies already implemented) and `should` for mandatory things

> Always write: _who asked for this requirement & why_ — makes debugging/testing easier (traceability).

---

## EARS Format (Easy Approach to Requirement Syntax)

|Pattern|Template|
|---|---|
|**Ubiquitous**|`The system shall {do something}`|
|**Event-driven**|`When {event}, the system shall {respond}`|
|**State-driven**|`While {state}, the system shall {behave}`|
|**Optional feature**|`Where {feature included}, the system shall {do something}`|
|**Unwanted behavior**|`If {unwanted condition}, then the system shall {handle it}`|

**Examples:**

- `When the student submits a leave application, the system shall send a confirmation email.`
- `While the server is under maintenance, the system shall display a maintenance notice.`
- `If the student enters the wrong password 5 times, then the system shall lock the account for 30 minutes.`

---

## Good vs Bad Requirements — Examples

### Bad → Good Transformations

**Example 1 (Vague time):**

> ❌ `The report should be generated quickly.`  
> ✅ `The system shall generate the report within 3 seconds of the user's request.`

**Example 2 (Vague notification):**

> ❌ `The system will notify users.`  
> ✅ `The system shall send a pop-up notification to users when a new task is assigned.`

**Example 3 (Vague usability):**

> ❌ `The system should be user-friendly.`  
> ✅ `The system shall allow a first-time user to submit a leave application within 3 minutes after logging in, without external assistance.`

**Example 4 (Multiple actions in one):**

> ❌ `The system shall allow students to register, login & update their profile.`  
> ✅ Split into 3 separate FRs — one per atomic action.

**Example 5 (Missing actor/condition/data):**

> ❌ `The system shall allow student to reset passwords.`  
> ✅ `The system shall allow registered students to reset their own passwords via a verification link sent to their registered email.`

**Example 6 (Unquantifiable):**

> ❌ `The system shall have an attractive UI.`  
> → Can't test attractiveness. Rephrase with measurable usability metric.

**Example 7 (Design decision, not NFR):**

> ❌ `The system shall use MongoDB to store student records.`  
> → This is a tech/design decision. Only becomes a valid requirement if the client explicitly mandates it as a constraint.

---

## 10 Characteristics of a Good Requirement

|#|Characteristic|What it means|
|---|---|---|
|1|**Clear**|No ambiguous language|
|2|**Unambiguous**|Only one possible interpretation|
|3|**Complete**|All necessary info included|
|4|**Consistent**|No contradictions with other requirements|
|5|**Verifiable**|Can be tested|
|6|**Feasible**|Technically & financially achievable|
|7|**Necessary**|Stakeholder actually needs it|
|8|**Modular / Atomic**|One requirement per sentence|
|9|**Traceable**|Has ID, source, reason, priority|
|10|**Modifiable**|Easy to change as the project evolves|

**Traceability fields (when needed):**

```
FR-001 | Requirement text | Source | Reason | Priority
```

---

## Rules for Writing Requirements (Checklist)

- [ ] Use `shall` (mandatory) or `should` (optional) — not vague verbs
- [ ] No vague words: quickly, easily, user-friendly, attractive, etc.
- [ ] One requirement per sentence (atomic)
- [ ] Don't mix FR and NFR in one statement (rare exceptions OK)
- [ ] Specify the **actor** (student, admin, registered user…)
- [ ] Specify the **condition** ("After payment is done, the system shall…")
- [ ] Specify the **data** ("The system shall store student name, ID, and email")
- [ ] Avoid design decisions unless they are real constraints
- [ ] Add rationale/reason where useful

---

## Quick Reference: FR vs NFR

| |FR|NFR|
|---|---|---|
|Focus|What system does|How system behaves|
|Keyword hint|Action verbs (search, generate, authenticate)|"-ty" words (security, reliability…)|
|Must be|Complete + Consistent|Quantified + Testable|
|Workaround if unmet|Usually possible|Usually **not** possible|
|Example|System authenticates users via username + password|System shall respond to login requests within 2 seconds|

---
