# IUT Medical System - Use Case Lab Final Cheat Sheet

This guide is designed for rapid review using Obsidian's collapsible callouts. Because there are over 100 use cases, they are grouped by **Scope**. 

For the exam, remember the **Generic Use Case Pattern** which you can adapt for *any* random use case you are given.

> [!abstract]- 🧠 Generic Use Case Pattern (Memorize This!)
> If you get a random use case you don't fully remember, use this template:
> 
> **Use Case Description Template:**
> - **Use Case Name:** [Name of the given use case]
> - **Primary Actor:** [Patient / Doctor / Receptionist / Pharmacist / Admin / Accounts]
> - **Pre-condition:** User is logged into the system. [Add one specific condition, e.g., Patient profile exists].
> - **Main Flow:**
>   1. Actor navigates to the [Specific] module.
>   2. System displays the [Specific] form/dashboard.
>   3. Actor enters details and submits.
>   4. System validates the information.
>   5. System saves the record and shows a success message.
> - **Post-condition:** The [Record/Task] is successfully created/updated in the database.
> 
> **Use Case Diagram Template:**
> ```mermaid
> flowchart LR
>     Actor((Actor))
>     System[System Boundary]
>     
>     Actor --> UC(["[Name of Use Case]"])
>     UC -.->|<<includes>>| Login(["Log In"])
>     UC -.->|<<includes>>| Verify(["Verify Credentials"])
> ```

---

## Scope 1: Patient Registration, Appointment & Records

> [!info] Actors & Context
> **Actors:** Patient, Receptionist, Nurse, Doctor.
> **Key Focus:** Handling walk-ins, online appointments, vital signs, and prescriptions.

> [!example]- 🏥 Registration & Profiles (UC-1.01 to UC-1.09)
> **Core Flow:** Receptionist or System creates/updates a patient profile.
> **Key Use Cases:** Log In, Create Patient Profile, Register Walk-In, Verify Eligibility.
> 
> ```mermaid
> flowchart LR
>     R((Receptionist))
>     P((Patient))
>     
>     R --> CreateProfile(["Create Patient Profile"])
>     R --> RegWalkIn(["Register Walk-In Patient"])
>     P --> RegWalkIn
>     RegWalkIn -.->|<<includes>>| Verify(["Verify Patient Eligibility"])
> ```

> [!example]- 📅 Appointments & Vitals (UC-1.10 to UC-1.24)
> **Core Flow:** Patient books appointment -> Nurse records vitals -> Patient put in queue.
> **Key Use Cases:** Book Appointment, Record Vital Signs, Update Patient Queue Status.
> 
> ```mermaid
> flowchart LR
>     P((Patient))
>     N((Nurse))
>     
>     P --> BookAppt(["Book Appointment"])
>     N --> RecVitals(["Record Vital Signs"])
>     N --> Q(["Update Patient Queue Status"])
>     RecVitals -.->|<<extends>>| Notify(["Notify Nurse of Missing Vitals"])
> ```

> [!example]- 💊 Consultation & Prescription (UC-1.25 to UC-1.31)
> **Core Flow:** Doctor reviews vitals, checks allergies, writes prescription, and publishes it.
> **Key Use Cases:** Create Prescription, Check Allergy Conflict, Publish Prescription, View Medical Record.
> 
> ```mermaid
> flowchart LR
>     D((Doctor))
>     
>     D --> CreateRx(["Create Prescription"])
>     CreateRx -.->|<<includes>>| CheckAllergy(["Check Allergy Conflict"])
>     D --> PubRx(["Publish Prescription"])
>     D --> ViewMed(["View Medical Record"])
> ```

---

## Scope 3: Pharmacy, Inventory & Laboratory

> [!info] Actors & Context
> **Actors:** Pharmacist, Lab Technician, Patient, CMO.
> **Key Focus:** Dispensing medicine, managing stock/equipment, ordering and processing lab tests.

> [!example]- 💊 Pharmacy & Dispensing (UC-3.54 to UC-3.57)
> **Core Flow:** Pharmacist searches for medicine, verifies patient, and dispenses.
> **Key Use Cases:** Patient Identification, Search Medicine, Dispense Medicine, Handle Wrong Dispensing.
> 
> ```mermaid
> flowchart LR
>     Ph((Pharmacist))
>     
>     Ph --> ID(["Patient Identification"])
>     Ph --> Dispense(["Dispense Medicine"])
>     Dispense -.->|<<includes>>| Search(["Search Medicine"])
>     Ph --> Wrong(["Handle Wrong Dispensing"])
> ```

> [!example]- 📦 Inventory & Procurement (UC-3.58 to UC-3.65)
> **Core Flow:** System alerts low stock -> Restock requested -> Medicine registered on shelf.
> **Key Use Cases:** Generate Low-Stock Alert, Emergency Medicine Procurement, Asset Maintenance.
> 
> ```mermaid
> flowchart LR
>     System((System))
>     Ph((Pharmacist))
>     CMO((CMO))
>     
>     System --> Alert(["Generate Low-Stock Alert"])
>     Ph --> Restock(["Low-Stock Restock Request"])
>     CMO --> Procure(["Emergency Medicine Procurement"])
> ```

> [!example]- 🔬 Laboratory Management (UC-3.66 to UC-3.70)
> **Core Flow:** Doctor requests test -> Lab Tech collects sample -> Results uploaded and verified.
> **Key Use Cases:** Request Laboratory Test, Sample Collection & Token Labeling, Upload & Verify Test Result.
> 
> ```mermaid
> flowchart LR
>     D((Doctor))
>     LT((Lab Tech))
>     
>     D --> ReqTest(["Request Laboratory Test"])
>     LT --> Collect(["Sample Collection<br>& Token Labeling"])
>     LT --> Upload(["Upload & Verify<br>Test Result"])
> ```

---

## Scope 4: Financial, Billing & Reimbursement

> [!info] Actors & Context
> **Actors:** Applicant (Patient/Staff), Medical Center Admin, Two-Doctor Panel, Accounts/Finance Officer.
> **Key Focus:** Applying for external medical expense reimbursement and getting it approved and paid.

> [!example]- 📝 Application Submission (UC-4.71 to UC-4.76)
> **Core Flow:** Applicant prepares draft, attaches bills, and submits.
> **Key Use Cases:** Prepare Reimbursement Request, Attach Documents, Submit Request.
> 
> ```mermaid
> flowchart LR
>     App((Applicant))
>     
>     App --> Prep(["Prepare Reimbursement Request"])
>     Prep -.->|<<includes>>| Attach(["Attach Supporting Documents"])
>     App --> Submit(["Submit Reimbursement Request"])
> ```

> [!example]- ⚖️ Verification & Sign-off (UC-4.77 to UC-4.82)
> **Core Flow:** Admin checks eligibility -> Two-Doctor panel verifies medical necessity -> Sent to Accounts.
> **Key Use Cases:** Check Applicant Eligibility, Perform Two-Doctor Verification and Sign-off.
> 
> ```mermaid
> flowchart LR
>     Admin((MC Admin))
>     Docs((Two-Doctor Panel))
>     
>     Admin --> Check(["Check Applicant Eligibility"])
>     Docs --> Verify(["Perform Two-Doctor Verification"])
>     Verify -.->|<<includes>>| Forward(["Forward to Accounts"])
> ```

> [!example]- 💰 Finance & Payment (UC-4.83 to UC-4.90)
> **Core Flow:** Finance calculates payable amount, applies deductions, and generates voucher.
> **Key Use Cases:** Perform Finance Re-check, Calculate Payable Amount, Generate Payment Voucher.
> 
> ```mermaid
> flowchart LR
>     Fin((Accounts / Finance))
>     
>     Fin --> Recheck(["Perform Finance Re-check"])
>     Fin --> Calc(["Calculate Payable Amount"])
>     Fin --> Voucher(["Generate Payment Voucher"])
> ```

---

## Scope 5: Administrative System & Reporting

> [!info] Actors & Context
> **Actors:** Administrator, Receptionist, Ambulance Driver.
> **Key Focus:** Duty rosters, driver shifts, system notifications, and reporting dashboards.

> [!example]- 🗓️ Duty Schedules & Shifts (UC-5.91 to UC-5.94, UC-5.98 to UC-5.100)
> **Core Flow:** Admin approves and distributes duty schedules for staff and ambulance drivers.
> **Key Use Cases:** Process duty schedule approvals, Coordinate staff shift changes.
> 
> ```mermaid
> flowchart LR
>     Admin((Administrator))
>     
>     Admin --> ApprDuty(["Process duty schedule approvals"])
>     Admin --> Distribute(["Distribute the duty schedule"])
>     Admin --> ShiftChange(["Coordinate shift changes"])
> ```

> [!example]- 🔔 Notifications & Reports (UC-5.95 to UC-5.97, UC-5.104 to UC-5.106)
> **Core Flow:** System generates notifications -> Admin generates management reports.
> **Key Use Cases:** Dispatch system notifications, Process department report data, Access report dashboards.
> 
> ```mermaid
> flowchart LR
>     System((System))
>     Admin((Administrator))
>     
>     System --> Notify(["Dispatch system notifications"])
>     Admin --> Report(["Process department report data"])
>     Admin --> Dashboard(["Access report dashboards"])
> ```

> [!example]- 🚑 Ambulance Tracking (UC-5.107 to UC-5.109)
> **Core Flow:** Admin dispatches ambulance, tracks live updates, and reviews history.
> **Key Use Cases:** Handle ambulance dispatching, Track live trip updates.
> 
> ```mermaid
> flowchart LR
>     Admin((Administrator))
>     
>     Admin --> Dispatch(["Handle ambulance dispatching"])
>     Admin --> Track(["Track live trip updates"])
> ```
