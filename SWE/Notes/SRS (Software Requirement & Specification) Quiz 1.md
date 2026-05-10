
**Exam scope:** Up to Elicitation & Specification

---

## Why SRS?

> Simple miscommunication → major project failures. SRS forces everyone to agree on _what_ is being built _before_ building it.

**Cost of fixing a bug by SDLC phase (old study):**

|Phase|Relative Cost|
|---|---|
|Requirement Analysis|$1|
|Testing|$100|
|Deployment & Maintenance|$1000|

∴ Get requirements right early — it's 1000x cheaper.

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

> These three are _almost_ similar but have slight differences. Specification also acts as a constraint.

---

## Two Types of Requirements

### Functional Requirement (FR)

- **What the system does** — specific services, inputs, outputs
- Defines features
- Must be **Complete** and **Consistent**
    - _Complete_: Devs can implement & testers can verify without ambiguity
    - _Consistent_: No two FRs contradict each other

**Bad FR:** `A user shall be able to search the appointment list for the clinics.` → Ambiguous: search _which_ clinic? Every clinic? One specific clinic? Creates different implementations.

### Non-Functional Requirement (NFR)

- **How the system behaves** — quality attributes
- Keyword hint: ends in **"-ty"** (security, scalability, usability, reliability, availability…)
- Must be **quantified** — vague adjectives are not allowed
- Focuses on overall system (sometimes on a specific feature too)

**Bad NFR:** `The system should be easy to use.` → How do you test "easy"? You can't. Must quantify.

**Categories of NFR:**

```
NFR
├── Product NFR       → Performance, Usability, Security, Reliability (quality of system itself)
├── Organizational NFR → Internal policies, standards, tech stack constraints
└── External NFR      → Legal, safety standards, environmental constraints
```

> ⚠️ If you can't fulfill an FR, there's usually a workaround. For NFR — usually **no workaround**. The whole system must comply.

---

## User Requirements vs System Requirements

| |User Requirements|System Requirements|
|---|---|---|
|**Audience**|Non-technical / managers|Developers|
|**Language**|Natural language, diagrams|Detailed technical blueprint|
|**Source**|Stakeholders|Derived from user requirements|
|**Length**|Max 1–2 sentences each|Multiple sub-requirements per user req|

**Mapping:**

```
User Req 1  ──► System Req 1.1, 1.2, 1.3 ...
User Req 2  ──► System Req 2.1, 2.2, 2.3 ...
```

**Example (from Mentcare system):**

> **User Req:** The Mentcare system shall generate monthly management reports showing the cost of drugs prescribed by each clinic during that month.

→ **System Reqs (1.1 – 1.5):** Generated on last working day, after 17:30, per clinic, with drug names/doses/costs, access restricted to authorized users.

---

## Stakeholders

**Definition:** Anyone affected by or involved in the system.

### Types

|Type|Description|
|---|---|
|**Internal**|Employees, managers, owners — people building/commissioning the system|
|**External**|Users of the system, clients, suppliers — people affected by it|
|**Primary**|Directly impacted (positive or negative)|
|**Secondary**|Indirectly impacted|

**Priority order for gathering requirements:** `Primary → Secondary → External → Internal`

**Example (nationwide medical system):** Stakeholders = patients, doctors, hospital management, staff, accountants, legal department, legal system of Bangladesh.

---

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
