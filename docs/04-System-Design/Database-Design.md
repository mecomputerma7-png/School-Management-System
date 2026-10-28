# School Management System
## Database Design — Final Logical Baseline

| Document control | Value |
|---|---|
| Version | 2.0 |
| Date | October 2026 |
| Status | **Approved logical decisions; implementation constraints pending** |
| Application | Desktop-only, single private school |
| Storage | Centralized relational database on the school LAN |
| DBMS | Not yet selected |

> **Scope of finality:** This is the final **logical design baseline** incorporating the four approved review decisions. It is not a claim that a particular DBMS schema, SQL migrations, or unresolved school policies have been approved.

## 1. Purpose and Scope

This document specifies the entities, attributes, cardinalities, business invariants, and transaction boundaries for the School Management System (SMS). It supports seven modules: Student, Teacher, Class, Grade, Fee, User, and Reports Management. Reporting reads from the operational data model; it does not require a separate operational entity. Audit records form a cross-cutting domain. No attendance, timetable, standalone examination, web/mobile portal, or multi-school functionality is included.

## 2. Architectural Assumptions

- The desktop client contains presentation, business-logic, and data-access layers; clients connect to one centrally managed relational database over the school LAN.
- Each academic year contains configurable academic terms; grade scales, component weights, and approval permissions remain configurable policy decisions.
- Financial amounts use exact decimal arithmetic; currency, rounding, and fee policies require confirmation before physical implementation.
- Historical academic and financial events are preserved rather than silently overwritten.
- All protected operations require authenticated users and RBAC authorization.

## 3. Entity Inventory

| Domain | Entities | Count |
|---|---|---:|
| Student | Student, Enrollment, SectionAssignment | 3 |
| Teacher | Teacher, TeachingAssignment | 2 |
| Academic structure | AcademicYear, AcademicTerm, GradeLevel, ClassSection, Subject, SectionSubject | 6 |
| Grade | AssessmentComponent, StudentGrade, GradeFinalization | 3 |
| Fee | FeeStructure, StudentCharge, Payment, PaymentAllocation, FinancialAdjustment | 5 |
| Access | UserAccount, Role, Permission, UserRole, RolePermission | 5 |
| Audit | AuditLog | 1 |
| **Total** | | **25** |

## 4. Logical Entity Dictionary

**Notation:** `PK` primary key; `FK → X` references X; `UQ` unique; `FK?` nullable foreign key. Fields not marked nullable are assumed required unless a status-specific rule or policy says otherwise. All identifiers are surrogate keys except composite PKs for the RBAC junction tables. Physical data types are deferred to the DBMS selection.

### 4.1 Student Management

#### `Student`

| Attribute | Constraint / reference |
|---|---|
| `StudentID` | PK |
| `StudentNumber` | UQ |
| `FirstName` | — |
| `LastName` | — |
| `DateOfBirth` | — |
| `Gender` | — |
| `ContactInformation` | — |
| `StudentStatus` | — |
| `CreatedAt` | — |

#### `Enrollment`

| Attribute | Constraint / reference |
|---|---|
| `EnrollmentID` | PK |
| `StudentID` | FK → `Student` |
| `AcademicYearID` | FK → `AcademicYear` |
| `EnrollmentDate` | — |
| `EnrollmentStatus` | — |

#### `SectionAssignment`

| Attribute | Constraint / reference |
|---|---|
| `SectionAssignmentID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `ClassSectionID` | FK → `ClassSection` |
| `EffectiveFrom` | — |
| `EffectiveTo` | — |

### 4.2 Teacher Management

#### `Teacher`

| Attribute | Constraint / reference |
|---|---|
| `TeacherID` | PK |
| `TeacherNumber` | UQ |
| `FirstName` | — |
| `LastName` | — |
| `ContactInformation` | — |
| `TeacherStatus` | — |
| `CreatedAt` | — |

#### `TeachingAssignment`

| Attribute | Constraint / reference |
|---|---|
| `AssignmentID` | PK |
| `TeacherID` | FK → `Teacher` |
| `SectionSubjectID` | FK → `SectionSubject` |
| `AssignmentStatus` | — |

### 4.3 Academic Structure

#### `AcademicYear`

| Attribute | Constraint / reference |
|---|---|
| `AcademicYearID` | PK |
| `YearName` | UQ |
| `StartDate` | — |
| `EndDate` | — |
| `YearStatus` | — |

#### `AcademicTerm`

| Attribute | Constraint / reference |
|---|---|
| `TermID` | PK |
| `AcademicYearID` | FK → `AcademicYear` |
| `TermName` | — |
| `StartDate` | — |
| `EndDate` | — |
| `TermStatus` | — |

#### `GradeLevel`

| Attribute | Constraint / reference |
|---|---|
| `GradeLevelID` | PK |
| `GradeName` | UQ |
| `GradeOrder` | — |

#### `ClassSection`

| Attribute | Constraint / reference |
|---|---|
| `ClassSectionID` | PK |
| `AcademicYearID` | FK → `AcademicYear` |
| `GradeLevelID` | FK → `GradeLevel` |
| `SectionName` | — |
| `SectionStatus` | — |

#### `Subject`

| Attribute | Constraint / reference |
|---|---|
| `SubjectID` | PK |
| `SubjectCode` | UQ |
| `SubjectName` | — |
| `SubjectStatus` | — |

#### `SectionSubject`

| Attribute | Constraint / reference |
|---|---|
| `SectionSubjectID` | PK |
| `ClassSectionID` | FK → `ClassSection` |
| `SubjectID` | FK → `Subject` |
| `SectionSubjectStatus` | — |

### 4.4 Grade Management

#### `AssessmentComponent`

| Attribute | Constraint / reference |
|---|---|
| `AssessmentComponentID` | PK |
| `SectionSubjectID` | FK → `SectionSubject` |
| `TermID` | FK → `AcademicTerm` |
| `ComponentName` | — |
| `MaximumScore` | — |
| `Weight` | — |
| `AssessmentDate` | — |

#### `StudentGrade`

| Attribute | Constraint / reference |
|---|---|
| `StudentGradeID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `SectionAssignmentID` | FK → `SectionAssignment` |
| `AssessmentComponentID` | FK → `AssessmentComponent` |
| `Score` | — |
| `GradeStatus` | — |
| `RecordedBy` | FK → `UserAccount` |
| `RecordedAt` | — |

#### `GradeFinalization`

| Attribute | Constraint / reference |
|---|---|
| `FinalizationID` | PK |
| `SectionSubjectID` | FK → `SectionSubject` |
| `TermID` | FK → `AcademicTerm` |
| `FinalizedBy` | FK → `UserAccount` |
| `FinalizedAt` | — |
| `FinalizationStatus` | — |

### 4.5 Fee Management

#### `FeeStructure`

| Attribute | Constraint / reference |
|---|---|
| `FeeStructureID` | PK |
| `AcademicYearID` | FK → `AcademicYear` |
| `GradeLevelID` | FK → `GradeLevel` |
| `FeeName` | — |
| `FeeAmount` | — |
| `FeeStatus` | — |

#### `StudentCharge`

| Attribute | Constraint / reference |
|---|---|
| `ChargeID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `FeeStructureID` | FK → `FeeStructure` |
| `ChargeAmount` | — |
| `ChargeDate` | — |
| `ChargeStatus` | — |

#### `Payment`

| Attribute | Constraint / reference |
|---|---|
| `PaymentID` | PK |
| `PaymentNumber` | UQ |
| `EnrollmentID` | FK → `Enrollment` |
| `PaymentAmount` | — |
| `PaymentDate` | — |
| `PaymentStatus` | — |
| `RecordedBy` | FK → `UserAccount` |
| `RecordedAt` | — |

#### `PaymentAllocation`

| Attribute | Constraint / reference |
|---|---|
| `AllocationID` | PK |
| `PaymentID` | FK → `Payment` |
| `ChargeID` | FK → `StudentCharge` |
| `AllocatedAmount` | — |
| `AllocatedAt` | — |

#### `FinancialAdjustment`

| Attribute | Constraint / reference |
|---|---|
| `AdjustmentID` | PK |
| `EnrollmentID` | FK → `Enrollment` |
| `PaymentID` | FK? → `Payment` |
| `ChargeID` | FK? → `StudentCharge` |
| `AdjustmentType` | — |
| `AdjustmentAmount` | — |
| `AdjustmentReason` | — |
| `ApprovedBy` | FK → `UserAccount` |
| `CreatedAt` | — |

### 4.6 User Management

#### `UserAccount`

| Attribute | Constraint / reference |
|---|---|
| `UserID` | PK |
| `Username` | UQ |
| `PasswordHash` | — |
| `AccountStatus` | — |
| `CreatedAt` | — |

#### `Role`

| Attribute | Constraint / reference |
|---|---|
| `RoleID` | PK |
| `RoleName` | UQ |
| `RoleDescription` | — |

#### `Permission`

| Attribute | Constraint / reference |
|---|---|
| `PermissionID` | PK |
| `PermissionCode` | UQ |
| `PermissionDescription` | — |

#### `UserRole`

| Attribute | Constraint / reference |
|---|---|
| `UserID` | PK/FK → `UserAccount` |
| `RoleID` | PK/FK → `Role` |

#### `RolePermission`

| Attribute | Constraint / reference |
|---|---|
| `RoleID` | PK/FK → `Role` |
| `PermissionID` | PK/FK → `Permission` |

### 4.7 Audit

#### `AuditLog`

| Attribute | Constraint / reference |
|---|---|
| `AuditID` | PK |
| `UserID` | FK? → `UserAccount` |
| `ActionType` | — |
| `EntityName` | — |
| `RecordIdentifier` | — |
| `OldValue` | — |
| `NewValue` | — |
| `EventTimestamp` | — |

## 5. Relationships and Cardinality

Each relationship below is **one-to-many (1:N)** from the referenced parent to the referencing child. A foreign key expresses structural existence; cross-row and temporal eligibility rules are additionally enforced as specified in §6.

| Parent (1) | Child (N) | Child foreign key |
|---|---|---|
| `Student` | `Enrollment` | `StudentID` |
| `AcademicYear` | `Enrollment` | `AcademicYearID` |
| `Enrollment` | `SectionAssignment` | `EnrollmentID` |
| `ClassSection` | `SectionAssignment` | `ClassSectionID` |
| `Teacher` | `TeachingAssignment` | `TeacherID` |
| `SectionSubject` | `TeachingAssignment` | `SectionSubjectID` |
| `AcademicYear` | `AcademicTerm` | `AcademicYearID` |
| `AcademicYear` | `ClassSection` | `AcademicYearID` |
| `GradeLevel` | `ClassSection` | `GradeLevelID` |
| `ClassSection` | `SectionSubject` | `ClassSectionID` |
| `Subject` | `SectionSubject` | `SubjectID` |
| `SectionSubject` | `AssessmentComponent` | `SectionSubjectID` |
| `AcademicTerm` | `AssessmentComponent` | `TermID` |
| `Enrollment` | `StudentGrade` | `EnrollmentID` |
| `SectionAssignment` | `StudentGrade` | `SectionAssignmentID` |
| `AssessmentComponent` | `StudentGrade` | `AssessmentComponentID` |
| `UserAccount` | `StudentGrade` | `RecordedBy` |
| `SectionSubject` | `GradeFinalization` | `SectionSubjectID` |
| `AcademicTerm` | `GradeFinalization` | `TermID` |
| `UserAccount` | `GradeFinalization` | `FinalizedBy` |
| `AcademicYear` | `FeeStructure` | `AcademicYearID` |
| `GradeLevel` | `FeeStructure` | `GradeLevelID` |
| `Enrollment` | `StudentCharge` | `EnrollmentID` |
| `FeeStructure` | `StudentCharge` | `FeeStructureID` |
| `Enrollment` | `Payment` | `EnrollmentID` |
| `UserAccount` | `Payment` | `RecordedBy` |
| `Payment` | `PaymentAllocation` | `PaymentID` |
| `StudentCharge` | `PaymentAllocation` | `ChargeID` |
| `Enrollment` | `FinancialAdjustment` | `EnrollmentID` |
| `Payment` | `FinancialAdjustment` | `PaymentID (optional)` |
| `StudentCharge` | `FinancialAdjustment` | `ChargeID (optional)` |
| `UserAccount` | `FinancialAdjustment` | `ApprovedBy` |
| `UserAccount` | `UserRole` | `UserID` |
| `Role` | `UserRole` | `RoleID` |
| `Role` | `RolePermission` | `RoleID` |
| `Permission` | `RolePermission` | `PermissionID` |
| `UserAccount` | `AuditLog` | `UserID (optional)` |

**Many-to-many relationships:** `UserAccount ↔ Role` through `UserRole`; `Role ↔ Permission` through `RolePermission`; `ClassSection ↔ Subject` through `SectionSubject`; `Payment ↔ StudentCharge` through `PaymentAllocation`.

## 6. Business Invariants and Integrity Rules

### 6.1 Enrollment and Section History

- **ENR-01:** At most one **active** `Enrollment` per `(StudentID, AcademicYearID)`. Historical closed enrollment records may be retained as policy permits.
- **ENR-02:** `SectionAssignment` intervals are half-open: `[EffectiveFrom, EffectiveTo)`. `EffectiveTo = NULL` denotes an open-ended assignment. Intervals for the same enrollment must not overlap.
- **ENR-03:** An assigned `ClassSection.AcademicYearID` must equal the associated `Enrollment.AcademicYearID`.
- **ENR-04:** At most one current/open section assignment per enrollment. Transfer closes the prior interval and opens the next in one transaction; historical assignments remain intact.
- **ENR-05:** A section transfer must preserve grade-level eligibility according to school policy; a different grade level cannot be assigned merely by changing the section.

### 6.2 Academic Structure and Grading

- **GRD-01:** An `AcademicTerm` lies within its parent `AcademicYear`; term naming is unique per year. Whether term intervals may overlap is an institutional policy to confirm.
- **GRD-02:** `(ClassSectionID, SubjectID)` is unique in `SectionSubject`. The academic year of a section subject is inherited from its section.
- **GRD-03:** `AssessmentComponent.TermID` and its `SectionSubjectID` must resolve to the same academic year. `AssessmentDate` must be within the configured term, subject to school policy.
- **GRD-04:** `MaximumScore > 0`; `Weight >= 0`; each `StudentGrade.Score` must satisfy `0 <= Score <= MaximumScore`. Weight totals and formulas remain configurable.
- **GRD-05:** A student's `StudentGrade.SectionAssignmentID` must reference an assignment belonging to the same `EnrollmentID`, and that assignment's section must equal the assessment's `SectionSubject.ClassSectionID`. The assignment must be effective on the component's `AssessmentDate`. This preserves historical eligibility after transfers.
- **GRD-06:** A unique grade result per `(EnrollmentID, AssessmentComponentID)` is required unless a separately approved retake/versioning policy is introduced.
- **GRD-07:** The recording user must have the grade-entry permission and a valid teaching assignment for the `SectionSubject`, or an explicitly approved override permission.
- **GRD-08:** At most one current finalization record per `(SectionSubjectID, TermID)`; finalization and any reopening require an authorized workflow and audit trail. A finalized result is not silently edited.

### 6.3 Fees, Payments, and Corrections

- **FIN-01:** `FeeStructure.FeeAmount >= 0`, `StudentCharge.ChargeAmount >= 0`, `Payment.PaymentAmount > 0`, and `PaymentAllocation.AllocatedAmount > 0`.
- **FIN-02:** A `StudentCharge` must use a fee structure from the enrollment's academic year and the grade level applicable to that enrollment on the charge date.
- **FIN-03:** Each payment and all its allocations must belong to the **same enrollment**.
- **FIN-04:** For each **posted** payment, `SUM(PaymentAllocation.AllocatedAmount) = Payment.PaymentAmount`. This cross-row invariant is enforced at transaction commit, not by a single-row CHECK constraint.
- **FIN-05:** The sum of effective allocations against a charge must not exceed its effective payable amount. The system rejects a payment exceeding the student's total outstanding balance; **no unallocated credit or overpayment is supported**.
- **FIN-06:** A new payment, its allocations, and any resulting balance effects must be committed atomically, with concurrency control over the affected charges.
- **FIN-07:** Original payment and allocation amounts are immutable after posting. Corrections are documented by `FinancialAdjustment` and an approved reversal/void workflow. A voided payment remains stored with its original allocations for evidence, but those allocations are excluded from effective paid totals.
- **FIN-08:** `FinancialAdjustment.AdjustmentType` distinguishes `PAYMENT_VOID` and `CHARGE_ADJUSTMENT`. A payment void must reference `PaymentID`; a charge adjustment must reference `ChargeID`. The associated enrollment must match the referenced record. An adjustment has an approver, reason, and timestamp.
- **FIN-09:** Charge adjustments cannot reduce effective charges below the amount already paid by effective allocations; a payment reversal and reallocation must occur first where required.
- **FIN-10:** Outstanding balances are **derived** from effective charges/adjustments minus effective allocations of posted payments; do not store an independently editable balance field.

### 6.4 Identity, Auditing, and Lifecycle

- **SEC-01:** Usernames, role names, permission codes, and institutional numbers are unique; passwords are stored only as secure salted password hashes using a suitable password-hashing algorithm.
- **SEC-02:** `UserRole` and `RolePermission` have composite primary keys; effective privileges are checked in the business-logic layer for every protected operation.
- **SEC-03:** Sensitive changes and approvals are written to `AuditLog` with actor, action, target, and timestamp; audit records must be append-protected. A nullable actor is permitted only for documented system-originated events.
- **SEC-04:** Historical academic, financial, and audit records must not be permanently deleted through ordinary application workflows.

## 7. Validation and Transaction Boundaries

| Operation | Required atomic work | Concurrency/integrity protection |
|---|---|---|
| Student transfer | Close previous assignment; insert next assignment; write audit | Prevent overlapping intervals and cross-year sections |
| Grade entry | Check assignment/date, teacher permission, score range; write grade and audit | Unique result and valid historical section assignment |
| Grade finalization | Validate eligible results; create/update finalization state; audit | Lock affected grading scope; prevent unauthorized later edits |
| Payment posting | Validate outstanding balances; insert payment and allocations; audit | Lock affected charges/enrollment; prevent overpayment and duplicate allocations |
| Payment correction | Approve and record adjustment; void original payment as appropriate; audit | Original amounts immutable; recompute effective allocations consistently |
| Charge adjustment | Validate approval and paid-to-date limit; record adjustment; audit | Prevent negative effective outstanding balance |

**Enforcement model:** use database `PK`, `FK`, `UNIQUE`, `NOT NULL`, and `CHECK` constraints where feasible; use transactional business logic for cross-table/temporal conditions; add DBMS-specific partial indexes, exclusion constraints, triggers, or locking after DBMS selection. Application-side validation alone is insufficient against concurrent writes.

## 8. Normalization and Data Types

The logical design targets 3NF: each table has atomic attributes, each non-key attribute depends on its full key, and repeating/many-to-many facts are separated into junction tables. Validate the normal form during physical design, particularly `ContactInformation` (split into typed contact rows if multiple contacts are required) and `OldValue`/`NewValue` audit payloads (structured snapshots, not operational relational facts).

Use DBMS-native date/timestamp types, exact decimal amounts and scores, bounded identifiers, enumerated/status-domain checks, and indexes on foreign keys and common `(AcademicYearID, ClassSectionID, TermID)` filters as appropriate. Avoid floating-point storage for money.

## 9. Security, Backup, and Recovery

Enforce least-privilege DB credentials, secure secret storage, encrypted client-to-database connections where supported, RBAC checks, protected audit logs, scheduled backups, and tested restoration procedures. Backup frequency, retention, and recovery objectives must be approved by the school.

## 10. ERD Deliverables

- [Complete ERD (PNG)](diagrams/ERD.png) — full-resolution visual.
- [Complete ERD (SVG)](diagrams/ERD.svg) — scalable visual.
- [Editable draw.io ERD](diagrams/ERD.drawio) — editable entities and connectors.
- [Graphviz ERD source](diagrams/ERD.dot) — reproducible layout source.
- Domain views: [Students](diagrams/ERD-Students.png), [Academic](diagrams/ERD-Academic.png), [Grades](diagrams/ERD-Grades.png), [Finance](diagrams/ERD-Finance.png), [Access](diagrams/ERD-Access.png).

The diagram contains **25 entities and 37 explicit FK relationships**. It represents **structural relationships**; cross-table and temporal business rules are specified in §6 and are not fully expressible by ordinary ERD connectors.

## 11. Approved Decisions

| ID | Decision | Status |
|---|---|---|
| DD-01 | Separate `Enrollment` from historical `SectionAssignment` | Approved |
| DD-02 | Add `AcademicTerm` and `SectionSubject` | Approved |
| DD-03 | Keep assessment components and weights configurable | Approved |
| DD-04 | Add `PaymentAllocation` for charge-level allocation | Approved |
| DD-05 | Reject payments exceeding outstanding balances | Approved |
| DD-06 | Preserve originals and record financial corrections | Approved |
| DD-07 | Apply DB constraints, business rules, and atomic concurrency-safe transactions | Approved |

## 12. Implementation Decisions Still Open

| ID | Decision needed |
|---|---|
| PD-01 | DBMS, access driver/ORM, and DBMS-specific constraints |
| PD-02 | Grade scales, weight-total rules, term policies, and finalization authority |
| PD-03 | Detailed authorization and reconciliation steps for financial reversal |
| PD-04 | Currency, decimal precision, rounding, retention, and backup policy |
| PD-05 | Final RBAC permission matrix, including administrative overrides |
| PD-06 | Whether student contact details need separate normalized contact records |

## 13. Related Documents

| Document | Relationship |
|---|---|
| [SRS](../02-Requirements-Analysis/SRS.md) | System requirements and scope |
| [Functional Requirements](../02-Requirements-Analysis/Functional-Requirements.md) | Functional requirements |
| [Non-Functional Requirements](../02-Requirements-Analysis/Non-Functional-Requirements.md) | Security, reliability, and quality constraints |
| [Business Rules](../03-System-Analysis/Business-Rules.md) | Institutional rules |
| [Process Analysis](../03-System-Analysis/Process-Analysis.md) | Operational processes and data flows |
| [System Architecture](System-Architecture.md) | Desktop architecture and central DB deployment |
| [Interface Design](Interface-Design.md) | Future desktop UI design |

## 14. Conclusion

This final logical baseline defines **25 entities** and **37 structural foreign-key relationships** supporting the approved SMS scope. Historical section assignments, term-specific grading, charge-level payment allocation, strict overpayment rejection, and multi-layer integrity controls are explicitly incorporated. Physical SQL implementation and outstanding institutional policies remain separate approval steps.

---
**End of Document**
