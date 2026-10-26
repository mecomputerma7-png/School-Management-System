# School Management System
## Use Case Descriptions

**Document Version:** 1.0  
**Date:** October 2026  
**Status:** Draft

---

## 1. Purpose

This document describes the primary interactions between authorized users and the School Management System (SMS).

Each use case defines an operational goal, participating actor, preconditions, main success scenario, alternative flows, and expected outcome.

The descriptions complement the functional requirements without repeating their specifications.

## 2. Actors

| Actor | Description |
|---|---|
| School Principal | Reviews institutional information and reports. |
| System Administrator | Manages system users and access permissions. |
| Administrative Staff | Manages student, teacher, and class information. |
| Teacher | Records and reviews authorized student grades. |
| Accountant | Manages tuition fees and payment transactions. |

All actors must be authenticated and possess the permissions required for the requested operation.

## 3. Use Case Overview

| Use Case ID | Use Case | Primary Actor | Related Requirements |
|---|---|---|---|
| UC-01 | Authenticate User | All Users | FR-045, FR-052 |
| UC-02 | Register Student | Administrative Staff | FR-001–FR-003 |
| UC-03 | Update Student Information | Administrative Staff | FR-004–FR-007 |
| UC-04 | Register Teacher | Administrative Staff | FR-009–FR-010 |
| UC-05 | Assign Teacher | Administrative Staff | FR-013–FR-015 |
| UC-06 | Manage Class Sections | Administrative Staff | FR-016–FR-023 |
| UC-07 | Record Student Grades | Teacher | FR-026–FR-028 |
| UC-08 | Finalize Grades | Authorized Academic Staff | FR-029–FR-031 |
| UC-09 | Configure Tuition Fees | Accountant | FR-033–FR-035 |
| UC-10 | Record Student Payment | Accountant | FR-036–FR-038, FR-040–FR-041 |
| UC-11 | Correct Payment Transaction | Accountant | FR-043 |
| UC-12 | Manage User Accounts | System Administrator | FR-044, FR-047–FR-048 |
| UC-13 | Generate Reports | Authorized Users | FR-054–FR-062 |

## 4. Detailed Use Case Descriptions

### UC-01 — Authenticate User

**Primary Actor:** All Users  
**Preconditions:** The user has an active account.

**Main Success Scenario:**

1. The user opens the login interface.
2. The user enters valid credentials.
3. The system verifies the credentials and account status.
4. The system establishes an authenticated session.
5. The system displays the authorized user interface.

**Alternative Flows:**

- **A1 — Invalid Credentials:** The system rejects authentication and displays an appropriate error message.
- **A2 — Inactive Account:** The system denies access and informs the user that the account is unavailable.

**Postcondition:** The user gains access only to permitted system functions.

---

### UC-02 — Register Student

**Primary Actor:** Administrative Staff  
**Preconditions:** The actor is authenticated and authorized to register students.

**Main Success Scenario:**

1. The actor opens the student registration form.
2. The actor enters the required student and enrollment information.
3. The system validates the submitted information.
4. The system checks for potential duplicate records.
5. The actor confirms the registration.
6. The system creates the student record and assigns a unique identifier.
7. The system displays a registration confirmation.

**Alternative Flows:**

- **A1 — Missing or Invalid Data:** The system identifies the invalid fields and requests correction.
- **A2 — Duplicate Student:** The system rejects the registration according to the defined duplicate-detection rules.

**Postcondition:** A valid student record is stored successfully.

---

### UC-03 — Update Student Information

**Primary Actor:** Administrative Staff  
**Preconditions:** The student record exists and the actor has update permission.

**Main Success Scenario:**

1. The actor searches for and selects a student record.
2. The system displays the current student information.
3. The actor modifies the permitted fields.
4. The system validates the changes.
5. The actor confirms the update.
6. The system saves the changes and displays confirmation.

**Alternative Flows:**

- **A1 — Invalid Changes:** The system rejects the update and identifies the errors.
- **A2 — Restricted Field:** The system prevents modification of information outside the actor's permissions.

**Postcondition:** The student record is updated without losing required historical information.

---

### UC-04 — Register Teacher

**Primary Actor:** Administrative Staff  
**Preconditions:** The actor is authorized to manage teacher records.

**Main Success Scenario:**

1. The actor opens the teacher registration form.
2. The actor enters the required professional and contact information.
3. The system validates the submitted information.
4. The actor confirms registration.
5. The system creates the teacher record and assigns a unique identifier.
6. The system displays confirmation.

**Alternative Flow:**

- **A1 — Invalid Data:** The system requests correction before registration.

**Postcondition:** A new teacher record is available for authorized management.

---

### UC-05 — Assign Teacher

**Primary Actor:** Administrative Staff  
**Preconditions:** The teacher is active, and the target class section and subject exist.

**Main Success Scenario:**

1. The actor selects an active teacher.
2. The actor selects the academic year, class section, and subject.
3. The system validates the proposed assignment.
4. The actor confirms the assignment.
5. The system records the teaching assignment.

**Alternative Flows:**

- **A1 — Inactive Teacher:** The system rejects the assignment.
- **A2 — Invalid or Conflicting Assignment:** The system prevents the operation and explains the conflict.

**Postcondition:** The teaching assignment is recorded and available to authorized users.

---

### UC-06 — Manage Class Sections

**Primary Actor:** Administrative Staff  
**Preconditions:** The academic year and applicable grade level have been defined.

**Main Success Scenario:**

1. The actor opens class management.
2. The actor creates or selects a class section.
3. The actor enters or updates the section information.
4. The actor assigns eligible students to the section.
5. The system validates the section and student assignments.
6. The actor confirms the changes.
7. The system saves the information and updates the class roster.

**Alternative Flows:**

- **A1 — Duplicate Section Identifier:** The system rejects the conflicting section definition.
- **A2 — Multiple Active Assignments:** The system prevents a student from having multiple active sections in the same academic year.
- **A3 — Student Transfer:** The system closes the previous active assignment and records the new assignment while preserving history.

**Postcondition:** Class section information and student assignments remain consistent.

---

### UC-07 — Record Student Grades

**Primary Actor:** Teacher  
**Preconditions:** The teacher has an authorized teaching assignment and the grading period is open.

**Main Success Scenario:**

1. The teacher selects an assigned class section and subject.
2. The system displays the eligible student list and grading components.
3. The teacher enters or updates student grades.
4. The system validates the entered values.
5. The teacher submits the grades.
6. The system saves the grades and displays confirmation.

**Alternative Flows:**

- **A1 — Invalid Grade:** The system rejects values outside the configured range.
- **A2 — Unauthorized Assignment:** The system denies access to the requested class or subject.
- **A3 — Finalized Period:** The system prevents grade modification.

**Postcondition:** Valid grades are stored for the selected students and grading period.

---

### UC-08 — Finalize Grades

**Primary Actor:** Authorized Academic Staff  
**Preconditions:** The grading period exists and the actor has approval permission.

**Main Success Scenario:**

1. The actor selects the grading period and applicable class or subject.
2. The system calculates results according to the configured grading rules.
3. The system displays the results for review.
4. The actor confirms finalization.
5. The system marks the applicable grades as finalized.
6. The system restricts further unauthorized modifications.

**Alternative Flows:**

- **A1 — Incomplete Grades:** The system identifies missing required assessment information and prevents finalization.
- **A2 — Reopening Required:** An authorized actor initiates the approved reopening process, records the reason, and obtains the required approval.

**Postcondition:** Finalized results are protected against unauthorized changes.

---

### UC-09 — Configure Tuition Fees

**Primary Actor:** Accountant  
**Preconditions:** The academic year and applicable grade levels exist.

**Main Success Scenario:**

1. The accountant opens fee configuration.
2. The accountant selects the academic year and grade level.
3. The accountant defines the applicable tuition amount.
4. The system validates the fee configuration.
5. The accountant confirms the configuration.
6. The system saves the fee structure for authorized use.

**Alternative Flow:**

- **A1 — Invalid Fee Configuration:** The system rejects invalid values or conflicting configurations.

**Postcondition:** A valid tuition fee structure is available for student fee assessment.

---

### UC-10 — Record Student Payment

**Primary Actor:** Accountant  
**Preconditions:** The student has an existing fee account with an outstanding balance.

**Main Success Scenario:**

1. The accountant searches for the student's fee account.
2. The system displays the outstanding balance and payment history.
3. The accountant enters the payment amount and required transaction information.
4. The system validates the payment.
5. The accountant confirms the transaction.
6. The system records the payment and generates a unique transaction identifier.
7. The system updates the outstanding balance.
8. The system generates a payment receipt.

**Alternative Flows:**

- **A1 — Invalid Amount:** The system rejects zero, negative, or otherwise invalid payment values.
- **A2 — Excess Payment:** The system rejects an amount exceeding the outstanding balance unless an approved overpayment policy applies.
- **A3 — Transaction Failure:** The system cancels the incomplete operation without recording a partial payment.

**Postcondition:** The payment is recorded accurately, and the student balance is updated.

---

### UC-11 — Correct Payment Transaction

**Primary Actor:** Accountant  
**Preconditions:** The original transaction exists and the actor has correction permission.

**Main Success Scenario:**

1. The accountant locates the original payment.
2. The accountant initiates a correction or reversal.
3. The accountant provides the required justification.
4. The system validates the request and applicable permissions.
5. The authorized correction is confirmed.
6. The system records the corrective transaction.
7. The system recalculates the student's outstanding balance.

**Alternative Flow:**

- **A1 — Unauthorized Correction:** The system rejects the operation.

**Postcondition:** The financial records reflect the correction while preserving the original transaction history.

---

### UC-12 — Manage User Accounts

**Primary Actor:** System Administrator  
**Preconditions:** The actor is authenticated with administrative privileges.

**Main Success Scenario:**

1. The administrator opens user management.
2. The administrator creates or selects a user account.
3. The administrator enters or updates account information and authorized roles.
4. The system validates the requested changes.
5. The administrator confirms the operation.
6. The system saves the changes and records the relevant audit information.

**Alternative Flows:**

- **A1 — Invalid Account Data:** The system rejects the changes and requests correction.
- **A2 — Unauthorized Role Assignment:** The system prevents assignment of permissions beyond the administrator's authority.

**Postcondition:** User account information and permissions are updated securely.

---

### UC-13 — Generate Reports

**Primary Actor:** Authorized Users  
**Preconditions:** The actor has permission to access the selected report and its underlying data.

**Main Success Scenario:**

1. The actor opens the reporting interface.
2. The actor selects an available report type.
3. The actor specifies applicable filtering criteria.
4. The system retrieves authorized data.
5. The system generates and displays the report.
6. The actor prints or exports the report when the selected format is supported.

**Alternative Flows:**

- **A1 — No Matching Records:** The system displays an empty result with an appropriate message.
- **A2 — Unauthorized Report:** The system denies access to the report or restricted information.
- **A3 — Report Generation Failure:** The system displays an error without exposing sensitive technical information.

**Postcondition:** The actor receives a report containing only authorized information.

## 5. Use Case Relationships

The following relationships describe common interactions between use cases.

| Relationship | Description |
|---|---|
| UC-02 → UC-06 | A registered student may subsequently be assigned to a class section. |
| UC-04 → UC-05 | A registered teacher may subsequently receive teaching assignments. |
| UC-06 → UC-07 | Student class assignments determine eligibility for grade recording. |
| UC-07 → UC-08 | Recorded grades are reviewed before finalization. |
| UC-09 → UC-10 | Configured tuition fees provide the basis for payment processing. |
| UC-10 → UC-11 | A recorded payment may require an authorized correction. |
| UC-01 → Protected Use Cases | Authentication is required before accessing protected operations. |

These are operational dependencies and do not automatically imply UML `include` or `extend` relationships.

## 6. Business Rules and Exceptions

Detailed validation policies, approval conditions, calculation rules, and exception-handling constraints will be maintained in:

**`Business-Rules.md`**

Use case descriptions shall reference these rules where applicable to avoid duplicating business logic across documents.

## 7. Conclusion

This document defines **13 primary use cases** covering the essential operational interactions of the School Management System.

The scenarios establish a foundation for process analysis, interface design, and use case-based testing.

---

**End of Document**
