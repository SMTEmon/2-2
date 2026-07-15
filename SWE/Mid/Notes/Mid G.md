---
tags:
  - pkm
  - research
  - structured-knowledge
  - software-engineering
  - srs
date: 2026-06-28
type: literature-note
---

# Software Requirement & Specification (SRS) - SWE 4401

> [!info] Course Metadata & Context
> - **Course:** SWE 4401 - Software Requirement & Specification (SRS)
> - **Instructor:** Farzana (`farzana@iut-dhaka.edu`)
> - **Venue:** Room AB2(204)
> - **Evaluation Focus:** Scenario-based implementations over direct theoretical queries.
> - **Exam Prerequisites:** Diagrammatic comprehensive coverage, explicitly assessing scenario assumptions, stakeholder identification, FR, NFR, AC, Use Case Diagrams & Descriptions, and Activity Diagrams.

---

## 1. Software Development Life Cycle (SDLC)

Software engineering pipelines operate across six rigid sequential phases.

> [!example]- SDLC Pipeline Diagram
> ```mermaid
> flowchart LR
>     P[Planning] --> R[Requirement<br>Analysis]
>     R --> D[Design]
>     D --> I[Implementation]
>     I <--> T[Testing]
>     T --> DM[Deployment &<br>Maintenance]
>     
>     classDef highlight fill:#2d3748,stroke:#63b3ed,stroke-width:2px,color:#fff;
>     class R highlight;
> ```

- **Requirement Analysis:** Outputs the **Software Requirement Specification (SRS)** document.
- **Design:** Yields the **Design Document Specification (DDS)**.
- **Implementation & Testing:** Execute in parallel. Testing is driven by developers utilizing Unit Testing and **Test-Driven Development (TDD)**.

### Error Escalation Economics
Systematic requirement analysis mitigates exponential error cost amplification.

> [!warning] Cost of Defect Rectification
> The economic cost of fixing a bug increases drastically the later it is discovered in the SDLC.
> 
> $$ \text{Cost} = \mathcal{O}(10^p) $$ 
> *(Where $p$ represents the progression of phases)*
> 
> - **Planning Phase:** $\sim \$1$
> - **Design Phase:** $\sim \$100$
> - **Deployment Phase:** $\sim \$1000$
> 
> **Conclusion:** Minor miscommunications consistently trigger severe systemic failures.

---

## 2. Requirement Engineering Epistemology

Requirement Engineering formalizes stakeholder needs prior to selecting technological stacks. The discipline partitions software objectives into hierarchical abstractions.

1. **Requirement:** The *"What"* and *"Why"* (e.g., A user needs to log in).
2. **Feature:** The *"How"* (e.g., The methodology to log in).
3. **Specification:** The granular technical constraints and parameter details (e.g., Processing username and password data).

### Functional vs Non-Functional Requirements (FR vs NFR)

> [!abstract]- FR vs NFR Comparison
> | Attribute | Functional Requirements (FR) | Non-Functional Requirements (NFR) |
> | :--- | :--- | :--- |
> | **Definition** | Explicit system behaviors, discrete services, inputs/outputs. | Systemic behavioral characteristics, quality attributes, operational constraints. |
> | **Syntax** | "What the system does" | Ends in "-ty" (Security, Scalability, Reliability). |
> | **Quantification** | Permits technical workarounds. | Mandates strict quantification (e.g., "handle 100,000 concurrent users"). No workarounds. |

**NFR Environmental Pressures:**
1. **Product Constraints:** Quality metrics intrinsic to software (performance, usability).
2. **Organizational Constraints:** Internal corporate policies, operational standards.
3. **External Constraints:** Legal statutes, regulatory safety mandates, external infrastructural dependencies.

### User vs System Parameters
- **User Requirements:** Utilize simple, descriptive natural language. Condensed into maximum 2-sentence declarations. **No technical jargon**.
- **System Requirements:** Translate a singular User Requirement into multiple, highly specialized, and numerically indexed technical components (e.g., *User Req 1* $\rightarrow$ *System Reqs 1.1, 1.2, 1.3*). All System Requirements inherently trace back to initial user stipulations.

### Stakeholder Analysis
Stakeholders encompass any entity interfacing with, impacted by, or involved in the system architecture.
- **Internal vs External:** Internal (managers, owners, government bodies) vs External (clients, patients, suppliers).
- **Primary vs Secondary:** Primary (absorb direct operational impact) vs Secondary (absorb indirect impact).

> [!tip] Stakeholder Priority Rule
> Analytical priority mandates securing **primary stakeholders** first, subsequently mapping secondary and peripheral entities to extract conflict-free requirements.

---

## 3. Requirement Elicitation Methodologies

The Requirement Engineering lifecycle progresses incrementally: **Elicitation (gathering) $\rightarrow$ Specification (documentation) $\rightarrow$ Validation (correctness confirmation)**. 

> [!note]- Deep Elicitation Tactics
> - **Context & Identification:** Brainstorming, document analysis, **Joint Application Development (JAD)** workshops to simultaneously align competing stakeholders to forge consensus.
> - **Interviews:** Deploy open-ended structures for scenarios and closed-ended for binary facts. Probing methodologies ("Why?") force stakeholders to evaluate system depths and navigate conflicted requirements.
> - **Focus Groups:** Capture consensus within localized demographics.
> - **Surveys:** Broad metric gathering capped at 10 minutes. Isolate variables using multiple-choice and ranking matrices. Limit open-ended questions to prevent respondent fatigue.

> [!example]- Observational Ethnography
> System analysts deploy physical shadowing to monitor genuine workflows, correcting reporting biases.
> - **Passive Observation:** Following users and interjecting questions.
> - **Participatory Observation:** Analyst performs the tasks directly.
> 
> *Example:* Hospital staff ignoring digital charts in favor of physical whiteboards necessitates architectural revisions to map digital UI to physical habits.

> [!example]- Visual Tangibility
> Analysts deploy prototyping and wireframing to solidify abstract stakeholder requests. 
> - **Gap Analysis:** Evaluates differences between prototyped interfaces and unfulfilled needs.
> - **Pipeline:** Business Analysts $\rightarrow$ UI/UX Design $\rightarrow$ Developers $\rightarrow$ Quality Assurance (QA).

---

## 4. Requirement Specification Syntax

Specification translates gathered data into standardized architectural rules. All requirements demand **rigorous testability**; ambiguous adjectives like "easy to use" or "fast" are fundamentally invalid and must transform into rigid temporal bounds (e.g., "report generation within 35 seconds").

### Notation Frameworks
1. **Natural Language:** Flexible but high ambiguity risks. Mitigation requires strict enforcement of a *one-requirement-per-sentence* rule. Must use standard declarations ("A user shall..."). "Shall" = mandatory, "Should" = optional.
2. **Structured Natural Language:** Rigid form templates (function, inputs, outputs, preconditions). Eliminates developer interpretation errors.
3. **Graphical Notations:** UML Diagrams (Use Case, Sequence) to map system scope and data flow visually.
4. **Mathematical Specifications:** Finite-state machines and set theory. Unambiguous and verifiable, but rarely used commercially due to client comprehension limits.

### Functional Requirement Structural Imperatives
Valid FRs adhere to strict atomic characteristics. They must be: **Clear, Unambiguous, Complete, Consistent, Verifiable, Feasible, Necessary, Modular, Traceable, and Modifiable**.

*Violation Example:* "The system shall allow students to register, login, and update profiles" violates modularity and must fracture into three independent IDs.

> [!abstract] EARS Framework (Easy Approach to Requirement Syntax)
> Standardizes sentence structures to eliminate linguistic drift.
> - **Ubiquitous Requirement:** _The system shall [action]_
> - **Event-Driven Requirement:** _When [trigger], the system shall [action]_
> - **State-Driven Requirement:** _While [state], the system shall [action]_
> - **Unwanted Behavior Requirement:** _If [exception], the system shall [action]_

---

## 5. Acceptance Criteria (AC) Architecture

Acceptance Criteria establish the binary pass/fail conditions of a system to secure user acceptance. While specifications inform developer construction, AC targets QA testers and clients to objectively measure success limits.
- **Mapping:** Separate FR and AC matrices linked via Mapping Tables (e.g., FR01 correlates to AC01.1, AC01.2).
- **Density:** Standard density requires **5 to 10 distinct ACs** per Functional Requirement. Excessive AC density indicates the parent FR violates modularity.

### Execution Formats

> [!example]- 1. Checklist Format
> Deployed for sequential, linear step verification. Explicitly outlines what actors *must*, *can*, and *must not* execute.
> - [x] Verify login execution
> - [x] Validate period timing
> - [x] Valid course generation
> - [x] Credit limit checks

> [!example]- 2. Gherkin Syntax
> Deployed for branching logic and interdependent scenarios. Enforces strict Given-When-Then formatting.
> ```gherkin
> Given that user is on the log in page
> When user gives username, password and clicks submit
> Then system shall show an error message
> ```

*Completeness validation requires mapping the Happy Path (optimal success), Edge Cases (data voids, format errors), Roles, and Implicit NFR constraints.*

---

## 6. UML Modeling: Use Case Diagrams

Use Case Diagrams (UCD) govern behavioral mappings, linking directly to FR generation from the actor's perspective.
- **Actor Taxonomy:** Actors represent external generalized entities (Primary, Secondary, External System APIs). Permitted to use Generalization (Inheritance, e.g., "IUT Staff" inherits from "Faculty").
- **Use Cases & Boundaries:** Represented via ellipses using Verb + Noun syntax (e.g., *Register Course*). Organized sequentially within the System Box.

### Relationship Matrices
1. **Association:** Direct straight-line connection between an Actor and a Use Case.
2. **Include (`<<include>>`):** One Use Case inherently calls a secondary mandatory Use Case.
3. **Exclude/Extend (`<<extend>>`):** Optional or failure-state deviations.
4. **Generalization:** Hierarchical inheritance structures.

> [!example]- Use Case Diagram Structure
> ```mermaid
> flowchart LR
>     subgraph System Boundary
>         UC1([Login])
>         UC2([Verify Password])
>         UC3([Show Error])
>     end
>     
>     Actor1([User]) --> UC1
>     UC1 -. "<<include>>" .-> UC2
>     UC1 -. "<<extend>>" .-> UC3
>     
>     classDef uClass fill:#2b6cb0,stroke:#2b6cb0,stroke-width:2px,color:#fff,rx:20,ry:20;
>     class UC1,UC2,UC3 uClass;
> ```

To facilitate management review, every node requires a **Use Case Description** table documenting: UC ID, Name, Primary/Secondary Actors, Trigger Events, Pre Conditions, Alternative Pathways, Failure Scenarios, and linked Acceptance Criteria.

---

## 7. UML Modeling: Activity Diagrams

Activity Diagrams track holistic sequential states, modeling intricate business processes and continuous workflows similar to advanced flowcharts.

### Node Topology & Control Structures
- **Starting/End Nodes:** Exactly 1 Starting Node; one or multiple End Nodes.
- **Decision Node:** Routes paths based on conditional Guards (e.g., `[Valid]`, `[Invalid]`).
- **Merge Node:** Consolidates multiple incoming pathways via an **OR** relationship.
- **Fork Node:** Splinters a single sequential process into multiple parallel, concurrent activities.
- **Join Node:** Synchronizes parallel branches via an **AND** relationship.
- **Time Event:** Suspends sequence based on strict temporal triggers.
- **Swimlanes (Partitions):** Vertically segregate tasks across distinct roles, subsystems, or third-party actors.

> [!example]- Venue Booking System (Activity Diagram)
> ```mermaid
> flowchart TD
>     subgraph User [User Swimlane]
>         S(( )) -.-> A[Submit UI Preferences]
>     end
>     
>     subgraph System [Booking System]
>         A --> B{Available?}
>         B -- "[Unavailable]" --> C[Show Error]
>         B -- "[Available]" --> D[Route to Payment]
>     end
>     
>     subgraph Payment [Payment System]
>         D --> F1[\ Fork /]
>         F1 --> P1[Process Payment]
>         F1 --> P2[Prepare Order]
>         P1 --> J1[/ Join \]
>         P2 --> J1
>         J1 --> Z[Finalize Reservation]
>     end
>     
>     Z --> E((( )))
>     C --> E
> ```

### Additional Operational Scenarios
- **E-Commerce Distribution:** E-commerce engine validates details $\rightarrow$ **Fork** splits workflow to Billing Team (invoice) and Warehouse (packaging) $\rightarrow$ Branches **Join** upon David completing payment.
- **SIS Course Registration:** Student queries registration periods $\rightarrow$ System loops until Registrar opens period $\rightarrow$ Concurrent profile loading and course selection $\rightarrow$ Secondary validation node by Academic Advisor before finalization.