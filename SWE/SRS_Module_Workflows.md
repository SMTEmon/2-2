# IUT Medical Centre System — Module-by-Module Workflows (Happy & Sad Paths)

> [!abstract] 🧭 Lab Final Scenario Guide
> This guide breaks down the system into **16 functional modules** across all 5 scopes.
> For each module, you will find:
> - 🟢 **Happy Path (Primary Success Scenario)**: The standard sequence of steps when everything goes smoothly, with a Mermaid flowchart.
> - 🔴 **Sad Paths & Edge Cases (Alternative & Exception Scenarios)**: What happens when credentials fail, allergies conflict, ambulances are unavailable, doctors disagree, or documents are missing.
> 
> 💡 **Exam Tip**: In your lab final, a use case question often asks for **Main Flow**, **Alternative Flows**, and **Exceptions / Failure Scenarios**. Reading these module stories will make those answers instantly obvious!

## Table of Contents
- [Scope 1: Patient Registration, Profiles, Scheduling & Consultations](#scope-1-patient-registration-profiles-scheduling--consultations)
  - [Module 1.1: Registration & Profiles (UC-1.01 to UC-1.09)](#module-11-registration--profiles-uc-101-to-uc-109)
  - [Module 1.2: Appointments & Vitals Queue (UC-1.10 to UC-1.24)](#module-12-appointments--vitals-queue-uc-110-to-uc-124)
  - [Module 1.3: Consultation & Prescription (UC-1.25 to UC-1.31)](#module-13-consultation--prescription-uc-125-to-uc-131)
- [Scope 2: Intake, Emergency, Triage, Diagnosis & Referrals](#scope-2-intake-emergency-triage-diagnosis--referrals)
  - [Module 2.1: Intake, Triage & Eligibility (UC-2.32 to UC-2.35)](#module-21-intake-triage--eligibility-uc-232-to-uc-235)
  - [Module 2.2: Emergency Treatment & Hospital Transfer (UC-2.36 to UC-2.41)](#module-22-emergency-treatment--hospital-transfer-uc-236-to-uc-241)
  - [Module 2.3: Ambulance Standby & Clinical Dispatch (UC-2.42 to UC-2.46)](#module-23-ambulance-standby--clinical-dispatch-uc-242-to-uc-246)
  - [Module 2.4: Diagnosis, Follow-up & External Referral (UC-2.47 to UC-2.53)](#module-24-diagnosis-follow-up--external-referral-uc-247-to-uc-253)
- [Scope 3: Pharmacy, Inventory, Procurement & Laboratory](#scope-3-pharmacy-inventory-procurement--laboratory)
  - [Module 3.1: Pharmacy Dispensing (UC-3.54 to UC-3.57)](#module-31-pharmacy-dispensing-uc-354-to-uc-357)
  - [Module 3.2: Inventory, Restocking & Asset Maintenance (UC-3.58 to UC-3.65)](#module-32-inventory-restocking--asset-maintenance-uc-358-to-uc-365)
  - [Module 3.3: Laboratory Test Management (UC-3.66 to UC-3.70)](#module-33-laboratory-test-management-uc-366-to-uc-370)
- [Scope 4: Financial, Billing & Multi-Stage Reimbursement Management](#scope-4-financial-billing--multi-stage-reimbursement-management)
  - [Module 4.1: Reimbursement Preparation & Submission (UC-4.71 to UC-4.76)](#module-41-reimbursement-preparation--submission-uc-471-to-uc-476)
  - [Module 4.2: Medical Center Verification & Two-Doctor Sign-off (UC-4.77 to UC-4.82)](#module-42-medical-center-verification--two-doctor-sign-off-uc-477-to-uc-482)
  - [Module 4.3: Financial Re-check, Calculation & Payment (UC-4.83 to UC-4.90)](#module-43-financial-re-check-calculation--payment-uc-483-to-uc-490)
- [Scope 5: Administrative System, Duty Rosters, Notifications & Reporting](#scope-5-administrative-system-duty-rosters-notifications--reporting)
  - [Module 5.1: Staff & Driver Duty Roster Management (UC-5.91 to UC-5.94, UC-5.98 to UC-5.100)](#module-51-staff--driver-duty-roster-management-uc-591-to-uc-594-uc-598-to-uc-5100)
  - [Module 5.2: Paperwork Digitization & Notification System (UC-5.95 to UC-5.97, UC-5.101 to UC-5.103)](#module-52-paperwork-digitization--notification-system-uc-595-to-uc-597-uc-5101-to-uc-5103)
  - [Module 5.3: Management Reporting & Ambulance Dispatch Operations (UC-5.104 to UC-5.109)](#module-53-management-reporting--ambulance-dispatch-operations-uc-5104-to-uc-5109)

---

## Scope 1: Patient Registration, Profiles, Scheduling & Consultations

### Module 1.1: Registration & Profiles (UC-1.01 to UC-1.09)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-1.01 (Log In), UC-1.02 (Verify Credentials), UC-1.03 (Retrieve User Details), UC-1.04 (Create Patient Profile), UC-1.05 (Update Patient Profile), UC-1.06 (Register Walk-In Patient), UC-1.07 (Search Patient Record), UC-1.08 (Verify Patient Eligibility), UC-1.09 (Record Walk-In Patient).
> **Primary Actors**: Patient, Receptionist, System, IUT Authentication Database.

> [!example]- 🟢 Happy Path: Standard Patient Login & Profile Creation / Walk-in Registration
> **Scenario A (Online Patient)**:
> 1. Patient opens application and submits IUT ID & password (`UC-1.01`).
> 2. System verifies credentials against IUT Authentication DB (`UC-1.02`).
> 3. System retrieves student/staff details (Name, Department, Contact) (`UC-1.03`).
> 4. Finding no prior profile, System creates a new Patient Profile with a unique ID (`UC-1.04`).
> 5. Patient views profile dashboard and updates contact/emergency details (`UC-1.05`).
> 
> **Scenario B (Walk-in Patient at Reception)**:
> 1. Patient arrives at reception; Receptionist initiates registration (`UC-1.06`).
> 2. Receptionist searches database by IUT ID or phone (`UC-1.07`).
> 3. Record found $\rightarrow$ Receptionist verifies student/staff eligibility category (`UC-1.08`).
> 4. Receptionist opens a new visit record stamped with date/time (`UC-1.10`).
> 
> ```mermaid
> flowchart TD
>     Start([Patient Arrives / Opens App]) --> Channel{Channel?}
>     
>     %% Online Flow
>     Channel -->|Online Login| Auth["UC-1.01: Submit Credentials<br>UC-1.02: Verify against IUT DB"]
>     Auth --> Fetch["UC-1.03: Retrieve User Details"]
>     Fetch --> CheckProf{Profile Exists?}
>     CheckProf -->|No| CreateP["UC-1.04: Auto-Create Profile"]
>     CheckProf -->|Yes| Dash["Open Role Dashboard"]
>     CreateP --> Dash
>     Dash --> UpdateP["UC-1.05: Update Profile / Emergency Contact"]
>     
>     %% Walk-in Flow
>     Channel -->|Walk-in Reception| Search["UC-1.06/07: Search by ID or Phone"]
>     Search --> Found{Found?}
>     Found -->|Yes| VerifyE["UC-1.08: Verify Eligibility Category"]
>     Found -->|No| ManualReg["UC-1.09: Record Walk-In Details"]
>     ManualReg --> VerifyE
>     VerifyE --> Visit["UC-1.10: Create Visit Record"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Registration & Profiles
> - **Sad Path 1.1.1 (Invalid IUT Credentials)**:
>   - *Trigger*: User types wrong password or non-existent ID during `UC-1.01`.
>   - *Handling*: `UC-1.02` fails validation. System blocks access, shows clear error message "Invalid IUT Credentials", and prevents session initiation.
> - **Sad Path 1.1.2 (IUT Authentication Database Down)**:
>   - *Trigger*: External IUT auth server is unreachable.
>   - *Handling*: System displays a retry notification; offline emergency triage can proceed via temporary tokens in Scope 2 (`UC-2.34`).
> - **Sad Path 1.1.3 (Walk-in Patient Has No IUT Account - Non-IUT Guest)**:
>   - *Trigger*: Visitor or external guest arrives at medical center.
>   - *Handling*: `UC-1.07` search returns no record. Receptionist invokes `UC-1.09` (Record Walk-In Patient) to manually enter Name, Phone, and Sponsor before visit creation.
> - **Sad Path 1.1.4 (Duplicate Profile Prevention)**:
>   - *Trigger*: Receptionist attempts to register a patient who is already in the database.
>   - *Handling*: `UC-1.06` detects matching IUT ID/Phone, rejects duplicate creation, and automatically loads the existing profile.

---

### Module 1.2: Appointments & Vitals Queue (UC-1.10 to UC-1.24)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-1.10 (Create Visit Record), UC-1.11 (Book Appointment), UC-1.12 (Book Online Appointment), UC-1.13 (Book Walk-In Appointment), UC-1.14 (View Doctor Availability), UC-1.15 (Suggest Alternate Doctor), UC-1.16 (Reject Walk-In Attempt), UC-1.17 (Verify Appointment Validity), UC-1.18 (Record Vital Signs), UC-1.19 (Mark Vitals Ready for Review), UC-1.20 (Update Patient Queue Status), UC-1.21 (View Patient Queue), UC-1.22 (Open Patient Record), UC-1.23 (Review Vitals and History), UC-1.24 (Notify Nurse of Missing Vitals).
> **Primary Actors**: Patient, Receptionist, Nurse, Doctor, System.

> [!example]- 🟢 Happy Path: Appointment Booking, Vitals Capture & Doctor Calling
> 1. Patient selects service and date online (`UC-1.11`, `UC-1.12`).
> 2. System checks doctor duty roster and displays available time slots (`UC-1.14`).
> 3. Patient confirms slot; System verifies validity and books appointment (`UC-1.17`).
> 4. On visit day, Patient checks in $\rightarrow$ Nurse takes Blood Pressure, Temp, Pulse, Weight (`UC-1.18`).
> 5. Nurse marks vitals complete (`UC-1.19`) and updates queue status to `Vitals Done / Waiting for Doctor` (`UC-1.20`).
> 6. Doctor views live queue (`UC-1.21`), calls next patient, and opens electronic medical record (`UC-1.22`).
> 7. Doctor reviews complete vital signs and past consultation history (`UC-1.23`).
> 
> ```mermaid
> flowchart TD
>     Book["UC-1.12: Select Service & Date"] --> Avail["UC-1.14: Check Doctor Slots"]
>     Avail --> Valid["UC-1.17: Verify Slot Validity & Confirm"]
>     Valid --> Arrival["Patient Arrives on Consultation Day"]
>     Arrival --> Vitals["UC-1.18: Nurse Records Vitals (BP, Temp, Pulse)"]
>     Vitals --> MarkReady["UC-1.19: Mark Vitals Ready"]
>     MarkReady --> Queue["UC-1.20: Update Queue Status ('Ready')"]
>     Queue --> DocQueue["UC-1.21: Doctor Views Queue"]
>     DocQueue --> CallDoc["UC-1.22: Doctor Opens Record ('With Doctor')"]
>     CallDoc --> Review["UC-1.23: Doctor Reviews Vitals & Past History"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Appointments & Vitals
> - **Sad Path 1.2.1 (Psychiatric Consultation Walk-In Rejected)**:
>   - *Trigger*: Walk-in patient arrives at front desk requesting immediate psychiatric session.
>   - *Handling*: `UC-1.16` triggers. System strictly prohibits psychiatric walk-ins (mandatory appointment-only policy). Receptionist informs patient and guides them to `UC-1.12` online booking.
> - **Sad Path 1.2.2 (Preferred Doctor Fully Booked)**:
>   - *Trigger*: Patient requests a doctor who has 0 open slots.
>   - *Handling*: `UC-1.15` triggers. System scans the same department for available physicians at the earliest time and presents alternative doctor options.
> - **Sad Path 1.2.3 (Double-Booking Slot Collision)**:
>   - *Trigger*: Two users attempt to book the exact same slot simultaneously.
>   - *Handling*: `UC-1.17` validates slot availability at final confirmation. The second submission is rejected with "Slot already taken" and prompted to pick another slot.
> - **Sad Path 1.2.4 (Doctor Opens Record with Missing Vitals)**:
>   - *Trigger*: Doctor opens patient record before nurse completed vitals.
>   - *Handling*: `UC-1.24` triggers automatically. System flags missing parameters and pushes a high-priority prompt to the nurse station to complete vitals.

---

### Module 1.3: Consultation & Prescription (UC-1.25 to UC-1.31)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-1.25 (Create Prescription), UC-1.26 (Check Allergy Conflict), UC-1.27 (Display Allergy Warning), UC-1.28 (Lock Prescription), UC-1.29 (Publish Prescription), UC-1.30 (View Medical Record), UC-1.31 (View Prescription).
> **Primary Actors**: Doctor, Patient, Nurse, System, Pharmacy Module (Scope 3).

> [!example]- 🟢 Happy Path: Digital Prescription, Allergy Clearance & Publishing
> 1. Doctor opens prescription form during consultation (`UC-1.25`).
> 2. Doctor prescribes medications, specifying dosage, frequency, and duration.
> 3. System checks prescribed medicines against patient's recorded allergies in real time (`UC-1.26`).
> 4. No conflict found $\rightarrow$ Doctor reviews and clicks Finalize.
> 5. System locks the prescription to make it immutable and read-only (`UC-1.28`).
> 6. System publishes the prescription (`UC-1.29`), making it instantly viewable in:
>    - Patient Portal (`UC-1.31`)
>    - Nurse Station (`UC-1.31`)
>    - Pharmacy Dispensing Queue (`Scope 3 / UC-3.56`)
> 
> ```mermaid
> flowchart TD
>     StartRx["UC-1.25: Doctor Enters Medicines & Dosage"] --> AutoCheck["UC-1.26: System Checks Patient Allergy Profile"]
>     AutoCheck --> Conflict{Allergy Clash?}
>     Conflict -->|No Conflict| Finalize["Doctor Finalizes Rx"]
>     Finalize --> Lock["UC-1.28: System Locks Rx (Immutable)"]
>     Lock --> Pub["UC-1.29: Publish Prescription"]
>     Pub --> P_View["UC-1.31: Patient Views Rx"]
>     Pub --> Ph_Queue["Scope 3: Sent to Pharmacy Dispensing Queue"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Consultation & Prescription
> - **Sad Path 1.3.1 (Allergy Conflict Triggered)**:
>   - *Trigger*: Doctor prescribes Penicillin, but patient profile has a recorded Penicillin allergy.
>   - *Handling*: `UC-1.27` triggers. System flashes a red warning modal detailing the allergen and drug clash. Doctor must either (a) remove/replace the drug, or (b) enter an explicit clinical override justification before the system allows submission.
> - **Sad Path 1.3.2 (Attempt to Modify Finalized/Locked Prescription)**:
>   - *Trigger*: Staff tries to edit an issued prescription.
>   - *Handling*: `UC-1.28` enforces strict read-only immutability. Editing is blocked. If an adjustment is needed, a formal new prescription or addendum visit must be initiated.
> - **Sad Path 1.3.3 (Publishing Service Interruption)**:
>   - *Trigger*: Network disconnect when sending Rx to Scope 3 Pharmacy queue.
>   - *Handling*: `UC-1.29` retains the locked Rx in local persistent storage and retries queue delivery automatically without losing data.

---

## Scope 2: Intake, Emergency, Triage, Diagnosis & Referrals

### Module 2.1: Intake, Triage & Eligibility (UC-2.32 to UC-2.35)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-2.32 (Triage & Route Arrival), UC-2.33 (Confirm/Override Visit Category), UC-2.34 (Open & Reconcile Temporary Patient Record), UC-2.35 (Verify Eligibility & Record Guest Sponsor).
> **Primary Actors**: Receptionist, Nurse, Doctor, Patient, Attendant.

> [!example]- 🟢 Happy Path: Standard Triage & Category Confirmation
> 1. Patient arrives at reception; Staff asks presenting complaint (`UC-2.32`).
> 2. Staff logs self-declared category (`Diagnosis` or `Emergency`).
> 3. Case is routed to the corresponding department queue.
> 4. Attending Doctor/Nurse reviews presentation and confirms the category (`UC-2.33`).
> 5. Receptionist verifies patient ID against IUT student/staff records and sets full eligibility coverage (`UC-2.35`).
> 
> ```mermaid
> flowchart TD
>     Arrive([Patient Arrival]) --> Triage["UC-2.32: Triage & Record Declared Category"]
>     Triage --> Route{Category?}
>     Route -->|Emergency| EmerPath["Emergency Care Workflow"]
>     Route -->|Diagnosis| DiagPath["Diagnosis Consultation Workflow"]
>     EmerPath --> ClinReview["UC-2.33: Doctor/Nurse Confirms Category"]
>     DiagPath --> ClinReview
>     ClinReview --> Elig["UC-2.35: Verify IUT Eligibility Class"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Intake & Triage
> - **Sad Path 2.1.1 (Unconscious / Unidentified Patient Arrival)**:
>   - *Trigger*: Patient arrives unconscious with no ID or attendant.
>   - *Handling*: `UC-2.34` triggers immediately. Staff generates a temporary token record. Stabilizing emergency care proceeds without waiting for ID. Once identity is later discovered, staff reconciles the token with their permanent IUT profile with zero data loss.
> - **Sad Path 2.1.2 (Ambiguous Presentation / Undetermined Severity)**:
>   - *Trigger*: Patient is unable to clearly describe symptoms and has no vitals yet.
>   - *Handling*: `UC-2.32` enforces safety default: **Ambiguous arrivals default directly to Emergency** pending clinical assessment.
> - **Sad Path 2.1.3 (Unsponsored Outsider / Non-IUT Visitor)**:
>   - *Trigger*: Non-IUT member arrives with no university sponsor.
>   - *Handling*: `UC-2.35` caps service level: Stabilizing emergency care is provided, but non-emergency Diagnosis consultations and external hospital referral coverage are strictly blocked.

---

### Module 2.2: Emergency Treatment & Hospital Transfer (UC-2.36 to UC-2.41)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-2.36 (Create Emergency Record), UC-2.37 (Log Emergency Procedures), UC-2.38 (Record Emergency Outcome & Linked Diagnosis), UC-2.39 (Create Hospital Transfer & Retrieve Transfer Slip), UC-2.40 (Retrieve & Search Emergency Records), UC-2.41 (Handle After-Hours Hotline Call & On-Site Care).
> **Primary Actors**: Doctor, Nurse, Receptionist, Patient/Attendant.

> [!example]- 🟢 Happy Path: Emergency Care & External Hospital Transfer
> 1. Case enters emergency; Doctor/Nurse creates Emergency Record with minimal ID/token in < 3s (`UC-2.36`).
> 2. Clinical staff performs and logs procedures: Oxygen, IV fluids, wound dressing (`UC-2.37`).
> 3. Doctor reviews condition and records disposition outcome as `Transferred` (`UC-2.38`).
> 4. Doctor selects receiving hospital and creates Transfer Record (`UC-2.39`).
> 5. System generates official digital Emergency Transfer Slip; Reception prints A4 copy for ambulance attendant (`UC-2.39`).
> 
> ```mermaid
> flowchart TD
>     OpenE["UC-2.36: Create Emergency Record (< 3s)"] --> LogProc["UC-2.37: Log Procedures (Oxygen, IV, Dressing)"]
>     LogProc --> Outcome["UC-2.38: Doctor Selects Outcome ('Transferred')"]
>     Outcome --> Trans["UC-2.39: Create Hospital Transfer Record"]
>     Trans --> Slip["Generate & Print A4 Transfer Slip"]
>     Slip --> Disp["Hand off to Ambulance Dispatch"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Emergency & Transfers
> - **Sad Path 2.2.1 (Doctor Shift Ends Before Disposition Recorded)**:
>   - *Trigger*: Attending doctor's duty shift finishes while an emergency case is ongoing.
>   - *Handling*: `UC-2.38` flags the record as `Incomplete Outcome` and surfaces it on the incoming shift doctor's emergency dashboard during handover.
> - **Sad Path 2.2.2 (Receiving Hospital Unable to Accept Transfer)**:
>   - *Trigger*: Designated hospital contacts medical center stating intensive care beds are full.
>   - *Handling*: `UC-2.39` allows doctor to quickly switch destination hospital and instantly regenerate the transfer slip without re-entering medical data.
> - **Sad Path 2.2.3 (After-Hours Emergency Hotline Call)**:
>   - *Trigger*: Severe medical incident occurs in student dorms at 2:00 AM.
>   - *Handling*: `UC-2.41` triggers. On-duty nurse logs caller and campus location, dispatches on-site with first-aid kit before doctor arrives, and records care start timestamp.

---

### Module 2.3: Ambulance Standby & Clinical Dispatch (UC-2.42 to UC-2.46)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-2.42 (Maintain Ambulance & Driver Availability), UC-2.43 (Dispatch Ambulance - Authorisation & Availability), UC-2.44 (Nurse Fallback Ambulance Authorisation), UC-2.45 (Log Self-Transport Fallback), UC-2.46 (Log Off-Campus Trip Incident).
> **Primary Actors**: Doctor, Nurse, Administrator, Ambulance Driver, Transport Coordinator.

> [!example]- 🟢 Happy Path: Standby Check & Doctor-Authorized Dispatch
> 1. Doctor decides ambulance transport is mandatory for critical transfer.
> 2. System queries live standby availability roster (`UC-2.42`).
> 3. Available vehicle and driver confirmed $\rightarrow$ Doctor authorizes dispatch with destination hospital (`UC-2.43`).
> 4. System marks vehicle as `On-Trip` and passes execution to Transport Coordinator in Scope 5 (`UC-5.107`).
> 
> ```mermaid
> flowchart TD
>     NeedAmb([Critical Patient Requires Ambulance]) --> CheckRoster["UC-2.42: Query Live Vehicle & Driver Standby"]
>     CheckRoster --> Avail{Available?}
>     Avail -->|Yes| DocAuth["UC-2.43: Doctor Authorizes Dispatch"]
>     DocAuth --> MarkTrip["Vehicle Marked 'On-Trip'"]
>     MarkTrip --> Scope5Exec["Scope 5: Operational Dispatch Execution"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Ambulance Dispatch
> - **Sad Path 2.3.1 (Doctor Unreachable During Time-Critical Dispatch)**:
>   - *Trigger*: Life-threatening emergency; on-call doctor cannot be reached by phone immediately.
>   - *Handling*: `UC-2.44` (*`<<extends UC-43>>`*) triggers. System records failed doctor contact and permits Nurse Fallback Authorisation. System flags the dispatch with a mandatory retrospective doctor/admin review requirement.
> - **Sad Path 2.3.2 (Zero Ambulances / Drivers Available - Self-Transport Fallback)**:
>   - *Trigger*: All campus ambulances are already deployed or in maintenance.
>   - *Handling*: `UC-2.45` triggers. Staff logs self-transport arrangement (e.g. taxi/personal car) including destination and vehicle unavailability reason, marking the visit for financial transport cost reimbursement under Scope 4.
> - **Sad Path 2.3.3 (Medical Emergency on Authorized Off-Campus Study Trip)**:
>   - *Trigger*: Student injured during an official university field trip.
>   - *Handling*: `UC-2.46` triggers. Staff verifies IUT trip authorization, logs hotline approval, and tags external hospital bills for reimbursement eligibility.

---

### Module 2.4: Diagnosis, Follow-up & External Referral (UC-2.47 to UC-2.53)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-2.47 (Create & Update Diagnosis Record), UC-2.48 (Access Diagnosis Record & Record Treatment Period), UC-2.49 (Record Follow-up Visit & Auto-Close Missed Follow-up), UC-2.50 (Create Referral & Retrieve Referral Slip), UC-2.51 (Order & Record In-Campus/External Test Results), UC-2.52 (Access & Update Referral Record), UC-2.53 (Hospital Affiliation & Cost Settlement).
> **Primary Actors**: Doctor, Nurse, Receptionist, Patient, External Hospital.

> [!example]- 🟢 Happy Path: Diagnosis, Follow-up & Specialist Referral
> 1. Case routed to Diagnosis; Doctor documents symptoms, diagnosis, and treatment plan (`UC-2.47`).
> 2. Doctor records prescribed treatment recovery period (e.g. 7 days) (`UC-2.48`).
> 3. Patient returns for follow-up within 7 days $\rightarrow$ Doctor logs follow-up visit (`UC-2.49`).
> 4. Condition requires specialist care $\rightarrow$ Doctor creates Referral to affiliated hospital (`UC-2.50`).
> 5. System generates official Referral Slip for the patient; Hospital affiliation coverage is recorded (`UC-2.53`).
> 
> ```mermaid
> flowchart TD
>     Diag["UC-2.47: Record Clinical Diagnosis & Symptoms"] --> Period["UC-2.48: Set Prescribed Treatment Period (e.g. 7 Days)"]
>     Period --> PatientReturn{Patient Returns in Window?}
>     PatientReturn -->|Yes| FollowUp["UC-2.49: Record Follow-Up Encounter"]
>     FollowUp --> NeedRef{Specialist Needed?}
>     NeedRef -->|Yes| CreateRef["UC-2.50: Create Referral & Generate Referral Slip"]
>     CreateRef --> Settle["UC-2.53: Apply Hospital Affiliation Coverage"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Diagnosis & Follow-up
> - **Sad Path 2.4.1 (Patient Misses Follow-Up Window - Auto-Closure)**:
>   - *Trigger*: Prescribed 7-day treatment period elapses without patient returning.
>   - *Handling*: `UC-2.49` auto-closes the diagnosis case as `Presumed Recovered` with an automated closure timestamp.
> - **Sad Path 2.4.2 (Patient Returns After Auto-Closure)**:
>   - *Trigger*: Patient returns on Day 12 with recurring symptoms.
>   - *Handling*: `UC-2.49` allows doctor to manually reopen the case, preserving all historical clinical entries while annotating the reopening reason.
> - **Sad Path 2.4.3 (Non-Doctor Attempts Referral Creation)**:
>   - *Trigger*: Administrative staff or nurse attempts to generate a specialist referral.
>   - *Handling*: `UC-2.50` strictly blocks unauthorized creation; referrals require doctor-exclusive authorization (with traceable exceptions for doctor-dictated call entries).

---

## Scope 3: Pharmacy, Inventory, Procurement & Laboratory

### Module 3.1: Pharmacy Dispensing (UC-3.54 to UC-3.57)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-3.54 (Patient Identification), UC-3.55 (Search Medicine), UC-3.56 (Dispense Medicine), UC-3.57 (Handle Wrong Dispensing & Medicine Reissue).
> **Primary Actors**: Pharmacist, Patient, System.

> [!example]- 🟢 Happy Path: Prescription Verification & Medicine Dispensing
> 1. Patient arrives at pharmacy counter; Pharmacist verifies Student/Staff ID (`UC-3.54`).
> 2. System retrieves active published digital prescriptions for that patient.
> 3. Pharmacist searches medicine inventory to locate shelf position (`UC-3.55`).
> 4. Pharmacist confirms items and quantities, and clicks Dispense (`UC-3.56`).
> 5. System decrements medicine stock in real-time and logs the dispensing transaction.
> 
> ```mermaid
> flowchart TD
>     PatID["UC-3.54: Pharmacist Verifies Patient ID"] --> LoadRx["System Loads Published Rx"]
>     LoadRx --> SearchMed["UC-3.55: Search Medicine & Shelf Location"]
>     SearchMed --> Dispense["UC-3.56: Pharmacist Dispenses Medication"]
>     Dispense --> UpdateStock["System Auto-Decrements Stock Count"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Pharmacy Dispensing
> - **Sad Path 3.1.1 (Wrong Medicine / Dosage Dispensed - Error Recovery)**:
>   - *Trigger*: Pharmacist inadvertently dispenses 500mg instead of 250mg or wrong brand.
>   - *Handling*: `UC-3.57` triggers. Pharmacist logs a formal Dispensing Error incident report. System restores incorrectly deducted stock, logs discrepancy, decrements the correct item, and issues the right medicine.
> - **Sad Path 3.1.2 (Unpublished / Draft Prescription Presented)**:
>   - *Trigger*: Patient requests medicine while doctor's prescription is still in unlocked draft.
>   - *Handling*: `UC-3.56` blocks dispensing; only finalized, locked, and published prescriptions can be fulfilled.

---

### Module 3.2: Inventory, Restocking & Asset Maintenance (UC-3.58 to UC-3.65)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-3.58 (Generate Low-Stock Alert), UC-3.59 (Low-Stock Restock Request & Tracking), UC-3.60 (Emergency Medicine Procurement), UC-3.61 (Large-Quantity Bulk Procurement & Tender), UC-3.62 (Expired Medicine Discovery, Registry & Disposal), UC-3.63 (Medicine Registration & Shelf Allocation), UC-3.64 (Equipment Asset Registration & Storeroom Management), UC-3.65 (Asset Maintenance & Damage Handling).
> **Primary Actors**: Pharmacist, Nurse, CMO, Chairman, PPD, Equipment Vendor.

> [!example]- 🟢 Happy Path: Low-Stock Restocking & Shelf Registration
> 1. Medicine stock drops below configured reorder threshold.
> 2. System automatically generates Low-Stock Alert (`UC-3.58`).
> 3. Pharmacist submits standard restock requisition (`UC-3.59`).
> 4. Shipment arrives $\rightarrow$ Pharmacist enters Batch, Expiry Date, Quantity, and Shelf Location (`UC-3.63`).
> 5. Stock becomes immediately available for dispensing.
> 
> ```mermaid
> flowchart TD
>     Threshold[Stock < Minimum Level] --> Alert["UC-3.58: System Auto-Generates Low-Stock Alert"]
>     Alert --> Req["UC-3.59: Pharmacist Submits Restock Requisition"]
>     Req --> Delivery[Shipment Arrives at Medical Center]
>     Delivery --> Register["UC-3.63: Register Batch, Expiry & Shelf Location"]
>     Register --> StockReady[Inventory Updated & Available]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Inventory & Procurement
> - **Sad Path 3.2.1 (Critical Life-Saving Drug Depleted - Emergency Procurement)**:
>   - *Trigger*: Emergency room runs out of critical anti-venom or adrenaline.
>   - *Handling*: `UC-3.60` triggers. Standard multi-week procurement is bypassed; Chief Medical Officer (CMO) provides direct emergency authorization for instant local purchase.
> - **Sad Path 3.2.2 (Annual Bulk Order Requiring Tender)**:
>   - *Trigger*: High-value, large-volume medicine procurement.
>   - *Handling*: `UC-3.61` requires institutional Chairman approval and routing through the Purchase & Procurement Division (PPD) for competitive bidding.
> - **Sad Path 3.2.3 (Expired Medicine Discovery)**:
>   - *Trigger*: Routine check or expiry alert flags past-expiry drugs.
>   - *Handling*: `UC-3.62` triggers. Nurse quarantines medicine immediately, registers it for disposal, and obtains formal CMO sign-off before safe destruction.
> - **Sad Path 3.2.4 (Medical Equipment Breakdown)**:
>   - *Trigger*: ECG machine or sterilizer malfunctions.
>   - *Handling*: `UC-3.65` triggers. Staff logs damage, changes status to `Non-Operational`, and triggers external vendor maintenance request.

---

### Module 3.3: Laboratory Test Management (UC-3.66 to UC-3.70)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-3.66 (Request Laboratory Test), UC-3.67 (Sample Collection & Computerized Token Labeling), UC-3.68 (Upload & Verify Laboratory Test Result), UC-3.69 (Report Distribution, Retrieval & Uncollected Report Disposal), UC-3.70 (Generate Monthly Inventory & Laboratory Reports).
> **Primary Actors**: Doctor, Laboratory Technician, Pathologist, Patient, Finance.

> [!example]- 🟢 Happy Path: Test Ordering, Tokenized Sampling & Verified Results
> 1. Doctor orders diagnostic blood test during consultation (`UC-3.66`).
> 2. Order appears in Lab Queue $\rightarrow$ Patient arrives $\rightarrow$ Lab Tech collects sample (`UC-3.67`).
> 3. System generates computerized unique token label and prints barcode for vial (`UC-3.67`).
> 4. Test is run $\rightarrow$ Lab Tech enters results into system (`UC-3.68`).
> 5. Pathologist reviews and verifies findings (`UC-3.68`).
> 6. Report is distributed electronically to Doctor and Patient Portal (`UC-3.69`).
> 
> ```mermaid
> flowchart TD
>     Order["UC-3.66: Doctor Orders Lab Test"] --> Queue["Request Placed in Lab Queue"]
>     Queue --> Collect["UC-3.67: Tech Collects Sample & Generates Barcode Token"]
>     Collect --> Analyze[Sample Processed in Laboratory]
>     Analyze --> Upload["UC-3.68: Tech Enters Test Readings"]
>     Upload --> Verify["UC-3.68: Pathologist Digitally Verifies Result"]
>     Verify --> Distribute["UC-3.69: Report Distributed to Doctor & Patient"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Laboratory Management
> - **Sad Path 3.3.1 (Hemolyzed / Compromised Sample)**:
>   - *Trigger*: Blood sample clots or is contaminated during transit.
>   - *Handling*: `UC-3.67` flags sample as invalid, notifies patient and ordering doctor, and requests urgent sample re-collection.
> - **Sad Path 3.3.2 (Inconclusive Test Result)**:
>   - *Trigger*: Analyzer output is ambiguous or uncalibrated.
>   - *Handling*: `UC-3.68` prevents unverified release; pathologist marks result inconclusive and triggers re-testing.
> - **Sad Path 3.3.3 (Uncollected Physical Lab Reports)**:
>   - *Trigger*: Patient never collects printed laboratory report.
>   - *Handling*: `UC-3.69` retains physical and digital records for 1 full year before initiating administrative archive under Scope 5.

---

## Scope 4: Financial, Billing & Multi-Stage Reimbursement Management

### Module 4.1: Reimbursement Preparation & Submission (UC-4.71 to UC-4.76)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-4.71 (Prepare Reimbursement Request), UC-4.72 (Save and Resume Reimbursement Draft), UC-4.73 (Attach and Manage Supporting Documents), UC-4.74 (Submit Reimbursement Request), UC-4.75 (View Request Status and Acknowledgment), UC-4.76 (Respond to Reimbursement Correction Request).
> **Primary Actors**: Applicant (Student/Faculty/Staff), Medical Center Staff.

> [!example]- 🟢 Happy Path: Claim Preparation, Bill Upload & Submission
> 1. Applicant opens New Reimbursement Claim form (`UC-4.71`).
> 2. Applicant enters treatment dates, hospital name, expense items, and amounts.
> 3. Applicant uploads supporting digital bills, prescriptions, and receipts (`UC-4.73`).
> 4. Applicant reviews summary, checks declaration box, and clicks Submit (`UC-4.74`).
> 5. System generates unique Claim Reference and instant Acknowledgment receipt (`UC-4.75`).
> 
> ```mermaid
> flowchart TD
>     NewClaim["UC-4.71: Open Claim Form & Enter Expense Items"] --> Attach["UC-4.73: Upload Bills, Prescriptions & Receipts"]
>     Attach --> Review["UC-4.74: Review Declaration & Submit Claim"]
>     Review --> Ack["UC-4.75: System Generates Unique Claim Ref & Receipt"]
>     Ack --> StaffQueue[Placed in Medical Center Verification Queue]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Claim Preparation & Submission
> - **Sad Path 4.1.1 (Incomplete Session - Save as Draft)**:
>   - *Trigger*: Applicant lacks a receipt and must leave the computer.
>   - *Handling*: `UC-4.72` saves form state as `Draft`. Applicant resumes later without losing entered line items.
> - **Sad Path 4.1.2 (Corrupt / Oversized File Upload)**:
>   - *Trigger*: Applicant uploads unreadable 50MB TIFF image.
>   - *Handling*: `UC-4.73` validates format and size (< 10MB PDF/JPEG), rejects invalid upload, and shows actionable format guidelines.
> - **Sad Path 4.1.3 (Responding to Staff Correction Request)**:
>   - *Trigger*: Staff flags an illegible pharmacy bill during review.
>   - *Handling*: `UC-4.76` alerts applicant. Applicant opens claim, uploads replacement bill for the specific flagged item, and resubmits under the same claim ID without creating duplicate claims.

---

### Module 4.2: Medical Center Verification & Two-Doctor Sign-off (UC-4.77 to UC-4.82)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-4.77 (Receive and Register Reimbursement Request), UC-4.78 (Check Applicant Eligibility), UC-4.79 (Verify Bills and Supporting Records), UC-4.80 (Request and Receive Corrected Documents), UC-4.81 (Perform Two-Doctor Verification and Sign-off), UC-4.82 (Forward Verified Request to Accounts and Finance).
> **Primary Actors**: Medical Center Staff, Doctor 1, Doctor 2, CMO.

> [!example]- 🟢 Happy Path: Document Verification & Independent Two-Doctor Approval
> 1. Staff receives claim and logs it into physical/digital logbook (`UC-4.77`).
> 2. Staff checks applicant eligibility against IUT institutional records (`UC-4.78`).
> 3. Staff validates each bill against submitted prescriptions and referrals (`UC-4.79`).
> 4. First Doctor reviews medical necessity and signs off digitally (`UC-4.81`).
> 5. Second Independent Doctor reviews and signs off digitally (`UC-4.81`).
> 6. Status changes to `Doctor Verified` $\rightarrow$ Staff forwards claim to Accounts & Finance (`UC-4.82`).
> 
> ```mermaid
> flowchart TD
>     Receive["UC-4.77: Register Claim in Logbook"] --> Elig["UC-4.78: Verify Applicant Eligibility"]
>     Elig --> CheckBills["UC-4.79: Validate Bills & Medical Evidence"]
>     CheckBills --> Doc1["UC-4.81: Doctor 1 Reviews & Signs"]
>     Doc1 --> Doc2["UC-4.81: Doctor 2 Independently Reviews & Signs"]
>     Doc2 --> Forward["UC-4.82: Forward Verified Claim to Finance Queue"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Medical Center Verification
> - **Sad Path 4.2.1 (Applicant Ineligible for Reimbursement)**:
>   - *Trigger*: Discharged student or unauthorized relation submits claim.
>   - *Handling*: `UC-4.78` fails eligibility check. Staff marks claim `Ineligible` with recorded reason; claim is terminated and cannot proceed to calculation.
> - **Sad Path 4.2.2 (Missing / Deficient Supporting Documents)**:
>   - *Trigger*: Claim lacks hospital discharge summary.
>   - *Handling*: `UC-4.80` returns claim to applicant with specific correction note, shifting status to `Correction Required`.
> - **Sad Path 4.2.3 (Two Doctors Disagree on Medical Necessity)**:
>   - *Trigger*: Doctor 1 approves specialized test; Doctor 2 flags it as elective/unjustified.
>   - *Handling*: `UC-4.81` detects disagreement, locks standard forwarding, and escalates the claim directly to the **Chief Medical Officer (CMO)** for binding resolution.

---

### Module 4.3: Financial Re-check, Calculation & Payment (UC-4.83 to UC-4.90)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-4.83 (Perform Finance Re-check), UC-4.84 (Calculate Gross and Eligible Payable Amount), UC-4.85 (Apply Reimbursement Deductions), UC-4.86 (Generate Payment Voucher), UC-4.87 (Record Manual Payment Status and Evidence), UC-4.88 (Generate Monthly and Yearly Reimbursement Reports), UC-4.89 (Manage Roles, Policies, Notifications, and Backups), UC-4.90 (Search and Review Audit Trail).
> **Primary Actors**: Accounts/Finance Officer, Auditor.

> [!example]- 🟢 Happy Path: Financial Calculation, Deductions & Voucher Issuance
> 1. Finance Officer opens medically verified claim and performs financial re-check (`UC-4.83`).
> 2. System applies institutional rules to calculate Eligible vs. Excluded expenses (`UC-4.84`).
> 3. System computes gross payable amount and applies policy deductions (e.g. 10% copay or category caps) (`UC-4.85`).
> 4. Finance Officer confirms final net payable amount.
> 5. System generates official Payment Voucher (`UC-4.86`).
> 6. Finance executes disbursement and records manual cheque/EFT reference (`UC-4.87`).
> 
> ```mermaid
> flowchart TD
>     FinCheck["UC-4.83: Finance Officer Performs Re-Check"] --> Calc["UC-4.84: System Calculates Eligible & Excluded Amounts"]
>     Calc --> Deduct["UC-4.85: Apply Policy / Category Deductions"]
>     Deduct --> Voucher["UC-4.86: Generate Official Payment Voucher"]
>     Voucher --> Pay["UC-4.87: Record Payment Reference & Close Claim"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Finance & Billing
> - **Sad Path 4.3.1 (Non-Reimbursable Expenses Included in Bill)**:
>   - *Trigger*: Hospital bill includes luxury cabin fees, food items, or non-medical toiletries.
>   - *Handling*: `UC-4.84` itemizes expense lines: allowable medicines are categorized as `Eligible`, while non-medical items are categorized as `Excluded` and deducted from gross payable.
> - **Sad Path 4.3.2 (Discrepancy Between Claim and Bank Proof)**:
>   - *Trigger*: Claimed amount doesn't match official hospital paid receipt.
>   - *Handling*: `UC-4.83` places claim on `Financial Clarification Hold` and returns it to Medical Center staff for re-verification.
> - **Sad Path 4.3.3 (External Audit Investigation)**:
>   - *Trigger*: Auditor reviews historical high-value claim.
>   - *Handling*: `UC-4.90` allows auditor read-only access to immutable audit trails showing every timestamp, officer sign-off, and calculation version.

---

## Scope 5: Administrative System, Duty Rosters, Notifications & Reporting

### Module 5.1: Staff & Driver Duty Roster Management (UC-5.91 to UC-5.94, UC-5.98 to UC-5.100)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-5.91 (Process duty schedule approvals), UC-5.92 (Distribute the duty schedule), UC-5.93 (Coordinate staff shift changes), UC-5.94 (Finalize staff shift updates), UC-5.98 (Process driver schedule approvals), UC-5.99 (Distribute driver schedules), UC-5.100 (Coordinate driver shift changes).
> **Primary Actors**: Transport Officer, Administrator, Doctors, Nurses, Ambulance Drivers.

> [!example]- 🟢 Happy Path: Roster Approval, Distribution & Shift Changes
> 1. Duty/Transport Officer prepares monthly medical staff and driver schedule.
> 2. Administrator reviews shift coverage and approves schedule (`UC-5.91`, `UC-5.98`).
> 3. System automatically publishes and distributes schedules to user portals (`UC-5.92`, `UC-5.99`).
> 4. Staff/Driver views assigned shifts and submits planned shift swap request (`UC-5.93`, `UC-5.100`).
> 5. Admin confirms replacement personnel and finalizes update (`UC-5.94`).
> 
> ```mermaid
> flowchart TD
>     DraftRoster[Draft Roster Created] --> AdminAppr["UC-5.91/98: Administrator Approves Schedule"]
>     AdminAppr --> Distribute["UC-5.92/99: Auto-Distribute Schedule to Portals"]
>     Distribute --> SwapReq["UC-5.93/100: Staff/Driver Requests Shift Swap"]
>     SwapReq --> AdminSwap["UC-5.94: Admin Confirms Coverage & Finalizes Update"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Duty Rosters
> - **Sad Path 5.1.1 (Shift Swap Leaves Ambulance Uncovered)**:
>   - *Trigger*: Ambulance driver requests leave on night shift with no replacement.
>   - *Handling*: `UC-5.100` detects 0-driver coverage gap, blocks approval, and alerts administrator that an alternate driver must be assigned before swap is finalized.
> - **Sad Path 5.1.2 (Unapproved Roster Publication Blocked)**:
>   - *Trigger*: System scheduler attempts to publish draft roster.
>   - *Handling*: `UC-5.91` blocks distribution until explicit digital approval from the Administrator is logged.

---

### Module 5.2: Paperwork Digitization & Notification System (UC-5.95 to UC-5.97, UC-5.101 to UC-5.103)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-5.95 (Dispatch system notifications), UC-5.96 (Choose Type & Deliver Notification), UC-5.97 (Track notification logs), UC-5.101 (Process intake paperwork), UC-5.102 (Handle digital records), UC-5.103 (Process archive requests).
> **Primary Actors**: Receptionist, Administrator, System, Notification Service.

> [!example]- 🟢 Happy Path: Reception Paperwork Scanning & Notification Delivery
> 1. Physical referral letter arrives at reception desk (`UC-5.101`).
> 2. Receptionist scans document; System converts it to searchable PDF and tags metadata (`UC-5.101`, `UC-5.102`).
> 3. Trigger event occurs (e.g. appointment reminder or duty change) $\rightarrow$ System dispatches push/SMS notification (`UC-5.95`, `UC-5.96`).
> 4. Delivery receipt is confirmed and written to audit log (`UC-5.97`).
> 5. Old documents reach retention limit $\rightarrow$ Admin approves archival (`UC-5.103`).
> 
> ```mermaid
> flowchart TD
>     Paper[Physical Document Received] --> Scan["UC-5.101: Receptionist Scans Document"]
>     Scan --> Index["UC-5.102: System Indexes & Links to Patient Profile"]
>     Index --> Event[System Trigger Event Occurs]
>     Event --> Notify["UC-5.95/96: Notification Dispatched (SMS/Push)"]
>     Notify --> Log["UC-5.97: Delivery Status Logged in Audit Trail"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Digitization & Notifications
> - **Sad Path 5.2.1 (Poor Quality / Unreadable Scan)**:
>   - *Trigger*: Scanner feed produces blurry or cut-off document.
>   - *Handling*: `UC-5.101` prompts receptionist to review scan preview and triggers rescan before saving to patient record.
> - **Sad Path 5.2.2 (SMS Gateway Failure)**:
>   - *Trigger*: External SMS gateway down during shift alert broadcast.
>   - *Handling*: `UC-5.96` queues failed messages in a retry buffer while displaying the notification immediately on the web portal.
> - **Sad Path 5.2.3 (Unauthorized Archive Attempt)**:
>   - *Trigger*: Staff tries to archive active medical records.
>   - *Handling*: `UC-5.103` blocks automatic archiving; requires explicit Administrator approval.

---

### Module 5.3: Management Reporting & Ambulance Dispatch Operations (UC-5.104 to UC-5.109)

> [!info] 📌 Module Overview
> **Use Cases Included**: UC-5.104 (Process department report data), UC-5.105 (Finalize management reports), UC-5.106 (Access report dashboards), UC-5.107 (Handle ambulance dispatching), UC-5.108 (Track live trip updates), UC-5.109 (Review vehicle trip history).
> **Primary Actors**: Transport Coordinator, Ambulance Driver, Department Officer, CMO, Administrator.

> [!example]- 🟢 Happy Path: Operational Ambulance Trip Execution & Management Reports
> 1. Scope 2 authorizes emergency transfer $\rightarrow$ Transport Coordinator assigns ambulance and driver (`UC-5.107`).
> 2. Driver receives route details and updates live status: `En Route` $\rightarrow$ `Picked Up` $\rightarrow$ `Completed` (`UC-5.108`).
> 3. Trip distance, duration, and remarks are stored in vehicle history (`UC-5.109`).
> 4. Department officers submit monthly stats $\rightarrow$ System consolidates data (`UC-5.104`).
> 5. CMO reviews and Administrator approves finalized Management Report (`UC-5.105`).
> 6. CMO views executive KPI dashboard and exports PDF/Excel (`UC-5.106`).
> 
> ```mermaid
> flowchart TD
>     AuthFromScope2[Scope 2 Authorizes Transport] --> Assign["UC-5.107: Assign Ambulance & Driver"]
>     Assign --> TripStatus["UC-5.108: Driver Logs Live Status (En Route -> Picked Up -> Done)"]
>     TripStatus --> TripLog["UC-5.109: Store Trip Log & Mileage"]
>     TripLog --> MonthlyConsol["UC-5.104: Consolidate Monthly Department Data"]
>     MonthlyConsol --> MgtReport["UC-5.105: Finalize Report (CMO Review + Admin Approval)"]
>     MgtReport --> Dash["UC-5.106: Executive Dashboard & PDF/Excel Export"]
> ```

> [!error]- 🔴 Sad Paths & Edge Cases: Dispatch Execution & Reports
> - **Sad Path 5.3.1 (No Available Ambulance During Execution Phase)**:
>   - *Trigger*: Assigned vehicle experiences mechanical breakdown right before departure.
>   - *Handling*: `UC-5.107` alerts coordinator and escalates back to Scope 2 to trigger Self-Transport Fallback (`UC-2.45`).
> - **Sad Path 5.3.2 (Driver Misses Real-Time Status Checkpoint)**:
>   - *Trigger*: Mobile connectivity lost while driving in remote area.
>   - *Handling*: `UC-5.108` allows driver to submit a retrospective status update with timestamp notes upon return.
> - **Sad Path 5.3.3 (Department Submits Discrepant Reporting Data)**:
>   - *Trigger*: Pharmacy numbers don't match inventory ledger totals.
>   - *Handling*: `UC-5.104` validation catches discrepancy, flags inconsistencies, and returns report to department officer for correction before executive report generation.

---
