# Software Requirement & Specification (SRS)

**Course:** SWE 4401 | **Textbook:** Sommerville Ch.4

---

## Why SRS?

> Simple miscommunication → major project failures. SRS forces everyone to agree on _what_ is being built _before_ building it.

**Cost of fixing a bug by SDLC phase:**

|Phase|Relative Cost|
|---|---|
|Requirement Analysis|$1|
|Testing|$100|
|Deployment & Maintenance|$1000|

A mistake caught at requirements costs $1. The same mistake found after deployment costs $1000. This is why getting requirements right early is critical.

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

These three are almost the same concept but focus on different levels of detail.

|Term|Meaning|Example (Login)|
|---|---|---|
|**Requirement**|What/Why is needed|User needs to log in to access the system|
|**Feature**|How to fulfill it|A login form with username and password fields|
|**Specification**|Detailed constraints about the feature|Username must be a valid email; password must be 8+ chars; 3 failed attempts = lockout|

> Think of it as: Requirement = the need. Feature = the solution. Specification = the rules of the solution.

---

## Two Types of Requirements

### Functional Requirement (FR)

What the system **does** — specific services, inputs, outputs, behavior.

**Must be:**

- **Complete** — developers can implement it and testers can verify it without filling any gaps
- **Consistent** — no two FRs contradict each other

**Examples (MHC-PMS Medical System):**

```
FR-01: A user shall be able to search the appointment list for all clinics.
FR-02: The system shall generate each day, for each clinic, a list of patients
       expected to attend appointments that day.
FR-03: Each staff member using the system shall be uniquely identified by
       their eight-digit employee number.
```

**Examples (University system):**

```
FR-01: The system shall allow registered students to submit leave applications.
FR-02: The system shall allow course teachers to upload grades for their assigned courses.
FR-03: The system shall allow admins to generate semester-end result sheets.
```

**Why completeness matters — bad example:**

> `FR: A user shall be able to search the appointment list for the clinics.`
> 
> Is "the clinics" = all clinics? One specific clinic the user selects? Doesn't say.
> 
> - One developer builds: patient enters name → searches every clinic
> - Another developer builds: patient selects a clinic first → then searches within it
> 
> Same sentence. Two completely different systems. This is an incomplete FR.

---

### Non-Functional Requirement (NFR)

How the system **behaves** — quality, performance, constraints.

**Key rule: Every adjective must be quantified.**

Keyword hint: NFR topics usually end in **"-ty"** → security, reliability, scalability, usability, availability, portability.

**More critical than individual FRs:** A system that fails an NFR is often entirely unusable.

- FR not met → usually a workaround exists
- NFR not met → usually **no workaround**; the entire system must comply

> Example: An aircraft that fails its reliability NFR won't be certified for flight. There's no workaround.

**Examples:**

```
Bad NFR:  The system should be easy to use by medical staff.
Good NFR: Medical staff shall be able to use all system functions after 4 hours
          of training. After training, the average number of errors made by
          experienced users shall not exceed 2 per hour of system use.

Bad NFR:  The system shall be fast.
Good NFR: The system shall respond to any user login request within 2 seconds
          under normal load (up to 500 concurrent users).

Bad NFR:  The system should be secure.
Good NFR: The system shall require multi-factor authentication for all admin
          logins. All passwords shall be stored using bcrypt hashing with a
          minimum cost factor of 12.

Bad NFR:  The system shall be reliable.
Good NFR: The system shall maintain 99.5% uptime during each academic semester,
          excluding scheduled maintenance windows announced 48 hours in advance.
```

---

## NFR Categories (3 Types)

```
NFR
├── Product NFR       → Quality of the system itself
│     Performance, Usability, Security, Reliability, Space
│
├── Organizational NFR → Internal company rules
│     Internal policies, dev process standards, tech stack constraints
│     (e.g. "must use Python", "must follow ISO standard")
│
└── External NFR      → Imposed from outside
      Legal requirements, safety regulations, environmental constraints
      (e.g. privacy law, banking regulations)
```

**Real examples from the MHC-PMS system:**

```
Product NFR (Availability):
  The system shall be available to all clinics during normal working hours
  (Mon–Fri, 08:30–17:30). Downtime within normal working hours shall not
  exceed 5 seconds in any one day.

Organizational NFR (Authentication):
  Users of the MHC-PMS system shall authenticate themselves using their
  health authority identity card.

External NFR (Legal/Privacy):
  The system shall implement patient privacy provisions as set out in
  HStan-03-2006-priv.
```

**NFR Metrics — how to quantify each type:**

|Property|How to Measure|
|---|---|
|Speed|Transactions/second, response time in ms|
|Size|MB, number of storage chips|
|Ease of use|Training time in hours, errors per hour|
|Reliability|Mean time to failure, % uptime|
|Robustness|Time to restart after failure|
|Portability|% of target-dependent code statements|

---

## User Requirements vs System Requirements

**Analogy:**

- **User requirement** = "I want a house with 3 bedrooms and a garden." _(what the client says)_
- **System requirement** = The architect's blueprint: exact room dimensions, load-bearing wall specs, wiring plans. _(what the builder actually needs)_

The client doesn't need to understand load calculations. The builder can't work from "I want 3 bedrooms." Both documents are necessary, for different people.

|Aspect|User Requirements|System Requirements|
|---|---|---|
|**Who reads it**|Client, manager, non-technical stakeholders|Developers, architects, testers|
|**Written in**|Natural language + simple diagrams|Detailed, precise technical description|
|**Source**|Directly from stakeholders|Derived and expanded from user requirements|
|**Length per point**|Max 1–2 sentences|Multiple sub-points per user requirement|
|**Jargon**|None — must be understandable without technical background|Allowed — written for developers|
|**Ambiguity**|Some acceptable|Must be eliminated|

**Numbering convention:**

```
User Req 1     ──► System Req 1.1, 1.2, 1.3 ...
User Req 2     ──► System Req 2.1, 2.2, 2.3 ...
```

**Full concrete example:**

```
USER REQUIREMENT (1 sentence — for the client/manager to read):
  1. The MHC-PMS shall generate monthly management reports showing
     the cost of drugs prescribed by each clinic during that month.

SYSTEM REQUIREMENTS (derived from it — for developers to implement):
  1.1  On the last working day of each month, a summary of drugs prescribed,
       their cost, and the prescribing clinics shall be generated.
  1.2  The system shall automatically generate the report for printing
       after 17:30 on the last working day of the month.
  1.3  A report shall be created for each clinic listing individual drug names,
       total prescriptions, number of doses, and total cost.
  1.4  If drugs are available in different dose units (e.g. 10mg, 20mg),
       separate reports shall be created for each dose unit.
  1.5  Access to all cost reports shall be restricted to authorized users
       listed on a management access control list.
```

> One vague user-level sentence becomes 5 precise, independently testable system requirements. Each 1.x requirement can be verified on its own.

---

## Stakeholders

**Definition:** Anyone affected by or involved in the system — not just the end-users.

**Example — a nationwide medical system:**

> Stakeholders = patients, doctors, nurses, receptionists, hospital management, accountants, IT staff, legal department, Bangladesh legal system. Each of these groups has _different_ requirements that may even conflict.

**Example — an overbridge project:**

> Stakeholders = city corporation (commissioning it), pedestrians using it, drivers on the road underneath.

### 4 Types

|Type|Who|Example (Medical System)|
|---|---|---|
|**Internal**|Part of the company building/commissioning|Hospital management, IT staff|
|**External**|Outside the company; affected by the system|Patients, suppliers|
|**Primary**|Directly impacted (positive or negative)|Doctors, patients — they use it daily|
|**Secondary**|Indirectly impacted|Legal regulators, general public|

**Priority order for gathering requirements:**

```
Primary → Secondary → External → Internal
```

Start with who is most directly affected — their requirements drive everything else.

---

## Requirement Engineering (RE) Process

RE is the formal process of defining what a system must do, done **before** any tech stack decisions.

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
         (cycle repeats — incremental/spiral)
```

It is a spiral — you do not finish elicitation before starting specification. You loop back as understanding deepens.

**Full RE Process (Sommerville):**

1. **Feasibility Study** — Should we build this at all? (short focused study)
    - Does it meet organizational objectives?
    - Is it feasible within schedule/budget using current tech?
    - Can it integrate with existing systems?
    - If any answer is No → probably don't proceed.
2. Requirements Elicitation & Analysis
3. Requirements Specification
4. Requirements Validation
5. Requirements Management _(ongoing throughout)_

---

## Elicitation

### Steps:

1. Identify stakeholders
2. Do basic research — analyze competitors, product environment
3. JAD (**Joint Application Development**) — invite stakeholders for open discussion. Different backgrounds → conflicts → need **deep elicitation**

### Sub-activities inside Elicitation & Analysis:

1. **Requirements Discovery** — interact with stakeholders, gather raw requirements
2. **Classification & Organization** — group related requirements, link to system architecture
3. **Prioritization & Negotiation** — resolve conflicts through compromise
4. **Specification** — document what has been gathered

### Why Elicitation is Hard:

|Problem|Example|
|---|---|
|Stakeholders can't articulate needs|"I just want it to be better than the old system"|
|Domain jargon barriers|A librarian says "all acquisitions must be catalogued before shelving" — obvious to them, invisible to the analyst|
|Conflicting needs|Doctors want quick access to all records; privacy law says restrict access|
|Political factors|A manager requests features that expand their department's influence|
|Changing environment|New privacy legislation passed mid-project changes requirements|

### Elicitation Techniques

|Technique|Description|Best For|
|---|---|---|
|**One-on-one interviews**|Direct Q&A with a stakeholder|Product owner — getting deep detail|
|**Focus groups**|Small group discussion|Surfacing conflicting views across stakeholders|
|**Surveys & questionnaires**|Written questions sent broadly|Broad reach, initial data gathering|
|**Observation (Passive)**|Watch users work without interfering|Revealing actual vs. stated behavior|
|**Observation (Active/Participatory)**|Analyst joins and does the work alongside users|Deeper insight into implicit workflows|
|**Ethnography**|Immersive long-term observation in the work environment|Discovering unstated/implicit requirements|
|**Scenarios**|Walk through a real-life usage story|Making abstract needs concrete|
|**Use Cases**|Formal diagram of actor-system interactions|Capturing all interaction types systematically|
|**Prototyping**|Build a rough version for users to react to|Validation and discovery|

**Why Ethnography is uniquely powerful:**

- Air traffic controllers were found to _deliberately_ put aircraft on conflicting paths briefly as a control strategy — directly contradicting the procedures. A formal interview would never reveal this. Observation did.
- Analysts discovered that work groups self-covered for absent members — an implicit coordination requirement never written in any procedure.

### Interview Question Types with Examples:

**Open-ended** (broad, exploratory):

> "What will the student do in this system on a typical day?" "Walk me through how you currently process a leave request."

**Closed** (yes/no confirmation):

> "Should the password be exactly 8 characters, or at least 8 characters?" "Does the report need to be generated automatically, or triggered manually?"

**Probing** (follow-up dig):

> "You mentioned the system should notify advisors — what exactly should the notification say?" "Why is it important that only the course teacher can view marks, and not all faculty?"

**Scenario-based** (situational):

> "What should happen if a student submits a leave application but the advisor's account is inactive?" "If the server goes down mid-payment, what should the user see?"

---

## Scenarios & Use Cases

### Scenarios

A scenario walks through one specific interaction from start to finish. It is concrete and story-like — useful for getting stakeholders to engage.

**Structure of a scenario:**

1. Initial assumption (starting state — who is logged in, what data exists)
2. Normal flow of events (step by step)
3. What can go wrong + how it is handled
4. Other concurrent activities
5. System state at completion

**Example — Collecting Medical History (MHC-PMS):**

```
INITIAL ASSUMPTION:
  Patient has seen receptionist who created their record. Nurse is logged in.

NORMAL FLOW:
  Nurse searches patient by surname. If multiple matches, uses given name + DOB.
  Nurse selects "Add Medical History" from menu.
  System prompts for: consultations elsewhere (free text), existing conditions
  (menu), current medication (menu), allergies (free text), home life (form).

WHAT CAN GO WRONG:
  Patient record not found → nurse creates new record.
  Condition not in menu → nurse selects "Other" and enters free text.
  Patient refuses to provide history → nurse logs refusal. System prints
  standard exclusion form for patient to sign.

OTHER ACTIVITIES:
  Record can be viewed (not edited) by other staff during entry.

COMPLETION STATE:
  Nurse still logged in. Patient record with full medical history saved.
  Log entry created with session start/end time and nurse ID.
```

---

### Use Cases (UML)

Formally captures every possible actor-system interaction type.

**Notation:**

- **Actors** = stick figures (humans or other systems)
- **Use cases** = named ellipses ("Register Patient", "Generate Report")
- Lines = connect actors to the interactions they are involved in

**Example — MHC-PMS use cases:**

```
Actors: Medical Receptionist, Nurse, Doctor, Manager

Use Cases:
  ┌─────────────────────────────┐
  │  Register Patient           │ ← Medical Receptionist
  │  View Personal Info         │ ← Medical Receptionist, Doctor
  │  Edit Record                │ ← Nurse, Doctor
  │  Setup Consultation         │ ← Doctor
  │  Generate Report            │ ← Manager
  │  Export Statistics          │ ← Manager
  └─────────────────────────────┘
```

**Limitation:** Use cases are great for discovering FR. Not useful for NFR, organizational, or domain-level constraints.

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

**Example with rationale:**

```
3.2  The system shall measure blood sugar and deliver insulin, if required,
     every 10 minutes.
     (Rationale: Changes in blood sugar are relatively slow so more frequent
     measurement is unnecessary; less frequent measurement could lead to
     unnecessarily high sugar levels.)

3.6  The system shall run a self-test routine every minute with conditions
     and actions as defined in Table 1.
     (Rationale: Self-test can discover hardware/software problems and alert
     the user before normal operation becomes impossible.)
```

### Structured Natural Language (SNL) Template

Used for developer-facing system requirements. Forces explicit answers to every field — eliminates ambiguity.

```
Function      : Compute insulin dose — safe sugar level
Description   : Computes dose of insulin when current sugar level is in the
                safe zone (3–7 units)
Inputs        : Current reading (r2), previous two readings (r0, r1)
Source        : r2 from sensor; r0, r1 from memory
Outputs       : CompDose — insulin dose to be delivered
Destination   : Main control loop
Action        : CompDose = 0 if sugar is stable, falling, or rising but
                slowing. If rising and accelerating: CompDose = round((r2-r1)/4).
                If rounded result = 0 → set CompDose = MinimumDose.
Pre-condition : Insulin reservoir contains at least the maximum single dose.
Post-condition: r0 replaced by r1; r1 replaced by r2.
Side effects  : None
```

---

## Modal Verbs — Shall / Should / May

|Verb|Meaning|Example|
|---|---|---|
|**SHALL**|Mandatory — must be implemented|`The system SHALL encrypt all passwords using bcrypt before storage.`|
|**SHOULD**|Desirable — expected if feasible|`The system SHOULD provide a dark mode for low-light environments.`|
|**MAY**|Optional — allowed but not required|`The system MAY allow users to upload a profile picture.`|

**Avoid:**

- `can` — implies the feature already exists
- `will` — ambiguous tense, not a commitment
- `should` for things that are actually mandatory

**Example of wrong modal causing problems:**

```
Wrong: The system can notify the advisor when a student submits leave.
       → "can" implies the feature already exists or is optional.
       → A developer might skip implementing it.

Correct: The system shall notify the advisor when a student submits leave.
```

---

## Vague Words — Replace With Measurements

|Vague Word|Replace With|Example|
|---|---|---|
|fast|within N seconds|within 2 seconds|
|easy|complete task within N minutes|first-time user completes in 3 minutes|
|secure|specific mechanism|MFA required for all admin logins|
|reliable|% uptime|99.5% uptime per semester|
|soon|within N minutes|within 1 minute of trigger|
|large|up to N MB/GB|up to 10 MB per file upload|
|quickly|within N seconds for Y records|within 5 seconds for 2,000 records|
|user-friendly|measurable usability metric|max 2 errors/hour after 4 hours training|

**Full transformation example:**

```
Bad:  The system shall generate reports quickly.

Good: The system shall generate the semester performance report within
      5 seconds for up to 2,000 student records.
```

---

## EARS Format (Easy Approach to Requirement Syntax)

A lightweight syntax for writing natural-language requirements without ambiguity.

|Pattern|Template|Example|
|---|---|---|
|**Ubiquitous (Always)**|`The system shall [response].`|`The system shall store all student records persistently in the database.`|
|**Event-driven**|`When [event], the system shall [response].`|`When a student submits a leave application, the system shall send a notification to their assigned advisor within 1 minute.`|
|**State-driven**|`While [state], the system shall [response].`|`While the course registration period is closed, the system shall prevent students from adding or dropping courses.`|
|**Optional Feature**|`Where [feature included], the system shall [response].`|`Where the analytics module is enabled, the system shall log all user sessions with timestamps.`|
|**Unwanted Behavior**|`If [unwanted condition], then the system shall [response].`|`If a registered student enters an incorrect password 5 consecutive times, the system shall lock that account for 30 minutes and notify the student via email.`|

**Why EARS helps:** Without EARS: `The system should handle wrong passwords.` → How? How many? Lock forever or temporarily? Notify? None of this is specified.

With EARS (Unwanted Behavior pattern): `If a student enters an incorrect password 5 consecutive times, the system shall lock the account for 30 minutes and send a password reset email to the registered address.` → Everything is defined. One interpretation only.

---

## Requirements Validation

After writing requirements, you validate them — checking that they define the system the client actually wants.

### 5 Validation Checks with Examples:

**1. Validity** — Does this reflect real user needs?

```
Requirement: The system shall support 15 different report export formats.
Validity check: Do users actually need 15 formats? Interview users — maybe
only PDF and Excel are ever used. The other 13 are waste.
```

**2. Consistency** — Does it contradict another requirement?

```
FR-12:  All faculty members can view student marks.
NFR-08: Only the assigned course teacher can view student marks.
→ These directly contradict each other. One must be changed.
```

**3. Completeness** — Are all required behaviors covered?

```
Incomplete: The system shall allow users to reset passwords.
Missing:
  - Who can reset? (registered students only? anyone?)
  - What identifier is used? (email? student ID?)
  - How long is the reset link valid?
  - Can the reset link be reused?
  - What if the email is not registered?
  - What password complexity rules apply after reset?
```

**4. Realism** — Can this actually be implemented?

```
Unrealistic: The system shall respond to any query within 0.001 seconds
             for 1 million concurrent users.
→ Not achievable with current technology within a normal budget. Must revise.
```

**5. Verifiability** — Can a tester write a test for it?

```
Not verifiable: The system shall be reliable.
→ No test can pass or fail this. "Reliable" is undefined.

Verifiable: The system shall maintain 99.5% uptime per academic semester,
            excluding scheduled maintenance windows.
→ A tester can measure uptime over a semester and objectively pass or fail it.
```

### Validation Techniques:

**Requirements Reviews:** A team reads the requirements document together and checks for errors, contradictions, and gaps. Issues are recorded and sent back for renegotiation.

**Prototyping:** Build a rough working model and put it in front of stakeholders. Users can react to something they can see and click, not just imagine from text.

> Often reveals: "Oh, this isn't what I meant at all" — after seeing it live.

**Test-case generation:** Write test cases for each requirement during validation, before any code exists. If you cannot write a test, the requirement is unclear and must be fixed.

> Extreme Programming (XP) makes this mandatory — tests are written before code.

---

## Requirements Management

Requirements always change — new hardware, legislation, shifting business priorities, new stakeholders. Requirements management tracks and controls those changes.

### Why Requirements Change — Examples:

|Reason|Example|
|---|---|
|Business environment changes|A new privacy law is passed; the system must now anonymize all exported data|
|Technical environment changes|A new mobile platform is released; the system needs a mobile interface|
|Payers ≠ users|Management funds the system but wants dashboard reports; doctors (users) want quick note-taking tools|
|Large diverse user base|Students want a dark mode; faculty want detailed audit logs; admins want export tools|
|Growing understanding|After using the prototype, the client realizes they need role-based access, not just one admin account|

### Change Management Process (3 steps):

```
Identified Problem/Change Request
         │
         ▼
1. Problem Analysis & Change Specification
   └── Is this a real problem? Is the change valid?
       Clarify and document exactly what change is needed.
         │
         ▼
2. Change Analysis & Costing
   └── Which requirements are affected?
       Which system design and code modules need changes?
       What does it cost (time + money)?
       Is it worth it?
         │
         ▼
3. Change Implementation
   └── Update requirements doc first.
       Then update design.
       Then update code.
       Then re-test.
         │
         ▼
Revised Requirements Document
```

> Never modify the system first and update requirements later — they will get out of sync. Requirements doc must always reflect what the system actually does.

### Planning Needs:

|Item|Purpose|
|---|---|
|Unique IDs for every requirement (FR-001, NFR-003...)|Cross-referencing, traceability, change tracking|
|Traceability policies|Define relationships between requirements and between requirements and design|
|Tool support|Spreadsheets for small projects; dedicated RM tools for large ones|

---

## The SRS Document Structure (IEEE Standard)

The SRS is the official statement of what the system must do. It serves multiple audiences at once.

**Who reads it and why:**

|Reader|Uses SRS to...|
|---|---|
|System customers|Specify and review requirements; propose changes|
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

|Failure Type|What Happens|Example|
|---|---|---|
|**Ambiguous**|Different people imagine different systems|"Search all appointments" → one dev searches all clinics, another searches the selected clinic only|
|**Incomplete**|Developers fill gaps with assumptions|"Allow users to reset passwords" → no mention of who, how, expiry, or rules|
|**Inconsistent**|Requirements contradict each other|FR-12 says all faculty can view marks; NFR-08 says only the assigned teacher can|
|**Untestable**|Testers cannot prove completion|"The system shall be reliable" — what does passing this test look like?|

> "If two intelligent people can read the same requirement and imagine two different systems, the requirement is not ready."

---

## 10 Characteristics of a Good Requirement

|#|Characteristic|What it means|Bad Example|Good Example|
|---|---|---|---|---|
|1|**Clear**|No ambiguous language|"System shall notify users"|"System shall send an SMS to the student's registered phone number"|
|2|**Unambiguous**|Only one interpretation|"Search all appointments"|"Search appointments across all clinics by patient name"|
|3|**Complete**|Dev must not have to guess any behavior|"Allow password reset"|"Allow registered students to reset via email link valid for 30 min"|
|4|**Consistent**|No contradictions|FR-12 and NFR-08 above|One single access control rule defined in one place|
|5|**Verifiable**|Can be tested objectively|"System shall be fast"|"System shall respond within 2 seconds for 95% of requests"|
|6|**Feasible**|Achievable within budget and tech|"Respond in 0.001s for 1M users"|"Respond within 2s for up to 500 concurrent users"|
|7|**Necessary**|Stakeholder actually needs it|15 export formats when only 2 are used|PDF and Excel export only|
|8|**Atomic**|One requirement per sentence|"Register, login, and update profile"|Three separate FRs|
|9|**Traceable**|Has ID, source, reason, priority|No ID, no rationale|FR-003, Source: Student Affairs Dept, Priority: High|
|10|**Modifiable**|Easy to change without rewriting everything|Tightly coupled paragraph|Single sentence, modular section|

---

## Rules for Writing Requirements (Checklist)

- [ ] Use `shall` (mandatory) or `should` (optional) — not vague verbs like `can` or `will`
- [ ] No vague words: quickly, easily, user-friendly, attractive, soon, large, fast...
- [ ] One requirement per sentence — atomic
- [ ] Don't mix FR and NFR in one statement
- [ ] Specify the **actor** — not just "user" but "registered student", "admin", "course teacher"
- [ ] Specify the **condition** when needed — "After payment is confirmed, the system shall..."
- [ ] Specify the **data** — "shall store student name, ID, and email" not "shall store student info"
- [ ] Avoid design decisions unless they are real constraints
- [ ] Add rationale — who asked for it and why (helps during change management)

---

## Good vs Bad Requirements — Full Example Bank

### Ambiguity

```
Bad:  The system shall allow users to search all appointments.
      → All clinics? One clinic? Future only? By name? By date?

Good: The system shall allow a medical receptionist to search appointments
      across all clinics by patient name, appointment date, doctor name,
      and clinic name.
```

### Completeness — Password Reset

```
Bad:  The system shall allow users to reset passwords.

Good: The system shall allow registered students to reset their own passwords
      by requesting a one-time reset link sent to their registered email
      address. The link shall expire after 30 minutes and may only be used once.
      If the email address is not registered in the system, the system shall
      display the message: "If this email exists, a reset link has been sent."
```

### Consistency Conflict

```
FR-12:  All faculty members can view student marks.
NFR-08: Only the assigned course teacher can view student marks.

→ These directly contradict. Go back to stakeholders to decide the correct rule,
  then remove one and update the other.
```

### Verifiability

```
Bad:  The system shall be reliable.
      → A tester cannot pass or fail this.

Good: The system shall maintain 99.5% uptime during each academic semester,
      excluding scheduled maintenance windows announced at least 48 hours
      in advance.
      → A tester measures uptime logs over a semester and compares to 99.5%.
```

### Atomic — Don't Bundle Actions

```
Bad:  The system shall allow students to register, login, reset password,
      and update profile.
      → If login works but reset fails, is this requirement met or not?

Good — split into 4 atomic requirements:
  FR-01: The system shall allow new students to register using their
         university email address and student ID.
  FR-02: The system shall allow registered students to log in using
         their email address and password.
  FR-03: The system shall allow registered students to reset their
         password via a link sent to their registered email.
  FR-04: The system shall allow logged-in students to update their
         display name and profile picture.
```

### Separating FR and NFR

```
Bad:  The system shall allow users to search books quickly.
      → "search books" is FR. "quickly" is NFR. Mixing them = can't test either.

Good:
  FR-10:  The system shall allow users to search books by title, author, or ISBN.
  NFR-04: The system shall display search results within 2 seconds for 95%
          of requests under 300 concurrent users.
```

### Vague Time

```
Bad:  The report should be generated quickly.
Good: The system shall generate the monthly drug cost report within 5 seconds
      of the request being triggered.
```

### Vague Notification

```
Bad:  The system will notify users.
Good: When a student's leave application is approved, the system shall send
      an in-app notification and an email to the student's registered address
      within 5 minutes of the advisor's approval action.
```

### Vague Usability

```
Bad:  The system should be user-friendly.
Good: The system shall allow a first-time student user to submit a leave
      application within 3 minutes after logging in, without external
      assistance or help documentation.
```

### Design Decision (Not an NFR)

```
Bad:  The system shall use MongoDB to store student records.
      → This is a tech/architecture decision, not a quality constraint.
      → Only valid if the client explicitly mandates it.

Good: The system shall store all student records in a persistent database
      that supports concurrent reads from at least 200 simultaneous users.
      (Let the dev team choose the right tool.)
```

### Unquantifiable Quality

```
Bad:  The system shall have an attractive UI.
      → Attractiveness cannot be measured. What test proves this?

Good: The system shall receive a usability satisfaction score of at least
      4 out of 5 in post-training user surveys conducted with a sample of
      20 first-time users.
```

---

## Quick Reference: FR vs NFR

|Aspect|FR|NFR|
|---|---|---|
|Focus|What system does|How system behaves / quality|
|Keyword hint|Action verbs (search, generate, authenticate, display)|"-ty" words (security, reliability, scalability...)|
|Must be|Complete + Consistent|Quantified + Testable|
|Workaround if unmet|Usually possible|Usually not possible|
|Scope|Specific feature or function|Often entire system|
|Example|System authenticates users via username + password|System shall respond to any login request within 2 seconds|

---

_Sources: Sommerville — Software Engineering 9th Ed. Ch.4 | SWE 4401 class notes | FR Writing Styles lecture_