# Use Case Description — IUT Medical Center Digitalization

**Islamic University of Technology** Department of Computer Science and Engineering (CSE)

B.Sc. in Software Engineering SWE 4402: Software Requirements & Specifications Lab Summer, 2024-25

**Group 2**

|Name|Student ID|
|---|---|
|S. M. Tahsinuzzaman Emon|230042104|
|S. M. Samiul Hossain|230042110|
|S. M. Raiyan Al Arafat|230042134|
|Ayman Rahman Bhuiyan|230042140|
|Shudipto Sarwar|230042142|
|Mohammad Faiaj Jarif|230042152|

**Instructors** Farzana Tabassum — Junior Lecturer, CSE Sohail Ahmed — Assistant Professor, CSE

**Date:** 21 July, 2026

---

## Business Requirements Referenced

|ID|Business Requirement|
|---|---|
|BR-01|Treatment must never be delayed by identity, eligibility, or administrative checks — life priority overrides paperwork.|
|BR-02|Full medical services are reserved for IUT members and their registered families; institutional resources must be protected from unauthorised use.|
|BR-03|Every clinical and administrative action must be attributable to a named staff member and carry a timestamp, for accountability and audit.|
|BR-04|Escalation decisions (ambulance, transfer, referral) are a doctor's clinical responsibility; any deviation must be recorded and reviewable.|
|BR-05|The medical centre must provide 24-hour coverage for residential halls, including after-hours response.|
|BR-06|IUT subsidises external treatment only through affiliated hospitals and only against a valid IUT referral; the patient bears cost beyond the affiliated rate.|
|BR-07|A patient's medical history must remain a single connected record across emergency, diagnostic, and referral processes.|
|BR-08|Patients are entitled to access and obtain their own referral documentation.|
|BR-09|Guests receive first-aid-level care only, under the sponsorship of an identified eligible IUT person.|
|BR-10|Availability of critical response resources (ambulance, on-duty driver, on-call doctor) must be known at all times.|
|BR-11|Students on IUT-authorised off-campus activities remain under the medical centre's duty of care.|

---

## Actor Model

|Actor|Role in the System|
|---|---|
|Medical Staff|Generalised clinical role. Doctor and Nurse are specialisations of this actor.|
|Doctor|Specialisation of Medical Staff. Sole authority for diagnosis, referral, test orders, emergency outcome, and ambulance authorisation.|
|Nurse|Specialisation of Medical Staff. After-hours hotline response, on-site care, and fallback ambulance authorisation.|
|Lab Technician|Performs on-campus diagnostic tests and records results. Cannot order tests.|
|Admin|Maintains the affiliated-hospital directory and settles referral costs.|
|Patient / Attendant|Care recipient, or the person accompanying and declaring on their behalf.|
|University Directory «external system»|Verifies Student, Staff, and Family IDs.|
|IUT Finance Dept «external system»|Receives and settles cost determinations.|
|Time «secondary system actor»|Clock-driven trigger for treatment-period lapse (UC-26). No human initiator.|

---

## Usecase Table

|Use Case ID|Use Case Title|Very Short Description|
|---|---|---|
|UC-01|Register Patient Visit|Register a patient visit and route it to the correct workflow.|
|UC-02|Confirm Visit Category|Confirm or change the patient's visit category.|
|UC-03|Open Temporary Patient Record|Create a temporary patient record when identity is unknown.|
|UC-04|Reconcile Patient Identity|Link a temporary record to the patient's verified identity.|
|UC-05|Verify Eligibility|Determine the patient's eligibility for medical services.|
|UC-06|Record Guest Sponsor|Record the IUT sponsor for a guest patient.|
|UC-07|Create Emergency Record|Create an emergency treatment record.|
|UC-08|Log Vitals & Procedures|Record emergency vitals and procedures performed.|
|UC-09|Record Emergency Outcome|Record the outcome of emergency treatment.|
|UC-10|Create Linked Diagnosis Record|Create a diagnosis record from an emergency case.|
|UC-11|Create Hospital Transfer|Create a hospital transfer record.|
|UC-12|Record Ambulance Dispatch|Record ambulance dispatch details.|
|UC-13|Record Ambulance Return|Record the ambulance's return to the medical center.|
|UC-14|Create Diagnosis Record|Create a diagnosis consultation record.|
|UC-15|Order Diagnostic Test|Request a diagnostic test for a patient.|
|UC-16|Record Diagnostic Test Result|Record the results of a diagnostic test.|
|UC-17|Create Referral|Create a referral to an external hospital.|
|UC-18|Authorise Ambulance Dispatch as Nurse Fallback|Allow a nurse to authorize ambulance dispatch when necessary.|
|UC-19|Record Medicine Dispensing|Record medicines dispensed to a patient.|
|UC-20|Log Off-Campus Trip Incident|Record a medical incident during an authorized trip.|
|UC-21|Create Follow-Up Visit|Create a follow-up visit linked to previous treatment.|
|UC-22|Update Patient Medical History|Update the patient's medical history.|
|UC-23|View Patient Medical History|View a patient's complete medical history.|
|UC-24|Issue Referral Copy|Provide a copy of the referral to the patient.|
|UC-25|Record Ambulance Availability|Update ambulance availability status.|
|UC-26|Expire Treatment Period|Automatically expire an active treatment period.|
|UC-27|Manage Affiliated Hospital Directory|Maintain the list of affiliated hospitals.|
|UC-28|Manage Hospital Referral Rates|Maintain referral cost rates for hospitals.|
|UC-29|Record Hospital Admission|Record a patient's hospital admission.|
|UC-30|Record Hospital Discharge|Record a patient's hospital discharge.|
|UC-31|Record Hospital Bill|Record hospital billing information.|
|UC-32|Verify Referral|Verify the validity of an IUT referral.|
|UC-33|Record Referral Completion|Record completion of a hospital referral.|
|UC-34|Record Payment Claim|Record a reimbursement/payment claim.|
|UC-35|Settle Hospital Referral Cost|Calculate and settle referral costs.|
|UC-36|Send Cost Determination to Finance|Send finalized cost details to Finance.|
|UC-37|Record Refundable Admit Fee|Record refundable admission fees.|
|UC-38|Apply Non-Affiliated Cost Cap|Apply the cost cap for non-affiliated hospitals.|
|UC-39|Apply Hospital Cost Coverage Rules|Apply the hospital cost-sharing rules.|

---

# Use Case Descriptions

## Section 01 — Intake, Triage & Eligibility

### UC-01: Register Patient Visit

**Use Case ID:** UC-01 **Use Case Name:** Register Patient Visit **Primary Actor:** Medical Staff **Secondary/External Actor:** Patient / Attendant, University Directory «external»

**Description:** Opens a visit record for a person presenting at the medical centre, captures the declared and staff-confirmed case category, and routes the case into the Emergency or Diagnosis workflow.

**Trigger:** A patient, or an attendant on their behalf, presents at the medical centre.

**Preconditions:**

1. Registering staff member is authenticated and holds Medical Staff privileges.
2. The medical centre is open or on after-hours duty.

**Postconditions:**

1. A visit record exists with a unique visit ID, the confirmed category, registering staff ID, and arrival timestamp.
2. The case is routed to the Emergency or Diagnosis workflow per the confirmed category.
3. Identity status is recorded as confirmed or temporary token.

**Alternate Flow:**

- AF-1: Patient's identity cannot be established → «extend» UC-03 (Open Temporary Patient Record); registration continues without interruption.
- AF-2: Patient is unconscious or unable to speak → the attendant supplies the declared category; declaration source recorded as attendant.
- AF-3: Patient already has a medical history → staff link the new visit to the existing patient ID instead of creating a new patient.
- AF-4: Staff disagrees with the declared category → «include» UC-02 (Confirm Visit Category) records the override and its author.

**Exception Flow:**

- EF-1: University Directory unreachable → visit registered with provisional identity status and flagged for verification; treatment is not blocked.
- EF-2: An active, unclosed visit already exists for the same patient → the system warns and offers to resume it rather than create a duplicate.
- EF-3: Eligibility check returns Complete Outsider → the visit is closed with outcome refused-ineligible, logged with the deciding staff member.

**Related Requirement:** FR-01, FR-02, FR-03, FR-04 (extends to FR-10)

**Business Requirement:** BR-01, BR-03, BR-07

**Acceptance Criteria:**

1. The system rejects any attempt to save a visit record without a confirmed category of Emergency or Diagnosis.
2. Declared and confirmed categories are stored as separate fields; where they differ, the overriding staff ID and timestamp are retrievable.
3. Every saved visit record carries a unique visit ID, arrival timestamp, and registering staff ID.
4. A visit registered with unknown identity saves successfully and is marked with a temporary token.

### UC-02: Confirm Visit Category

**Use Case ID:** UC-02 **Use Case Name:** Confirm Visit Category **Primary Actor:** Medical Staff **Secondary/External Actor:** Patient / Attendant

**Description:** Captures the category declared at arrival, allows medical staff to confirm or override it after re-assessment, and fixes the final category that determines workflow routing.

**Trigger:** Included during UC-01 (Register Patient Visit), at the point the case category must be settled.

**Preconditions:**

1. A visit record is open and awaiting category confirmation.
2. The acting user holds Medical Staff privileges.

**Postconditions:**

1. Declared category, confirmed category, deciding staff ID, and decision timestamp are stored.
2. Where declared and confirmed differ, the record is marked staff-overridden.

**Alternate Flow:**

- AF-1: Staff confirm the declared category unchanged → both fields hold the same value; no override flag is set.
- AF-2: Staff downgrade Emergency → Diagnosis after re-assessment → override recorded with the deciding staff member.
- AF-3: Staff upgrade Diagnosis → Emergency → override recorded and the case is re-routed immediately to the Emergency workflow.
- AF-4: No declaration obtainable (unconscious, no attendant) → declared field recorded as not declared; staff assessment alone sets the category.

**Exception Flow:**

- EF-1: Category changed after clinical activity was logged under the previous workflow → earlier activity is retained and linked to the visit; the case is flagged for records review rather than data being deleted.
- EF-2: The acting user lacks override privileges → the override is refused and the attempt is logged.

**Related Requirement:** FR-01, FR-02, FR-03, FR-04

**Business Requirement:** BR-03, BR-07

**Acceptance Criteria:**

1. Declared and confirmed categories are independently retrievable for any visit.
2. Every override stores the acting staff ID and timestamp, and is distinguishable from an unchanged confirmation.
3. Changing the confirmed category re-routes the case to the corresponding workflow without loss of recorded clinical data.

### UC-03: Open Temporary Patient Record

**Use Case ID:** UC-03 **Use Case Name:** Open Temporary Patient Record **Primary Actor:** Medical Staff **Secondary/External Actor:** —

**Description:** Creates a patient record under a system-generated temporary token when identity cannot be established, so treatment can begin immediately.

**Trigger:** Extension of UC-01 (Register Patient Visit) when the patient's identity is unknown. Extension condition: identity unknown or unverifiable.

**Preconditions:**

1. A visit registration is in progress.
2. No verified patient ID has been supplied or matched.

**Postconditions:**

1. A patient record exists under a unique temporary token, marked identity unconfirmed and queued for reconciliation.
2. All subsequent clinical activity for the visit attaches to the token.

**Alternate Flow:**

- AF-1: An attendant supplies partial identifying details (name, hall, department) → stored against the token as reconciliation hints.
- AF-2: Identity is established mid-treatment → staff proceed directly to UC-04 (Reconcile Patient Identity) without closing the visit.

**Exception Flow:**

- EF-1: Token generation fails → the system issues a fallback token from a reserved offline range and flags it for synchronisation; registration is never blocked.
- EF-2: Staff attempt to close the visit while the token is unreconciled → closure is permitted, but the record stays on the outstanding-reconciliation list.

**Related Requirement:** FR-10

**Business Requirement:** BR-01, BR-07

**Acceptance Criteria:**

1. A treatment record can be created and clinical data logged with no patient ID present.
2. Every temporary token is unique and clearly distinguishable from a verified patient ID.
3. Unreconciled tokens appear on an outstanding-reconciliation list until resolved.

### UC-04: Reconcile Patient Identity

**Use Case ID:** UC-04 **Use Case Name:** Reconcile Patient Identity **Primary Actor:** Medical Staff **Secondary/External Actor:** University Directory «external»

**Description:** Attaches a confirmed identity to a record opened under a temporary token, reclassifies the patient's eligibility, and merges the record into the patient's permanent history.

**Trigger:** The identity of a patient treated under a temporary token becomes known.

**Preconditions:**

1. A patient record exists under a temporary token with status identity unconfirmed.
2. The acting user holds Medical Staff privileges.

**Postconditions:**

1. The temporary token is resolved to a verified patient identity.
2. The patient is reclassified as member, family, guest, or outsider.
3. Clinical data recorded under the token is merged into the permanent record; the original token remains traceable for audit.

**Alternate Flow:**

- AF-1: The identified person already has a record → the token's clinical data is merged into it.
- AF-2: The identified person has no prior record → a new permanent patient record is created from the token.
- AF-3: Identity resolves to a guest → record reclassified; guest service restrictions apply from that point forward.
- AF-4: Identity resolves to a complete outsider → record reclassified; care already given is retained and the case is flagged for administrative review.

**Exception Flow:**

- EF-1: Directory verification fails or returns no match → reconciliation is refused, the token stays open, and the attempt is logged.
- EF-2: The supplied identity is already linked to a different open token → the merge is blocked and a duplicate-identity conflict is raised for manual resolution.
- EF-3: Merge would overwrite existing clinical data → both versions are preserved and the conflict is flagged rather than either being discarded.

**Related Requirement:** FR-11 (resolves FR-10)

**Business Requirement:** BR-03, BR-07

**Acceptance Criteria:**

1. A reconciled record retains every clinical entry made under the temporary token.
2. The original token remains retrievable and linked to the permanent record after reconciliation.
3. Reclassification updates the patient's eligibility tier and applicable service restrictions.
4. Reconciliation stores the acting staff ID and timestamp.

### UC-05: Verify Eligibility

**Use Case ID:** UC-05 **Use Case Name:** Verify Eligibility **Primary Actor:** Medical Staff **Secondary/External Actor:** University Directory «external»

**Description:** Classifies the patient as IUT member, family, guest, or complete outsider, and applies the service entitlements attached to that tier.

**Trigger:** A patient's entitlement to service must be established — normally during registration, or before a chargeable service is given.

**Preconditions:**

1. A patient or visit record exists (verified identity or temporary token).
2. The acting user holds Medical Staff privileges.

**Postconditions:**

1. The patient carries an eligibility class of member, family, guest, or outsider.
2. Service entitlements for that class are enforced by the system.
3. The classification, its evidence, and the deciding staff member are recorded.

**Alternate Flow:**

- AF-1: Valid Student, Staff, or Family ID presented → classified as member or family with full service entitlement.
- AF-2: No IUT ID but an eligible IUT person vouches → «include» UC-06 (Record Guest Sponsor); classified as guest, restricted to basic dressing, saline for fainting or low sugar, and verbal advice; referral creation blocked.
- AF-3: Neither ID nor sponsor → classified as complete outsider, marked ineligible for treatment.
- AF-4: Identity unknown → classification deferred; treatment proceeds under BR-01 and classification completes at UC-04 (Reconcile Patient Identity).

**Exception Flow:**

- EF-1: University Directory unreachable → patient provisionally classified on the presented ID and flagged for later verification; treatment is not withheld.
- EF-2: Presented ID is expired, suspended, or fails verification → not classified as member or family; staff may still route to the guest path if a sponsor is available.
- EF-3: Staff attempt to create a referral for a guest → the system blocks the action and displays the guest restriction.

**Related Requirement:** FR-05, FR-06, FR-08, FR-09

**Business Requirement:** BR-01, BR-02, BR-09

**Acceptance Criteria:**

1. Every patient record carries exactly one eligibility class at any time.
2. Referral creation is refused by the system for any patient classified as guest.
3. A patient classified as complete outsider is marked ineligible and cannot be issued treatment services.
4. Provisional classifications made during a directory outage are listed for follow-up verification.

### UC-06: Record Guest Sponsor

**Use Case ID:** UC-06 **Use Case Name:** Record Guest Sponsor **Primary Actor:** Medical Staff **Secondary/External Actor:** University Directory «external»

**Description:** Records the identified, eligible IUT person who vouches for a guest, as the precondition for any guest service being given.

**Trigger:** Included from UC-05 (Verify Eligibility) when a patient has no IUT ID but an IUT person is vouching for them.

**Preconditions:**

1. The patient is being assessed on the guest path.
2. An IUT person is present or contactable to vouch.

**Postconditions:**

1. The sponsor's verified IUT ID, name, and relationship to the guest are stored against the guest's visit.
2. Sponsorship timestamp and recording staff member are stored.
3. Guest service entitlement is activated for that visit only.

**Alternate Flow:**

- AF-1: Sponsor is a staff member rather than a student → recorded identically; entitlement unchanged.
- AF-2: The same sponsor vouches for several guests in one visit → each guest carries an individual sponsorship entry referencing the same sponsor.

**Exception Flow:**

- EF-1: The proposed sponsor is not an eligible IUT person → sponsorship refused; the patient falls to the complete-outsider path.
- EF-2: No sponsor can be produced → guest service is not activated; only life-priority care under BR-01 may proceed, and the case is flagged.
- EF-3: Sponsorship recorded after service was given → accepted, but the visit is flagged retrospectively sponsored for review.

**Related Requirement:** FR-07

**Business Requirement:** BR-02, BR-03, BR-09

**Acceptance Criteria:**

1. No guest service can be recorded against a visit that has no stored sponsor.
2. The stored sponsor is a verified IUT identity, not free text.
3. Sponsorship entries are retrievable by sponsor ID, so repeated sponsorship can be audited.

## Section 02 — Emergency

### UC-07: Create Emergency Record

**Use Case ID:** UC-07 **Use Case Name:** Create Emergency Record **Primary Actor:** Medical Staff **Secondary/External Actor:** Patient / Attendant

**Description:** Creates an Emergency Record from the minimum identifying information available, capturing the emergency description, attending staff, and timestamp, so that stabilisation can begin without delay.

**Trigger:** A case is routed to the Emergency workflow by UC-02 (Confirm Visit Category), or a patient arrives in an evidently emergent state.

**Preconditions:**

1. A valid Student ID, Staff ID, or temporary token is available.
2. The acting user holds Medical Staff privileges.

**Postconditions:**

1. An Emergency Record exists containing patient ID or temporary token, emergency description, attending staff, and record timestamp.
2. The record is linked to the originating visit and to the patient's connected history.
3. The record status is open-in treatment.

**Alternate Flow:**

- AF-1: Only a temporary token is available → the record is created against the token and queued for reconciliation.
- AF-2: Emergency arises during an ongoing Diagnosis visit → an Emergency Record is created and linked to the existing diagnosis case rather than as a fresh visit.
- AF-3: Multiple casualties arrive together → each patient receives a separate Emergency Record; a shared incident reference links them.

**Exception Flow:**

- EF-1: No ID and no token can be produced → the system auto-issues a token via UC-03 (Open Temporary Patient Record); record creation never blocks treatment.
- EF-2: Mandatory description field is empty at save → the system permits a provisional save marked incomplete and prompts for completion before outcome recording.
- EF-3: System unavailable at the point of care → staff record on paper and back-enter, with the entry flagged retrospective and carrying the actual event time alongside the entry time.

**Related Requirement:** FR-12, FR-13

**Business Requirement:** BR-01, BR-03, BR-07

**Acceptance Criteria:**

1. An Emergency Record can be created from a Student ID, a Staff ID, or a temporary token alone — no other field is mandatory to open it.
2. Every saved Emergency Record contains patient ID or token, description, vitals section, attending staff, and timestamp.
3. The Emergency Record is retrievable from the patient's connected history.
4. An outcome cannot be recorded against a record still marked incomplete.

### UC-08: Log Vitals & Procedures

**Use Case ID:** UC-08 **Use Case Name:** Log Vitals & Procedures **Primary Actor:** Medical Staff **Secondary/External Actor:** —

**Description:** Records the clinical observations taken and the procedures performed during emergency stabilisation.

**Trigger:** Included from UC-07 (Create Emergency Record) whenever observations are taken or a procedure is performed.

**Preconditions:**

1. An open Emergency Record exists for the patient.
2. The acting user holds Medical Staff privileges.

**Postconditions:**

1. Pulse, blood pressure, temperature, blood sugar, and oxygen readings are stored with the time taken and the recording staff member.
2. Procedures performed — dressing, stitching, IV injection, IM injection — are stored with the same attribution.
3. The Emergency Record reflects the full stabilisation history in chronological order.

**Alternate Flow:**

- AF-1: Only a subset of vitals is clinically relevant → the remainder are left blank and not treated as errors.
- AF-2: Vitals taken repeatedly over the episode → each set is stored as a separate timestamped entry; earlier readings are never overwritten.
- AF-3: A procedure is performed by a nurse before the doctor arrives → recorded against the nurse, with the doctor's later entries appended.

**Exception Flow:**

- EF-1: A reading falls outside physiologically plausible bounds → the system warns and requires confirmation before saving, but does not refuse a confirmed entry.
- EF-2: Staff attempt to edit a saved vitals entry → the original is retained and the change is stored as a correction with author and reason.
- EF-3: Equipment failure prevents a measurement → the field is marked not obtainable with a reason, rather than left silently blank.

**Related Requirement:** FR-14

**Business Requirement:** BR-03, BR-07

**Acceptance Criteria:**

1. The system supports all five listed vitals and all four listed procedures as structured fields.
2. Repeated vitals entries are stored as a time-ordered series, with no entry overwritten.
3. Every vitals and procedure entry carries the recording staff ID and time taken.
4. Corrections preserve the original value and record the reason for change.

### UC-09: Record Emergency Outcome

**Use Case ID:** UC-09 **Use Case Name:** Record Emergency Outcome **Primary Actor:** Doctor **Secondary/External Actor:** —

**Description:** Records the doctor's decision on how an emergency episode concludes: recovered and discharged, converted to a diagnosis consultation, or transferred to a hospital.

**Trigger:** The doctor reaches a clinical decision on the disposition of an emergency patient.

**Preconditions:**

1. An open Emergency Record exists with stabilisation activity logged.
2. The acting user holds Doctor privileges.
3. The Emergency Record is not marked incomplete.

**Postconditions:**

1. The Emergency Record carries exactly one outcome — discharged, converted to diagnosis, or transferred — with the deciding doctor and timestamp.
2. The record status changes from open-in treatment to closed or escalated.
3. Any downstream record created by the decision is linked to this Emergency Record.

**Alternate Flow:**

- AF-1: Outcome recovered and discharged → the record is closed; discharge advice is stored.
- AF-2: Outcome converted to diagnosis → «extend» UC-10 (Create Linked Diagnosis Record).
- AF-3: Outcome transferred to hospital → «extend» to UC-11 (Create Hospital Transfer).
- AF-4: Patient leaves against medical advice before a decision → outcome recorded as left against advice, with the informing staff member noted.

**Exception Flow:**

- EF-1: A non-doctor attempts to record an outcome → the action is refused and logged; only stabilisation entries remain available to that user.
- EF-2: Doctor is unreachable and the patient's condition demands escalation → the case follows the nurse-fallback path in UC-18 (Authorise Ambulance Dispatch as Nurse Fallback); the outcome is recorded retrospectively by the doctor and flagged for review.
- EF-3: An outcome is already recorded → a change requires a documented correction with reason, preserving the original decision.

**Related Requirement:** FR-15

**Business Requirement:** BR-03, BR-04, BR-07

**Acceptance Criteria:**

1. The system accepts exactly one of the three defined outcomes per Emergency Record.
2. Outcome recording is available only to users holding Doctor privileges.
3. Selecting converted to diagnosis produces a linked Diagnosis Record; selecting transferred produces a linked hospital transfer.
4. Every outcome stores the deciding doctor ID and timestamp.

### UC-10: Create Linked Diagnosis Record

**Use Case ID:** UC-10 **Use Case Name:** Create Linked Diagnosis Record **Primary Actor:** Doctor **Secondary/External Actor:** —

**Description:** Creates a Diagnosis Record bound to the originating Emergency Record when an emergency case is escalated for further consultation, preserving one connected patient history.

**Trigger:** Extension of UC-09 (Record Emergency Outcome). Extension condition: outcome = converted to diagnosis.

**Preconditions:**

1. An Emergency Record exists with outcome converted to diagnosis.
2. The acting user holds Doctor privileges.

**Postconditions:**

1. A Diagnosis Record exists, carrying a permanent link to the originating Emergency Record.
2. Relevant clinical context — description, latest vitals, procedures performed — is carried forward into the Diagnosis Record.
3. The case now appears in the Diagnosis workflow.

**Alternate Flow:**

- AF-1: The patient already has an open Diagnosis Record → the emergency episode is linked to the existing record instead of a new one being created.
- AF-2: Consultation is scheduled for a later date → the Diagnosis Record is created in status awaiting consultation with the appointment recorded.

**Exception Flow:**

- EF-1: The patient is a guest → conversion to a full diagnosis case is blocked under the guest service restriction; the case is closed with advice only.
- EF-2: The link to the Emergency Record fails to persist → creation is rolled back rather than producing an orphan Diagnosis Record.

**Related Requirement:** FR-16

**Business Requirement:** BR-07, BR-09

**Acceptance Criteria:**

1. Every Diagnosis Record created this way is traceable to its originating Emergency Record, in both directions.
2. Carried-forward clinical data is visible in the Diagnosis Record without re-entry.
3. No orphan Diagnosis Record can be created by this flow.

### UC-11: Create Hospital Transfer

**Use Case ID:** UC-11 **Use Case Name:** Create Hospital Transfer **Primary Actor:** Doctor **Secondary/External Actor:** Medical Staff (authorised), Nurse

**Description:** Creates a hospital transfer for an emergency patient requiring treatment beyond the medical centre's capability, and generates the accompanying Emergency Transfer Slip as part of the transfer.

**Trigger:** A doctor determines that an emergency patient must be moved to a hospital.

**Preconditions:**

1. An open Emergency Record exists for the patient.
2. The acting user holds transfer-creation authority (Doctor, or authorised Medical Staff under the fallback rule).
3. A destination hospital is selectable.

**Postconditions:**

1. A hospital transfer record exists with patient ID, emergency description, destination hospital, attending staff, and transfer timestamp.
2. An Emergency Transfer Slip is generated and attached to the transfer.
3. The Emergency Record outcome is set to transferred.
4. If the destination is an affiliated hospital, the transfer is marked as carrying a valid IUT referral for coverage purposes.

**Alternate Flow:**

- AF-1: Destination is an affiliated hospital → the transfer is flagged coverage-eligible under BR-06.
- AF-2: Destination is non-affiliated but nearest/clinically necessary → the transfer proceeds and is flagged for the non-affiliated cost cap.
- AF-3: Transfer requires an ambulance → UC-15 (Dispatch Ambulance) is initiated from the transfer.
- AF-4: The patient is transported by their own arrangement → UC-19 (Log Self-Transport Fallback) is recorded against the transfer.

**Exception Flow:**

- EF-1: No doctor is reachable and the condition is time-critical → an authorised nurse creates the transfer; the record is flagged as created under fallback authority for review.
- EF-2: The patient is a guest → transfer creation is blocked by the guest restriction; only life-priority stabilisation and an unassisted hospital advice note are permitted, and the block is logged.
- EF-3: Slip generation fails → the transfer record is still saved, and the slip is queued for regeneration; the transfer is never held up by document generation.

**Related Requirement:** FR-17, FR-18

**Business Requirement:** BR-01, BR-03, BR-04, BR-06

**Acceptance Criteria:**

1. A saved transfer record contains all five required slip fields: patient ID, emergency description, destination hospital, attending staff, transfer timestamp.
2. Creating a transfer automatically sets the linked Emergency Record's outcome to transferred.
3. A transfer created without doctor authorisation is visibly flagged for review.
4. Transfer creation is refused for patients classified as guests.

### UC-12: Retrieve Emergency Transfer Slip

**Use Case ID:** UC-12 **Use Case Name:** Retrieve Emergency Transfer Slip **Primary Actor:** Medical Staff **Secondary/External Actor:** Patient / Attendant

**Description:** Retrieves a previously generated Emergency Transfer Slip for viewing, download, or printing, so it can accompany the patient or be produced later for records and reimbursement.

**Trigger:** A staff member, patient, or attendant needs a copy of the transfer slip.

**Preconditions:**

1. A hospital transfer record exists with a generated slip.
2. The requester is authorised for that patient's records.

**Postconditions:**

1. The slip is displayed, downloaded, or printed in its stored form.
2. The retrieval is logged with requester identity and timestamp.
3. The slip content is unchanged by retrieval.

**Alternate Flow:**

- AF-1: Requested at the moment of transfer → the slip is printed and handed to the accompanying staff or attendant.
- AF-2: Requested later by the patient for reimbursement → issued as a download, marked as a reissue.
- AF-3: Requested by the receiving hospital via the medical centre → staff retrieve and forward it on the patient's behalf.

**Exception Flow:**

- EF-1: The slip has not yet been generated (queued after a generation failure) → the system regenerates it on demand from the stored transfer record.
- EF-2: The requester is not authorised for the record → retrieval is refused, and the attempt is logged.
- EF-3: The underlying transfer record has been corrected since issue → the reissued slip shows the current data and is marked as a revised copy, with the original version retained.

**Related Requirement:** FR-18

**Business Requirement:** BR-03, BR-08

**Acceptance Criteria:**

1. The slip can be viewed, downloaded, and printed from the same record.
2. Every retrieval is logged with the requester and timestamp.
3. Retrieval never alters the stored transfer record.
4. A slip can be reissued after the original is lost.

### UC-13: Log After-Hours Hotline Request

**Use Case ID:** UC-13 **Use Case Name:** Log After-Hours Hotline Request **Primary Actor:** Nurse **Secondary/External Actor:** Patient / Attendant (caller)

**Description:** Logs a request received on the night hotline from a hall resident, capturing caller, hall location, and time, so that after-hours response can be initiated and audited.

**Trigger:** A hall resident calls the medical centre hotline outside normal operating hours.

**Preconditions:**

1. The hotline is in after-hours operation.
2. The receiving user holds Nurse or Medical Staff privileges.

**Postconditions:**

1. A hotline request record exists with caller identity, hall location, reported complaint, call time, and receiving staff member.
2. The request carries a response status.

**Alternate Flow:**

- AF-1: Caller is reporting on behalf of another person → the patient and the caller are recorded as separate identities on the request.
- AF-2: Advice over the phone resolves the situation → the request is closed with outcome advised-no dispatch.
- AF-3: On-site response is required → UC-14 (Record Nurse Dispatch & On-Site Care) is initiated from the request.
- AF-4: Situation is immediately critical → the request is escalated directly to UC-15 (Dispatch Ambulance).

**Exception Flow:**

- EF-1: Caller will not identify themselves → the request is logged as anonymous caller with the hall location, and response is not withheld.
- EF-2: Call drops before details are complete → a partial request is saved and flagged for callback.
- EF-3: Hall location is outside campus → the request is redirected to the off-campus path in UC-20 (Log Off-Campus Trip Incident) if it relates to an authorised trip, otherwise logged as out of scope.

**Related Requirement:** FR-19

**Business Requirement:** BR-03, BR-05

**Acceptance Criteria:**

1. Every hotline request stores caller, hall location, and call time as mandatory fields, with an explicit anonymous option for caller.
2. Requests are retrievable by date, hall, and receiving staff member.
3. A request cannot be closed without a recorded outcome.

### UC-14: Record Nurse Dispatch & On-Site Care

**Use Case ID:** UC-14 **Use Case Name:** Record Nurse Dispatch & On-Site Care **Primary Actor:** Nurse **Secondary/External Actor:** Doctor

**Description:** Records that a nurse has been dispatched to a hall, that on-site care has begun before the doctor's arrival, and that the doctor has been informed.

**Trigger:** A hotline request or in-person report requires on-site attendance at a hall.

**Preconditions:**

1. A hotline request or emergency case exists requiring on-site response.
2. A nurse is available for dispatch.

**Postconditions:**

1. Dispatch time, dispatched nurse, and destination hall are recorded against the request.
2. Start time of on-site care is recorded.
3. Notification of the doctor is recorded with the time and the method used.

**Alternate Flow:**

- AF-1: Doctor is reachable and attends → the doctor's arrival time is recorded and care handed over.
- AF-2: Doctor is reachable but advises remotely → the advice given and the advising doctor are recorded; the nurse continues on-site care.
- AF-3: Patient is brought to the medical centre instead → the on-site record is closed and an Emergency Record is opened via UC-07 (Create Emergency Record).
- AF-4: Condition escalates on site → the nurse initiates UC-15 (Dispatch Ambulance).

**Exception Flow:**

- EF-1: Doctor cannot be reached → the attempts made are recorded with times, and the case proceeds under fallback authority, flagged for review.
- EF-2: No nurse is available → the request is escalated and the unavailability is logged against the request.
- EF-3: Nurse arrives and the patient is absent or refuses care → recorded as attended-care declined with the time.

**Related Requirement:** FR-20

**Business Requirement:** BR-03, BR-04, BR-05

**Acceptance Criteria:**

1. Dispatch time, arrival time, and care start time are separately recorded.
2. The record shows whether and when the doctor was informed, and by what means.
3. Every failed attempt to reach the doctor is individually timestamped.

### UC-15: Dispatch Ambulance

**Use Case ID:** UC-15 **Use Case Name:** Dispatch Ambulance **Primary Actor:** Doctor **Secondary/External Actor:** Nurse, Driver (on duty)

**Description:** Deploys the standby ambulance and on-duty driver to move a patient to hospital, on authorised request and subject to availability.

**Trigger:** A doctor determines that a patient requires ambulance transport to a hospital.

**Preconditions:**

1. A patient case exists requiring hospital transport.
2. The requesting user holds Doctor privileges, or qualifies for nurse fallback authority.

**Postconditions:**

1. A dispatch record exists with authoriser, driver, destination, and dispatch timestamp.
2. The ambulance and driver are marked unavailable for the duration of the trip.
3. The dispatch is linked to the patient's Emergency Record and hospital transfer.

**Alternate Flow:**

- AF-1: Doctor authorises normally → «include» UC-17 (Record Dispatch Authorisation) with the doctor as authoriser.
- AF-2: Doctor unreachable → «extend» UC-18 (Authorise Ambulance Dispatch as Nurse Fallback).
- AF-3: No ambulance or driver available → «extend» UC-19 (Log Self-Transport Fallback).
- AF-4: Ambulance returns and is released → availability is restored via UC-16 (Check Ambulance Availability).

**Exception Flow:**

- EF-1: Ambulance is dispatched but the patient's condition resolves before departure → the dispatch is cancelled with a reason and the resource released.
- EF-2: Vehicle breaks down en route → the dispatch is marked failed-in transit, availability is updated, and the case falls to self-transport or a second dispatch.
- EF-3: Dispatch is requested for a guest or outsider → the request is blocked pending administrative authorisation, except where life priority under BR-01 applies, in which case it proceeds and is flagged.

**Related Requirement:** FR-21, FR-23, FR-24

**Business Requirement:** BR-01, BR-03, BR-04, BR-10

**Acceptance Criteria:**

1. No dispatch can be recorded without a stored authoriser.
2. Dispatch sets ambulance and driver availability to unavailable, and release restores it.
3. Every dispatch record carries authoriser, driver, destination, and timestamp.
4. Dispatches authorised under fallback are distinguishable from doctor-authorised ones in any listing.

### UC-16: Check Ambulance Availability

**Use Case ID:** UC-16 **Use Case Name:** Check Ambulance Availability **Primary Actor:** Medical Staff **Secondary/External Actor:** —

**Description:** Reports the current availability of the standby ambulance and the on-duty driver, so that dispatch decisions and fallbacks are based on live resource state.

**Trigger:** Included from UC-15 (Dispatch Ambulance) before any dispatch, and available on demand to staff.

**Preconditions:**

1. Ambulance and driver duty rosters are configured in the system.

**Postconditions:**

1. The current status of the ambulance (available · deployed · out of service) and the driver (on duty · off duty · deployed) is returned.
2. The check is recorded against the dispatch decision it informed.

**Alternate Flow:**

- AF-1: Both ambulance and driver available → dispatch proceeds.
- AF-2: Ambulance available but no driver on duty → treated as unavailable for dispatch; the reason is recorded distinctly.
- AF-3: Ambulance already deployed → expected release time is shown if known.

**Exception Flow:**

- EF-1: Roster data is missing or stale → the system reports availability unknown rather than assuming availability, and prompts for manual confirmation.
- EF-2: Actual availability contradicts recorded status → staff override the status manually; the override is logged with the acting staff member.

**Related Requirement:** FR-23

**Business Requirement:** BR-10

**Acceptance Criteria:**

1. Availability distinguishes vehicle status from driver status.
2. The system never reports available when roster data is absent.
3. Every dispatch record references the availability state at the time of the decision.

### UC-17: Record Dispatch Authorisation

**Use Case ID:** UC-17 **Use Case Name:** Record Dispatch Authorisation **Primary Actor:** Doctor **Secondary/External Actor:** —

**Description:** Records who authorised an ambulance dispatch, together with the driver, destination, and timestamp, establishing accountability for the escalation decision.

**Trigger:** Included from UC-15 (Dispatch Ambulance) whenever a dispatch is committed.

**Preconditions:**

1. A dispatch is being committed.
2. An authorising identity is present.

**Postconditions:**

1. Authoriser identity, authority basis (doctor / nurse fallback), driver, destination, and timestamp are stored.
2. Fallback authorisations are flagged for later review.

**Alternate Flow:**

- AF-1: Doctor authorises in person → authority basis recorded as doctor.
- AF-2: Doctor authorises remotely by phone → recorded as doctor-remote, with the contact time.
- AF-3: Nurse authorises under fallback → authority basis recorded as nurse fallback and the review flag set.

**Exception Flow:**

- EF-1: Authorisation is recorded after the ambulance has departed → accepted as a retrospective entry, carrying both the actual and the entry time, and flagged.
- EF-2: The authorising user's privileges cannot be verified → the dispatch is still recorded (life priority) but flagged authority unverified for review.

**Related Requirement:** FR-21, FR-24

**Business Requirement:** BR-03, BR-04

**Acceptance Criteria:**

1. Every dispatch record has exactly one authoriser and one authority basis.
2. Authority basis values are distinguishable and filterable for audit.
3. Retrospective authorisations store both the event time and the entry time.

### UC-18: Authorise Ambulance Dispatch as Nurse Fallback

**Use Case ID:** UC-18 **Use Case Name:** Authorise Ambulance Dispatch as Nurse Fallback **Primary Actor:** Nurse **Secondary/External Actor:** Doctor (unreachable party)

**Description:** Permits a nurse to authorise ambulance dispatch when the doctor is unreachable, recording the attempts made and flagging the decision for clinical review.

**Trigger:** Extension of UC-15 (Dispatch Ambulance). Extension condition: the doctor cannot be reached and transport is clinically necessary.

**Preconditions:**

1. Documented attempts to reach the doctor have failed.
2. The acting user holds Nurse privileges.
3. A patient case requires immediate transport.

**Postconditions:**

1. The dispatch is authorised with authority basis nurse fallback.
2. Each attempt to contact the doctor is stored with its time.
3. The dispatch appears on the review queue for the responsible doctor.

**Alternate Flow:**

- AF-1: Doctor becomes reachable before departure → authority reverts to the doctor and the fallback flag is cleared, with the sequence retained in the audit trail.
- AF-2: A second doctor is reachable → that doctor authorises; the case is not a fallback.
- AF-3: Doctor confirms retrospectively → the confirmation is appended; the fallback flag remains for the audit record.

**Exception Flow:**

- EF-1: No contact attempts are recorded → the system refuses to accept fallback authority, since the precondition is unmet.
- EF-2: Fallback authorisation is used repeatedly by the same nurse within a defined window → the system raises this on the review queue as a pattern requiring attention.

**Related Requirement:** FR-22

**Business Requirement:** BR-03, BR-04, BR-05

**Acceptance Criteria:**

1. Fallback authority is unavailable unless at least one failed doctor contact attempt is recorded.
2. Every fallback dispatch is flagged and appears on a review queue.
3. Fallback dispatches are reportable as a distinct category.

### UC-19: Log Self-Transport Fallback

**Use Case ID:** UC-19 **Use Case Name:** Log Self-Transport Fallback **Primary Actor:** Medical Staff **Secondary/External Actor:** Patient / Attendant

**Description:** Records that a patient travelled to hospital by their own arrangement because no ambulance or driver was available, and marks the case for later reconciliation.

**Trigger:** Extension of UC-15 (Dispatch Ambulance). Extension condition: no ambulance or no on-duty driver is available.

**Preconditions:**

1. Transport to hospital is required.
2. UC-16 (Check Ambulance Availability) has returned unavailable.

**Postconditions:**

1. A self-transport entry exists with reason for unavailability, departure time, destination, and the person arranging transport.
2. The case is marked pending reconciliation for cost and outcome follow-up.
3. The unavailability event is recorded against ambulance resource history.

**Alternate Flow:**

- AF-1: Family or friends transport the patient → the transporting party is recorded.
- AF-2: Patient uses a hired vehicle and seeks cost recovery → the case is linked to the cost rules in UC-39 (Apply Hospital Cost Coverage Rules).
- AF-3: Ambulance becomes available before departure → self-transport is cancelled and normal dispatch resumes.

**Exception Flow:**

- EF-1: The patient departs without informing staff → the entry is created retrospectively on discovery and flagged unconfirmed departure.
- EF-2: Destination hospital is unknown at the time of logging → the field is left pending and completed at reconciliation.

**Related Requirement:** FR-25

**Business Requirement:** BR-01, BR-03, BR-10

**Acceptance Criteria:**

1. A self-transport entry cannot be saved without a recorded reason for ambulance unavailability.
2. Self-transport cases appear on a reconciliation list until closed.
3. Ambulance unavailability events are reportable over a date range.

### UC-20: Log Off-Campus Trip Incident

**Use Case ID:** UC-20 **Use Case Name:** Log Off-Campus Trip Incident **Primary Actor:** Medical Staff **Secondary/External Actor:** Doctor (hotline approver)

**Description:** Records a medical incident occurring during an IUT-authorised off-campus trip, the first-aid given, the hotline approval to proceed to a hospital where required, and applies the applicable cost coverage rules.

**Trigger:** A medical incident is reported during an IUT-authorised off-campus activity.

**Preconditions:**

1. The activity is registered as an IUT-authorised trip.
2. The patient is an eligible IUT member or family member.

**Postconditions:**

1. An incident record exists with trip reference, patient, incident description, first-aid given, and time.
2. Where hospital treatment was required, the hotline approval, approving staff member, and destination hospital are recorded.
3. Cost coverage is determined via «include» UC-39 (Apply Hospital Cost Coverage Rules).

**Alternate Flow:**

- AF-1: First-aid resolves the incident → the record is closed with outcome resolved on site; no cost rules apply.
- AF-2: Hospital treatment required → hotline approval is obtained and recorded before the patient proceeds.
- AF-3: IUT dispatches medical staff to the location → the dispatched staff and their departure time are recorded against the incident.
- AF-4: Treatment is at an affiliated hospital → full coverage applies under BR-06.

**Exception Flow:**

- EF-1: Hotline cannot be reached and treatment is urgent → the patient proceeds; approval is recorded retrospectively and the case is flagged for review, with coverage not withheld on that basis alone.
- EF-2: The trip is not on the authorised list → the incident is logged, but coverage is refused pending administrative confirmation of authorisation.
- EF-3: No affiliated hospital is reachable from the trip location → the non-affiliated cap applies and the geographic constraint is recorded as justification for review.

**Related Requirement:** FR-26, FR-27

**Business Requirement:** BR-03, BR-06, BR-11

**Acceptance Criteria:**

1. Every incident record references a specific authorised trip.
2. Hospital escalation cannot be closed without a recorded hotline approval, retrospective entries included and flagged.
3. The same cost coverage logic used for referrals is applied here, not a separate implementation.
4. First-aid-only incidents close without invoking cost rules.

## Section 03 — Diagnosis

### UC-21: Create Diagnosis Record

**Use Case ID:** UC-21 **Use Case Name:** Create Diagnosis Record **Primary Actor:** Doctor **Secondary/External Actor:** Patient

**Description:** Records the doctor's examination findings for a diagnosis case, capturing the full clinical picture including signs and symptoms, chronicity, frequency and duration, side-effects and food history, referral status, and treatment.

**Trigger:** A doctor completes an examination of a patient routed to the Diagnosis workflow.

**Preconditions:**

1. The patient's visit is confirmed as category Diagnosis, or an emergency case has been converted.
2. The acting user holds Doctor privileges.
3. The patient's eligibility permits full service.

**Postconditions:**

1. A Diagnosis Record exists containing patient ID, signs and symptoms, chronic-or-temporary classification, frequency and duration, side-effects and food history, referral status, treatment, attending doctor, and timestamp.
2. The record is linked into the patient's connected history.
3. The record status is active.

**Alternate Flow:**

- AF-1: Case originates from an emergency → the record is created by UC-10 (Create Linked Diagnosis Record) and pre-populated with emergency context.
- AF-2: Condition is classified chronic → the record is marked for ongoing management rather than a single treatment period.
- AF-3: A referral is decided at the first visit → referral status is set and UC-27 (Create Referral) is initiated from this record.
- AF-4: The patient has previous diagnosis records → prior history is displayed to the doctor and may be referenced in the new record.

**Exception Flow:**

- EF-1: The patient is classified as guest → creation of a full Diagnosis Record is blocked; only verbal advice may be recorded under the guest restriction.
- EF-2: A mandatory clinical field is incomplete → the record saves in status draft and cannot be used to prescribe treatment until completed.
- EF-3: A non-doctor attempts creation → the action is refused and logged.

**Related Requirement:** FR-28

**Business Requirement:** BR-03, BR-07, BR-09

**Acceptance Criteria:**

1. All eight listed content fields are present as structured fields in every saved Diagnosis Record.
2. The record stores attending doctor ID and timestamp.
3. Creation is available only to users holding Doctor privileges.
4. A draft record cannot have a treatment plan attached to it.

### UC-22: Update Diagnosis Record

**Use Case ID:** UC-22 **Use Case Name:** Update Diagnosis Record **Primary Actor:** Medical Staff (authorised) **Secondary/External Actor:** Doctor

**Description:** Amends an existing Diagnosis Record as the clinical picture develops, preserving the previous content and recording who changed what and when.

**Trigger:** New clinical information, a correction, or a follow-up finding requires an existing Diagnosis Record to be amended.

**Preconditions:**

1. A Diagnosis Record exists.
2. The acting user holds authorised update privileges for diagnosis records.

**Postconditions:**

1. The record reflects the updated content.
2. The previous version is retained with the amending user's ID, timestamp, and the fields changed.
3. The patient's connected history reflects the amendment.

**Alternate Flow:**

- AF-1: Doctor updates findings after a follow-up visit → the update is linked to the corresponding follow-up entry.
- AF-2: Nurse or medical staff add observational data within their authorised scope → the update is attributed to them and scoped to permitted fields.
- AF-3: Referral status changes after a referral is created → the field is updated automatically from UC-27 (Create Referral).
- AF-4: A clerical correction is made → recorded as a correction with a reason, distinct from a clinical amendment.

**Exception Flow:**

- EF-1: The record is closed as presumed recovered → updating reopens the case, and the reopening is recorded with a reason.
- EF-2: Two users amend the same record concurrently → the system prevents silent overwrite and requires the second user to review the intervening change.
- EF-3: A user attempts to edit fields outside their authorised scope → the edit is refused and logged.

**Related Requirement:** FR-29

**Business Requirement:** BR-03, BR-07

**Acceptance Criteria:**

1. No update overwrites history — the prior version remains retrievable.
2. Every update stores the amending user, timestamp, and the fields changed.
3. Field-level edit permissions differ between Doctor and other Medical Staff and are enforced.
4. Concurrent edits cannot silently discard another user's change.

### UC-23: Access Diagnosis Record

**Use Case ID:** UC-23 **Use Case Name:** Access Diagnosis Record **Primary Actor:** Medical Staff **Secondary/External Actor:** Doctor, Patient (subject to an authorising requirement — see note)

**Description:** Retrieves a patient's diagnosis records by patient ID for clinical review, continuity of care, and follow-up decisions.

**Trigger:** A clinician needs a patient's diagnosis history, typically at a follow-up or when treating a related condition.

**Preconditions:**

1. At least one Diagnosis Record exists for the patient.
2. The requester is authenticated and authorised for that patient's records.

**Postconditions:**

1. The requested records are displayed in reverse-chronological order.
2. The access event is logged with requester identity, patient ID, and timestamp.
3. No record content is altered.

**Alternate Flow:**

- AF-1: Access during a follow-up consultation → the active treatment period and any missed follow-up are highlighted.
- AF-2: Access for a linked emergency case → the originating Emergency Record is shown alongside.
- AF-3: Patient requests their own record → access is granted only within the scope defined by the authorising requirement.

**Exception Flow:**

- EF-1: No records exist for the patient ID → an empty result is returned; this is not an error.
- EF-2: Requester is not authorised for that patient → access is refused and the attempt is logged as a potential privacy event.
- EF-3: Records exist only under an unreconciled temporary token → the requester is prompted to search by token, and the reconciliation gap is surfaced.

**Related Requirement:** FR-28, FR-29 (patient access requires a new FR)

**Business Requirement:** BR-03, BR-07

**Acceptance Criteria:**

1. Records are retrievable by patient ID and by temporary token.
2. Every access is logged with requester, patient, and timestamp.
3. Unauthorised access attempts are refused and logged.
4. Access never modifies record content.

### UC-24: Prescribe Treatment Plan

**Use Case ID:** UC-24 **Use Case Name:** Prescribe Treatment Plan **Primary Actor:** Doctor **Secondary/External Actor:** Patient

**Description:** Records the prescribed treatment and the period over which it applies, and sets the follow-up expectation that governs whether the case is later closed or continued.

**Trigger:** A doctor decides on treatment following a diagnosis or a follow-up assessment.

**Preconditions:**

1. An active, non-draft Diagnosis Record exists.
2. The acting user holds Doctor privileges.

**Postconditions:**

1. The treatment, its start date, and the treatment period are stored against the Diagnosis Record.
2. A follow-up expectation date is derived from the treatment period.
3. The case status is under treatment, and the follow-up clock is running.

**Alternate Flow:**

- AF-1: Chronic condition → a recurring treatment period is set, and the case is not eligible for automatic closure by non-return.
- AF-2: Treatment is revised at a follow-up → a new treatment period supersedes the previous one; the earlier plan is retained in history.
- AF-3: Treatment is prescribed alongside a referral → both are recorded, and the follow-up expectation accounts for the referral timeline.
- AF-4: Advice only, no medication → recorded as a treatment plan of type advice, with a follow-up period still set.

**Exception Flow:**

- EF-1: A recorded side-effect or food-history entry contraindicates the treatment → the system warns the doctor and requires acknowledgement before saving.
- EF-2: Treatment period is left blank → the plan cannot be saved, since the follow-up and closure logic depend on it.
- EF-3: The patient is a guest → prescribing beyond the guest restriction is blocked.

**Related Requirement:** FR-30

**Business Requirement:** BR-03, BR-07, BR-09

**Acceptance Criteria:**

1. A treatment plan cannot be saved without a defined treatment period.
2. The follow-up expectation date is derived automatically and visible on the record.
3. Superseded treatment plans remain retrievable in history.
4. Chronic cases are excluded from automatic closure by non-return.

### UC-25: Record Follow-up Visit

**Use Case ID:** UC-25 **Use Case Name:** Record Follow-up Visit **Primary Actor:** Doctor **Secondary/External Actor:** Patient

**Description:** Records a patient's re-visit within the treatment period and the doctor's decision to continue treatment or escalate to a specialist referral.

**Trigger:** A patient returns to the medical centre within, or shortly after, the prescribed treatment period.

**Preconditions:**

1. An active Diagnosis Record exists with a treatment plan and an open follow-up expectation.
2. The acting user holds Doctor privileges.

**Postconditions:**

1. A follow-up entry exists with the visit date, the doctor's assessment, and the decision taken.
2. The follow-up clock is either reset by a new treatment period or closed by the decision.
3. The Diagnosis Record reflects the current case status.

**Alternate Flow:**

- AF-1: Condition improving, treatment continues → a new treatment period is set via UC-24 (Prescribe Treatment Plan) and the cycle repeats.
- AF-2: Condition requires specialist input → «extend» to UC-27 (Create Referral); referral status on the Diagnosis Record is updated.
- AF-3: Patient has recovered → the case is closed with outcome recovered-confirmed at follow-up, which is distinct from presumed recovery.
- AF-4: Patient returns after the period has lapsed and the case was auto-closed → the case is reopened and the follow-up recorded against it.

**Exception Flow:**

- EF-1: Patient returns with an unrelated condition → a new Diagnosis Record is created rather than a follow-up entry on the existing one.
- EF-2: Patient returns in an emergent state → the case is routed to UC-07 (Create Emergency Record) and the follow-up is linked to the emergency episode.
- EF-3: Referral creation is blocked by eligibility → the block reason is recorded on the follow-up entry and treatment continues within the centre's scope.

**Related Requirement:** FR-30, FR-32

**Business Requirement:** BR-03, BR-04, BR-07

**Acceptance Criteria:**

1. A follow-up entry records exactly one decision: continue treatment, refer to specialist, or close as recovered.
2. Recording a follow-up within the period prevents automatic closure by non-return.
3. A case auto-closed as Missed Follow-Up can be reopened by a late follow-up, with both events retained.
4. Choosing refer to specialist creates a referral linked to the Diagnosis Record.

### UC-26: Close Case as Missed Follow-Up (Presumed Recovered)

**Use Case ID:** UC-26 **Use Case Name:** Close Case as Missed Follow-Up (Presumed Recovered) **Primary Actor:** Time «secondary system actor» **Secondary/External Actor:** Doctor (reviewer)

**Description:** Automatically marks a diagnosis case as Missed Follow-Up (presumed recovered) when the treatment period lapses without a follow-up visit, so that open cases do not accumulate indefinitely.

**Trigger:** The prescribed treatment period elapses with no follow-up entry recorded. This use case has no human initiator.

**Preconditions:**

1. A Diagnosis Record has an active treatment plan with a defined period.
2. No follow-up visit has been recorded within that period.
3. The case is not marked chronic.

**Postconditions:**

1. The case status becomes Missed Follow-Up (presumed recovered), with the closure date and the rule that fired.
2. The closure is distinguishable from a clinician-confirmed recovery.
3. The case remains reopenable.

**Alternate Flow:**

- AF-1: A grace period is configured → closure occurs after the period plus the grace window, not immediately at expiry.
- AF-2: Patient returns after closure → UC-25 (Record Follow-up Visit) reopens the case and the closure is retained in history.
- AF-3: A doctor reviews auto-closed cases → they may convert the status to confirmed recovery or reopen for outreach.

**Exception Flow:**

- EF-1: The case is chronic → automatic closure does not apply; the case remains open for ongoing management.
- EF-2: A referral is outstanding on the case → closure is suppressed until the referral is resolved, since the patient is still in active care.
- EF-3: The scheduled job fails to run → affected cases remain open and are picked up on the next run; no case is closed twice.

**Related Requirement:** FR-31

**Business Requirement:** BR-03, BR-07

**Acceptance Criteria:**

1. Closure occurs without any user action, driven solely by the lapse of the treatment period.
2. Missed Follow-Up is stored as a distinct status from recovered-confirmed at follow-up.
3. Chronic cases and cases with outstanding referrals are never auto-closed.
4. Every auto-closure records the closure date and the rule that produced it.

## Section 04 — Referral

### UC-27: Create Referral

**Use Case ID:** UC-27 **Use Case Name:** Create Referral **Primary Actor:** Doctor **Secondary/External Actor:** Patient

**Description:** Creates a referral sending a patient to a specialist or an external facility, recording the category and reason, and generating the accompanying Referral Slip as part of the same action.

**Trigger:** A doctor determines that a patient requires care beyond the medical centre's scope.

**Preconditions:**

1. An active Diagnosis Record or Emergency Record exists for the patient.
2. The acting user holds Doctor privileges.
3. The patient's eligibility class permits referral — guests are excluded.

**Postconditions:**

1. A referral record exists with referral ID, patient ID, referral category, reason, destination or referred doctor where known, attending doctor, and timestamp.
2. A Referral Slip is generated and attached to the referral.
3. Referral status on the linked Diagnosis Record is updated.
4. Where the destination is an affiliated hospital, the referral constitutes the valid IUT referral required for coverage.

**Alternate Flow:**

- AF-1: Category is Cardiology, Dermatology, or General Medicine → selected from the defined list.
- AF-2: Category is Other → a free-text specialisation is required alongside.
- AF-3: No specific destination or doctor is nominated → the referral is issued as open, with destination recorded at the point of use.
- AF-4: Referral is decided at a follow-up → created from UC-25 (Record Follow-up Visit) and linked to that entry.
- AF-5: Referral is for hospital admission → the referral is linked to cost handling via UC-35 (Settle Hospital Referral Cost).

**Exception Flow:**

- EF-1: The patient is classified as guest → referral creation is blocked by the guest restriction and the block is logged.
- EF-2: A non-doctor attempts to create a referral → refused and logged; where a doctor is absent, only the limited call-support order path applies (see traceability note).
- EF-3: Slip generation fails → the referral record is still saved and the slip is queued for regeneration.
- EF-4: An active referral already exists for the same condition → the system warns of duplication and requires confirmation.

**Related Requirement:** FR-33, FR-34, FR-35, FR-38, FR-41

**Business Requirement:** BR-02, BR-03, BR-04, BR-06, BR-09

**Acceptance Criteria:**

1. All six required content fields are present in every saved referral record.
2. Referral creation is available only to users holding Doctor privileges.
3. Referral creation is refused for patients classified as guest or outsider.
4. The four defined categories are enforced, with Other requiring a free-text specialisation.
5. Saving a referral generates a Referral Slip without a separate user action.

### UC-28: Retrieve Referral Slip

**Use Case ID:** UC-28 **Use Case Name:** Retrieve Referral Slip **Primary Actor:** Patient **Secondary/External Actor:** Doctor, Medical Staff

**Description:** Retrieves a generated Referral Slip for viewing, download, or printing, so the patient can present it at the destination facility or produce it later for reimbursement.

**Trigger:** A patient or staff member needs a copy of an issued referral slip.

**Preconditions:**

1. A referral record exists with a generated slip.
2. The requester is the patient concerned or an authorised staff member.

**Postconditions:**

1. The slip is displayed, downloaded, or printed in its stored form.
2. The retrieval is logged with requester identity and timestamp.
3. The referral record is unchanged.

**Alternate Flow:**

- AF-1: Printed at the point of issue and handed to the patient.
- AF-2: Downloaded later by the patient for a hospital visit or reimbursement claim, marked as a reissue.
- AF-3: Retrieved by staff on the patient's behalf when the patient cannot attend.

**Exception Flow:**

- EF-1: The slip was queued after a generation failure → it is regenerated on demand from the stored referral record.
- EF-2: The requester is not the patient and is not authorised → retrieval is refused and logged.
- EF-3: The referral has since been updated → the reissued slip shows current data, is marked a revised copy, and the original version is retained.
- EF-4: The referral has been cancelled → the slip is marked cancelled on retrieval and cannot be presented as valid.

**Related Requirement:** FR-38, FR-39

**Business Requirement:** BR-03, BR-08

**Acceptance Criteria:**

1. The slip can be viewed, downloaded, and printed from the same record.
2. A patient can retrieve their own referral slip without staff intervention.
3. Every retrieval is logged with a requester and timestamp.
4. A cancelled referral's slip is visibly marked as invalid.

### UC-29: Order In-Campus Test

**Use Case ID:** UC-29 **Use Case Name:** Order In-Campus Test **Primary Actor:** Doctor **Secondary/External Actor:** Lab Technician

**Description:** Orders a diagnostic test to be performed by the in-campus laboratory, and registers the ordering doctor as the recipient of the result.

**Trigger:** A doctor requires a diagnostic test to support or confirm a diagnosis.

**Preconditions:**

1. An active Diagnosis Record or Emergency Record exists for the patient.
2. The acting user holds Doctor privileges.
3. The requested test is available in-campus.

**Postconditions:**

1. A test order exists with patient ID, test type, clinical reason, ordering doctor, and timestamp.
2. The order is queued to the in-campus laboratory.
3. The ordering doctor is registered as the result recipient.

**Alternate Flow:**

- AF-1: Test is unavailable in-campus → the order is redirected to the external path handled by UC-31 (Record External Test Result).
- AF-2: Several tests are ordered together → each is a separate order under a shared order reference.
- AF-3: Test is urgent → the order is flagged priority and surfaced at the top of the laboratory queue.

**Exception Flow:**

- EF-1: A non-doctor attempts to order a test → refused and logged, per FR-33.
- EF-2: The in-campus laboratory is closed or the equipment is unavailable → the order is held with a reason, and staff are prompted to redirect externally.
- EF-3: The patient is a guest → the order is blocked by the guest service restriction.

**Related Requirement:** FR-33, FR-36

**Business Requirement:** BR-02, BR-03, BR-04, BR-09

**Acceptance Criteria:**

1. Test ordering is available only to users holding Doctor privileges — Lab Technicians cannot order.
2. Every order records the ordering doctor as the designated result recipient.
3. Orders for tests unavailable in-campus are redirectable without re-entry of patient data.

### UC-30: Record Test Result

**Use Case ID:** UC-30 **Use Case Name:** Record Test Result **Primary Actor:** Lab Technician **Secondary/External Actor:** Doctor (recipient)

**Description:** Records the result of an in-campus test against its order and routes it to the ordering doctor.

**Trigger:** An in-campus test has been performed and a result is available.

**Preconditions:**

1. An open in-campus test order exists.
2. The acting user holds Lab Technician privileges.

**Postconditions:**

1. The result is stored against the order, with the performing technician and the time performed.
2. The order status becomes completed.
3. The ordering doctor is notified and the result is visible on the patient's record.

**Alternate Flow:**

- AF-1: Result is within normal range → recorded and routed normally.
- AF-2: Result is critical or out of range → flagged and escalated to the ordering doctor with priority.
- AF-3: Result includes an attachment or report file → stored alongside the structured values.
- AF-4: A repeat test is required → a new order is raised, linked to the original.

**Exception Flow:**

- EF-1: The sample is unusable or the test fails → the order is closed as not performed with a reason, and the doctor is notified.
- EF-2: The ordering doctor is no longer available → the result routes to the covering doctor, and the reassignment is recorded.
- EF-3: A result is entered against the wrong order → correction preserves the original entry, records the reason, and notifies both affected doctors.

**Related Requirement:** FR-36

**Business Requirement:** BR-03, BR-07

**Acceptance Criteria:**

1. Results can be recorded only against an existing order.
2. Every result routes to the ordering doctor and is visible on the patient's record.
3. Out-of-range results are flagged distinctly from normal results.
4. Corrections retain the original entry and its author.

### UC-31: Record External Test Result

**Use Case ID:** UC-31 **Use Case Name:** Record External Test Result **Primary Actor:** Medical Staff **Secondary/External Actor:** Doctor, Patient

**Description:** Records the result of a test performed at an external laboratory, stores it for follow-up, and applies the refund cap tied to the affiliated-hospital rate.

**Trigger:** A patient returns with a test result obtained from an external laboratory, or an external result is received by the medical centre.

**Preconditions:**

1. A test referral or a doctor's instruction for an external test exists for the patient.
2. The acting user holds Medical Staff privileges.

**Postconditions:**

1. The external result, its source laboratory, and the test date are stored against the patient's record and linked to the ordering doctor.
2. The result is available for follow-up consultation.
3. Where a refund is claimed, cost handling is applied via «include» UC-39 (Apply Hospital Cost Coverage Rules), capped at the affiliated-hospital rate.

**Alternate Flow:**

- AF-1: Test was performed at an affiliated facility → the full applicable rate is eligible for coverage.
- AF-2: Test was performed at a non-affiliated laboratory → reimbursement is capped at the equivalent affiliated rate, with the patient liable for the difference.
- AF-3: No refund is claimed → the result is recorded for clinical purposes only and cost rules are not invoked.
- AF-4: Result arrives directly from the laboratory rather than via the patient → recorded with the receiving channel noted.

**Exception Flow:**

- EF-1: The result cannot be attributed to an existing order or instruction → it is stored as unsolicited external result and flagged for the doctor to accept or reject.
- EF-2: No equivalent affiliated rate exists for the test → the refund is held pending administrative determination of a comparable rate.
- EF-3: The result document is illegible or unverifiable → it is recorded as received-unverified and excluded from clinical decision-making until confirmed.

**Related Requirement:** FR-37

**Business Requirement:** BR-03, BR-06, BR-07

**Acceptance Criteria:**

1. External results are stored and retrievable alongside in-campus results on the same patient record.
2. Refunds for external tests never exceed the equivalent affiliated rate.
3. Results that cannot be matched to an order are quarantined rather than silently attached.

### UC-32: Access Referral Record

**Use Case ID:** UC-32 **Use Case Name:** Access Referral Record **Primary Actor:** Medical Staff **Secondary/External Actor:** Doctor, Patient

**Description:** Retrieves referral records by patient ID for clinical follow-up, administrative processing, and patient self-service.

**Trigger:** A staff member or patient needs to view referral information.

**Preconditions:**

1. At least one referral record exists for the patient.
2. The requester is the patient concerned or an authorised staff member.

**Postconditions:**

1. The referral records are displayed with their current status.
2. The access event is logged with requester identity and timestamp.
3. No record content is altered.

**Alternate Flow:**

- AF-1: Patient accesses their own referrals → scope is limited to their own records.
- AF-2: Staff access for cost processing → linked cost and coverage status are shown alongside.
- AF-3: Doctor accesses at a follow-up → the referral outcome and any returned results are shown in context.
- AF-4: The slip is requested from within the record → «extend» UC-28 (Retrieve Referral Slip).

**Exception Flow:**

- EF-1: No referrals exist for the patient → an empty result is returned; not an error.
- EF-2: Requester is not authorised for that patient → refused and logged as a potential privacy event.
- EF-3: Records exist under an unreconciled temporary token → the requester is prompted to search by token.

**Related Requirement:** FR-39

**Business Requirement:** BR-03, BR-07, BR-08

**Acceptance Criteria:**

1. Referral records are retrievable by patient ID.
2. A patient can retrieve their own referral records and no others.
3. Every access is logged with requester, patient, and timestamp.
4. Access never modifies record content.

### UC-33: Update Referral Record

**Use Case ID:** UC-33 **Use Case Name:** Update Referral Record **Primary Actor:** Medical Staff (authorised) **Secondary/External Actor:** Doctor

**Description:** Amends an existing referral record — destination, status, outcome, or correction of details — preserving the previous content and its authorship.

**Trigger:** Referral information changes: a destination is confirmed, an outcome is returned, or a detail requires correction.

**Preconditions:**

1. A referral record exists.
2. The acting user holds authorised update privileges for referral records.

**Postconditions:**

1. The referral reflects the updated content.
2. The previous version, the amending user, the timestamp, and the fields changed are retained.
3. Any regenerated slip is marked as a revised copy.

**Alternate Flow:**

- AF-1: An open referral is used and the destination becomes known → the destination is recorded and the status set to used.
- AF-2: The specialist returns an outcome → the outcome is recorded and the referral closed.
- AF-3: The referral is cancelled → status set to cancelled with a reason; the associated slip is invalidated.
- AF-4: The category was recorded incorrectly → corrected as a clerical amendment with a reason.

**Exception Flow:**

- EF-1: The referral is already closed → reopening is required first and is recorded with a reason.
- EF-2: An update would invalidate an already-presented slip → the system warns and marks the reissued slip as revised, retaining the original version.
- EF-3: A user without update privileges attempts an amendment → refused and logged.

**Related Requirement:** FR-39

**Business Requirement:** BR-03, BR-06

**Acceptance Criteria:**

1. No update overwrites history — the prior version remains retrievable.
2. Every update stores the amending user, timestamp, and fields changed.
3. Cancelling a referral invalidates its slip.
4. Update privileges are enforced separately from access privileges.

### UC-34: Maintain Affiliated-Hospital Directory

**Use Case ID:** UC-34 **Use Case Name:** Maintain Affiliated-Hospital Directory **Primary Actor:** Admin **Secondary/External Actor:** IUT Finance Dept «external»

**Description:** Maintains the authoritative list of hospitals affiliated with IUT, together with the reference rates used to determine coverage. Deliberately aggregates add, edit, and deactivate over one reference dataset with a single actor and a common postcondition shape.

**Trigger:** An affiliation agreement is signed, amended, or ended, or reference rates are revised.

**Preconditions:**

1. The acting user holds Admin privileges.

**Postconditions:**

1. The directory reflects the change, with the acting admin and timestamp recorded.
2. Coverage determinations made from that point use the updated directory.
3. Historical entries are retained so that past referrals remain interpretable against the rules in force at the time.

**Alternate Flow:**

- AF-1: Add hospital → a new entry is created with name, location, affiliation start date, and reference rates.
- AF-2: Edit hospital → details or rates are amended, with the effective date recorded.
- AF-3: Deactivate hospital → affiliation is ended with an effective date; the entry is retained, not deleted.
- AF-4: Rates are revised by IUT Finance Dept → updated with the effective date, leaving prior settlements untouched.

**Exception Flow:**

- EF-1: A hospital is deactivated while referrals to it are still open → those referrals remain governed by the affiliation in force at issue, and are flagged for administrative attention.
- EF-2: A duplicate hospital entry is attempted → the system detects the duplicate and requires merge or rejection.
- EF-3: An entry is created without reference rates → it is saved as incomplete and cannot be used for coverage determination until rates are supplied.

**Related Requirement:** FR-40

**Business Requirement:** BR-03, BR-06

**Acceptance Criteria:**

1. Directory entries are versioned by effective date, so historical coverage decisions remain reproducible.
2. Deactivation never deletes an entry.
3. An entry without reference rates cannot be used in a coverage determination.
4. All three sub-flows record the acting admin and timestamp.

### UC-35: Settle Hospital Referral Cost

**Use Case ID:** UC-35 **Use Case Name:** Settle Hospital Referral Cost **Primary Actor:** Admin **Secondary/External Actor:** IUT Finance Dept «external»

**Description:** Settles the financial outcome of a hospital referral: establishes the destination's affiliation status, applies the coverage rules, and records the amounts payable by IUT and by the patient.

**Trigger:** A hospital referral results in a chargeable treatment requiring settlement.

**Preconditions:**

1. A referral to a hospital exists with a recorded destination.
2. Treatment has been provided and cost information is available.
3. The acting user holds Admin privileges.

**Postconditions:**

1. Affiliation status has been determined and recorded against the settlement.
2. Coverage rules have been applied, producing the IUT-covered amount and the patient-liable amount.
3. The settlement is transmitted to, or made available for, IUT Finance Dept.

**Alternate Flow:**

- AF-1: Affiliated hospital with a valid IUT referral → treatment is IUT-covered; «extend» UC-37 (Record Refundable Admit Fee).
- AF-2: Non-affiliated hospital → «extend» UC-38 (Apply Non-Affiliated Cost Cap); the patient is liable for the difference and no refund applies.
- AF-3: Affiliated hospital but no valid IUT referral → coverage is refused under FR-41; the full cost falls to the patient.
- AF-4: Cost arises from an authorised off-campus trip → the same rules are applied via the shared UC-39 (Apply Hospital Cost Coverage Rules).

**Exception Flow:**

- EF-1: The destination hospital is not in the directory → settlement is held pending an entry via UC-34 (Maintain Affiliated-Hospital Directory).
- EF-2: Cost documentation is incomplete or disputed → the settlement is held in status pending verification; no amount is released.
- EF-3: The affiliation status changed between referral and treatment → the status in force at the date of referral governs the settlement.

**Related Requirement:** FR-41, FR-42, FR-43

**Business Requirement:** BR-03, BR-06

**Acceptance Criteria:**

1. Every settlement records IUT-covered and patient-liable amounts separately.
2. Coverage is refused for affiliated-hospital treatment lacking a valid IUT referral.
3. Settlements use the affiliation status and rates in force at the date of referral.
4. No settlement can be finalised while cost documentation is incomplete.

### UC-36: Determine Hospital Affiliation Status

**Use Case ID:** UC-36 **Use Case Name:** Determine Hospital Affiliation Status **Primary Actor:** Admin **Secondary/External Actor:** —

**Description:** Establishes whether a destination hospital was affiliated with IUT at the relevant date, and whether a valid IUT doctor referral exists — the two conditions that gate coverage.

**Trigger:** Included from UC-39 (Apply Hospital Cost Coverage Rules) whenever coverage must be determined.

**Preconditions:**

1. A destination hospital is recorded on the case.
2. The affiliated-hospital directory is populated.

**Postconditions:**

1. The destination is classified as affiliated or non-affiliated as at the relevant date.
2. The presence or absence of a valid IUT referral is recorded.
3. The applicable reference rate is identified.

**Alternate Flow:**

- AF-1: Hospital is affiliated and a valid referral exists → coverage-eligible.
- AF-2: Hospital is affiliated but no valid referral exists → not coverage-eligible under FR-41.
- AF-3: Hospital is not affiliated → the cap path applies, using the nearest equivalent affiliated rate.

**Exception Flow:**

- EF-1: The hospital is absent from the directory → treated as non-affiliated, and the gap is flagged for the directory to be updated.
- EF-2: Affiliation changed between referral date and treatment date → the status at referral date governs, and both dates are recorded.
- EF-3: No equivalent affiliated rate exists for the treatment → the determination is held pending administrative rate assignment.

**Related Requirement:** FR-40, FR-41

**Business Requirement:** BR-06

**Acceptance Criteria:**

1. Determination is made against the directory as at the referral date, not the current date.
2. Absence of a valid IUT referral is sufficient on its own to make affiliated treatment non-covered.
3. Hospitals absent from the directory are never treated as affiliated by default.

### UC-37: Record Refundable Admit Fee

**Use Case ID:** UC-37 **Use Case Name:** Record Refundable Admit Fee **Primary Actor:** Admin **Secondary/External Actor:** IUT Finance Dept «external», Patient

**Description:** Records the refundable admission fee charged for an affiliated-hospital admission and tracks it through to refund.

**Trigger:** Extension of UC-39 (Apply Hospital Cost Coverage Rules). Extension condition: destination is an affiliated hospital and an admission occurred.

**Preconditions:**

1. An affiliated-hospital admission has been determined coverage-eligible.
2. An admit fee has been charged.

**Postconditions:**

1. The admit fee amount, payer, payment date, and refund status are recorded against the settlement.
2. The fee appears as an outstanding refund obligation until settled.

**Alternate Flow:**

- AF-1: Fee paid by the patient → recorded as patient-paid and refundable to the patient.
- AF-2: Fee paid directly by IUT → recorded as institution-paid; no patient refund arises.
- AF-3: Refund is processed → status set to refunded with the date and reference.

**Exception Flow:**

- EF-1: Refund is refused by the hospital → status set to refund denied with a reason, and the case is escalated to IUT Finance Dept.
- EF-2: No admit fee was charged → the extension does not apply and no entry is created.
- EF-3: The admission is later found not to be coverage-eligible → the refund obligation is reversed and the change is recorded with a reason.

**Related Requirement:** FR-42

**Business Requirement:** BR-03, BR-06

**Acceptance Criteria:**

1. Every affiliated-hospital admission with a charged fee carries a refund status.
2. Outstanding refunds are reportable as a list.
3. Refund status changes record date, reference, and acting user.

### UC-38: Apply Non-Affiliated Cost Cap

**Use Case ID:** UC-38 **Use Case Name:** Apply Non-Affiliated Cost Cap **Primary Actor:** Admin **Secondary/External Actor:** IUT Finance Dept «external», Patient

**Description:** Limits IUT's contribution for treatment at a non-affiliated hospital to the equivalent affiliated rate, and records the balance as the patient's liability with no refund.

**Trigger:** Extension of UC-39 (Apply Hospital Cost Coverage Rules). Extension condition: destination is a non-affiliated hospital.

**Preconditions:**

1. Treatment occurred at a hospital determined to be non-affiliated.
2. An equivalent affiliated reference rate is identifiable.

**Postconditions:**

1. The IUT contribution is set to the equivalent affiliated rate, or the actual cost where that is lower.
2. The remaining balance is recorded as patient-liable and marked non-refundable.
3. The reference rate used and its source directory entry are recorded.

**Alternate Flow:**

- AF-1: Actual cost is below the affiliated rate → IUT covers the actual cost; no patient liability arises.
- AF-2: Actual cost exceeds the affiliated rate → IUT covers the rate; the difference is patient-liable.
- AF-3: Treatment arose on an authorised off-campus trip → the same cap applies under FR-27, with the trip incident referenced.

**Exception Flow:**

- EF-1: No equivalent affiliated rate exists → the settlement is held pending administrative rate assignment; no amount is released on an assumed rate.
- EF-2: Treatment was clinically unavoidable at a non-affiliated facility (nearest hospital in an emergency) → the cap still applies as written, and the circumstance is recorded for discretionary review by IUT Finance Dept.
- EF-3: The patient disputes the cap → the dispute is recorded against the settlement and escalated; the calculation basis remains visible.

**Related Requirement:** FR-27, FR-43

**Business Requirement:** BR-06

**Acceptance Criteria:**

1. IUT's contribution never exceeds the equivalent affiliated rate.
2. The patient-liable balance is stored explicitly and marked non-refundable.
3. The reference rate used and its directory source are retrievable for any capped settlement.
4. The same calculation applies to trip-related and referral-related costs.

### UC-39: Apply Hospital Cost Coverage Rules

**Use Case ID:** UC-39 **Use Case Name:** Apply Hospital Cost Coverage Rules **Primary Actor:** Admin **Secondary/External Actor:** IUT Finance Dept «external»

**Description:** The single, shared determination of how external treatment cost is divided between IUT and the patient. Included by both UC-35 (Settle Hospital Referral Cost) and UC-20 (Log Off-Campus Trip Incident), so that one rule set governs both paths.

**Trigger:** Included whenever external treatment cost must be apportioned — from a hospital referral, an off-campus trip incident, or an external test refund claim.

**Preconditions:**

1. A destination facility and a treatment cost are recorded.
2. The affiliated-hospital directory contains the rates in force at the relevant date.

**Postconditions:**

1. The IUT-covered amount and the patient-liable amount are calculated and stored.
2. The rule path applied (affiliated · non-affiliated · not covered) is recorded with the case.
3. The result is available to the calling use case for settlement.

**Alternate Flow:**

- AF-1: Affiliated with a valid referral → «include» UC-36 (Determine Hospital Affiliation Status); full coverage, and «extend» UC-37 (Record Refundable Admit Fee) where an admission occurred.
- AF-2: Non-affiliated → «extend» UC-38 (Apply Non-Affiliated Cost Cap).
- AF-3: Affiliated without a valid referral → coverage refused; full cost is patient-liable.
- AF-4: Called from an off-campus trip incident → identical rules apply, referencing the trip rather than a referral.

**Exception Flow:**

- EF-1: Directory rates are missing for the relevant date → the determination is held; no default rate is assumed.
- EF-2: The calling case has no recorded destination → the determination cannot proceed and returns an incomplete-input result to the caller.
- EF-3: Rules change between treatment date and settlement date → the rules in force at the treatment date govern, and both dates are recorded.

**Related Requirement:** FR-27, FR-41, FR-42, FR-43

**Business Requirement:** BR-06, BR-11

**Acceptance Criteria:**

1. One implementation serves both the referral path and the off-campus trip path — the same inputs produce the same result regardless of caller.
2. Every determination records the rule path applied and the rate source used.
3. No determination completes using an assumed or defaulted rate.