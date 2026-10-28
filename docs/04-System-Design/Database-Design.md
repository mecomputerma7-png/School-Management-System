# School Management System
## Database Design

**Document Version:** 1.0  
**Date:** October 2026  
**Status:** Revised Proposed Design  
**Database Type:** Relational Database  
**Application Type:** Desktop Application

---

## 1. Purpose

This document defines the revised logical database design for the School Management System (SMS).

It identifies the principal entities, attributes, relationships, and integrity constraints required to support the seven functional modules of the system.

The design incorporates the approved decisions concerning student section transfers, academic assessments, financial transactions, and multi-layer data validation.

## 2. Database Architecture

The SMS uses a centralized relational database accessed by authorized desktop applications through the school's Local Area Network (LAN).

The database design follows these principles:

- Centralized institutional data management.
- Referential integrity and controlled relationships.
- Preservation of historical academic and financial records.
- Role-based access control.
- Transactional consistency.
- Minimization of unnecessary data duplication.
- Logical normalization toward Third Normal Form (3NF).

The Database Management System (DBMS) remains to be selected.

## 3. Database Entity Overview

The revised design contains **25 entities**, organized into seven logical domains.

| Domain | Entities | Count |
|---|---|---:|
| Student Management | Student, Enrollment, SectionAssignment | 3 |
| Teacher Management | Teacher, TeachingAssignment | 2 |
| Class Management | AcademicYear, AcademicTerm, GradeLevel, ClassSection, Subject, SectionSubject | 6 |
| Grade Management | AssessmentComponent, StudentGrade, GradeFinalization | 3 |
| Fee Management | FeeStructure, StudentCharge, Payment, PaymentAllocation, FinancialAdjustment | 5 |
| User Management | UserAccount, Role, Permission, UserRole, RolePermission | 5 |
| System Auditing | AuditLog | 1 |
| **Total** | | **25** |

## 4. Entity Definitions

The following tables describe the proposed logical attributes. Data types and implementation-specific indexes will be defined after selecting the DBMS.

### 4.1 Student Management

#### Student

Stores student identity and institutional information.

| Attribute | Key | Description |
|---|---|---|
| StudentID | PK | Unique student identifier. |
| StudentNumber | UNIQUE | Institutional student number. |
| FirstName | — | Student's first name. |
| LastName | — | Student's last name. |
| DateOfBirth | — | Date of birth. |
| Gender | — | Recorded gender. |
| ContactInformation | — | Contact details. |
| StudentStatus | — | Institutional status. |
| CreatedAt | — | Creation timestamp. |

#### Enrollment

Represents a student's enrollment in an academic year.

| Attribute | Key | Description |
|---|---|---|
| EnrollmentID | PK | Unique enrollment identifier. |
| StudentID | FK | References Student. |
| AcademicYearID | FK | References AcademicYear. |
| EnrollmentDate | — | Enrollment date. |
| EnrollmentStatus | — | Enrollment status. |

A student may have historical enrollments across multiple academic years but only one active enrollment within the same academic year.

#### SectionAssignment

Preserves current and historical student section assignments.

| Attribute | Key | Description |
|---|---|---|
| SectionAssignmentID | PK | Unique assignment identifier. |
| EnrollmentID | FK | References Enrollment. |
| ClassSectionID | FK | References ClassSection. |
| EffectiveFrom | — | Assignment start date. |
| EffectiveTo | — | Assignment end date, if applicable. |

Section assignments must not overlap for the same enrollment.

The assigned section must belong to the same academic year as the associated enrollment.

### 4.2 Teacher Management

#### Teacher

| Attribute | Key | Description |
|---|---|---|
| TeacherID | PK | Unique teacher identifier. |
| TeacherNumber | UNIQUE | Institutional teacher number. |
| FirstName | — | First name. |
| LastName | — | Last name. |
| ContactInformation | — | Contact details. |
| TeacherStatus | — | Employment status. |
| CreatedAt | — | Creation timestamp. |

#### TeachingAssignment

Associates teachers with the subjects taught in specific sections.

| Attribute | Key | Description |
|---|---|---|
| AssignmentID | PK | Unique teaching assignment identifier. |
| TeacherID | FK | References Teacher. |
| SectionSubjectID | FK | References SectionSubject. |
| AssignmentStatus | — | Assignment status. |

The application must prevent unauthorized or conflicting teaching assignments according to approved institutional rules.

### 4.3 Class Management

#### AcademicYear

| Attribute | Key | Description |
|---|---|---|
| AcademicYearID | PK | Unique academic year identifier. |
| YearName | UNIQUE | Academic year label. |
| StartDate | — | Start date. |
| EndDate | — | End date. |
| YearStatus | — | Academic year status. |

#### AcademicTerm

Defines academic periods within an academic year.

| Attribute | Key | Description |
|---|---|---|
| TermID | PK | Unique term identifier. |
| AcademicYearID | FK | References AcademicYear. |
| TermName | — | Academic term name. |
| StartDate | — | Term start date. |
| EndDate | — | Term end date. |
| TermStatus | — | Term status. |

Term dates must fall within the associated academic year.

#### GradeLevel

| Attribute | Key | Description |
|---|---|---|
| GradeLevelID | PK | Unique grade level identifier. |
| GradeName | UNIQUE | Grade level name. |
| GradeOrder | — | Academic progression order. |

#### ClassSection

| Attribute | Key | Description |
|---|---|---|
| ClassSectionID | PK | Unique section identifier. |
| AcademicYearID | FK | References AcademicYear. |
| GradeLevelID | FK | References GradeLevel. |
| SectionName | — | Section designation. |
| SectionStatus | — | Section status. |

Section names must be unique within the same grade level and academic year.

#### Subject

| Attribute | Key | Description |
|---|---|---|
| SubjectID | PK | Unique subject identifier. |
| SubjectCode | UNIQUE | Institutional subject code. |
| SubjectName | — | Subject name. |
| SubjectStatus | — | Subject status. |

#### SectionSubject

Associates subjects with class sections.

| Attribute | Key | Description |
|---|---|---|
| SectionSubjectID | PK | Unique section-subject identifier. |
| ClassSectionID | FK | References ClassSection. |
| SubjectID | FK | References Subject. |
| SectionSubjectStatus | — | Association status. |

The same subject must not be duplicated within the same section.

### 4.4 Grade Management

#### AssessmentComponent

Defines configurable assessment components for a section subject and academic term.

| Attribute | Key | Description |
|---|---|---|
| AssessmentComponentID | PK | Unique assessment component identifier. |
| SectionSubjectID | FK | References SectionSubject. |
| TermID | FK | References AcademicTerm. |
| ComponentName | — | Assessment component name. |
| MaximumScore | — | Maximum permitted score. |
| Weight | — | Contribution to the calculated result. |

Assessment components must belong to a term within the academic year of their associated section.

Assessment weights and scoring rules remain configurable and subject to institutional approval.

#### StudentGrade

Stores individual student assessment results.

| Attribute | Key | Description |
|---|---|---|
| StudentGradeID | PK | Unique grade identifier. |
| EnrollmentID | FK | References Enrollment. |
| AssessmentComponentID | FK | References AssessmentComponent. |
| Score | — | Recorded score. |
| GradeStatus | — | Grade status. |
| RecordedBy | FK | References UserAccount. |
| RecordedAt | — | Recording timestamp. |

The system must validate that:

- The score is within the assessment's permitted range.
- The student belongs to the relevant academic year.
- The student's section assignment is valid for the assessment context.
- The recording user has the required permission and teaching assignment.
- Finalized grades cannot be modified without an approved procedure.

A unique constraint must prevent duplicate active results for the same enrollment and assessment component.

#### GradeFinalization

Records finalization decisions for section-subject results within an academic term.

| Attribute | Key | Description |
|---|---|---|
| FinalizationID | PK | Unique finalization identifier. |
| SectionSubjectID | FK | References SectionSubject. |
| TermID | FK | References AcademicTerm. |
| FinalizedBy | FK | References UserAccount. |
| FinalizedAt | — | Finalization timestamp. |
| FinalizationStatus | — | Finalization status. |

Finalization must be restricted to authorized users.

### 4.5 Fee Management

#### FeeStructure

Defines approved fees by academic year and grade level.

| Attribute | Key | Description |
|---|---|---|
| FeeStructureID | PK | Unique fee structure identifier. |
| AcademicYearID | FK | References AcademicYear. |
| GradeLevelID | FK | References GradeLevel. |
| FeeName | — | Fee designation. |
| FeeAmount | — | Approved fee amount. |
| FeeStatus | — | Fee status. |

#### StudentCharge

Stores fees assessed against individual student enrollments.

| Attribute | Key | Description |
|---|---|---|
| ChargeID | PK | Unique charge identifier. |
| EnrollmentID | FK | References Enrollment. |
| FeeStructureID | FK | References FeeStructure. |
| ChargeAmount | — | Assessed amount. |
| ChargeDate | — | Assessment date. |
| ChargeStatus | — | Charge status. |

The associated fee structure must match the enrollment's academic year and applicable grade level.

#### Payment

Stores accepted payment transactions.

| Attribute | Key | Description |
|---|---|---|
| PaymentID | PK | Unique payment identifier. |
| PaymentNumber | UNIQUE | Institutional transaction number. |
| EnrollmentID | FK | References Enrollment. |
| PaymentAmount | — | Accepted payment amount. |
| PaymentDate | — | Payment date. |
| PaymentStatus | — | Payment status. |
| RecordedBy | FK | References UserAccount. |
| RecordedAt | — | Recording timestamp. |

**Approved overpayment policy:** The system must reject payments exceeding the student's outstanding balance.

#### PaymentAllocation

Associates payment amounts with specific student charges.

| Attribute | Key | Description |
|---|---|---|
| AllocationID | PK | Unique allocation identifier. |
| PaymentID | FK | References Payment. |
| ChargeID | FK | References StudentCharge. |
| AllocatedAmount | — | Amount allocated to the charge. |
| AllocatedAt | — | Allocation timestamp. |

The system must enforce the following rules:

1. The sum of allocations must equal the accepted payment amount.
2. Allocations must not exceed the remaining balance of the associated charges.
3. The payment and associated charges must belong to the same enrollment.
4. Payment recording and allocation must be committed atomically.
5. Concurrent operations must not cause overpayment or duplicate allocation.

#### FinancialAdjustment

Preserves approved financial corrections without deleting original transactions.

| Attribute | Key | Description |
|---|---|---|
| AdjustmentID | PK | Unique adjustment identifier. |
| EnrollmentID | FK | References Enrollment. |
| PaymentID | FK, NULL | References Payment when applicable. |
| AdjustmentAmount | — | Signed adjustment amount. |
| AdjustmentReason | — | Documented justification. |
| ApprovedBy | FK | References UserAccount. |
| CreatedAt | — | Creation timestamp. |

Financial adjustments must preserve the original transaction history.

Any correction affecting allocated payments must maintain the consistency of the related allocations and outstanding balances.

### 4.6 User Management

#### UserAccount

| Attribute | Key | Description |
|---|---|---|
| UserID | PK | Unique user identifier. |
| Username | UNIQUE | Login username. |
| PasswordHash | — | Securely stored password hash. |
| AccountStatus | — | Account status. |
| CreatedAt | — | Creation timestamp. |

#### Role

| Attribute | Key | Description |
|---|---|---|
| RoleID | PK | Unique role identifier. |
| RoleName | UNIQUE | Institutional role name. |
| RoleDescription | — | Role description. |

#### Permission

| Attribute | Key | Description |
|---|---|---|
| PermissionID | PK | Unique permission identifier. |
| PermissionCode | UNIQUE | Permission code. |
| PermissionDescription | — | Permission description. |

#### UserRole

| Attribute | Key | Description |
|---|---|---|
| UserID | PK, FK | References UserAccount. |
| RoleID | PK, FK | References Role. |

#### RolePermission

| Attribute | Key | Description |
|---|---|---|
| RoleID | PK, FK | References Role. |
| PermissionID | PK, FK | References Permission. |

The role and permission structure implements Role-Based Access Control (RBAC).

### 4.7 System Auditing

#### AuditLog

| Attribute | Key | Description |
|---|---|---|
| AuditID | PK | Unique audit identifier. |
| UserID | FK | References UserAccount. |
| ActionType | — | Recorded operation. |
| EntityName | — | Affected entity. |
| RecordIdentifier | — | Affected record identifier. |
| OldValue | — | Previous value, when applicable. |
| NewValue | — | Updated value, when applicable. |
| EventTimestamp | — | Event timestamp. |

Audit records must be protected against unauthorized modification.

## 5. Relationship Design

The following table summarizes the primary structural relationships.

| Parent Entity | Child Entity | Cardinality |
|---|---|---|
| Student | Enrollment | 1:N |
| AcademicYear | Enrollment | 1:N |
| Enrollment | SectionAssignment | 1:N |
| ClassSection | SectionAssignment | 1:N |
| AcademicYear | AcademicTerm | 1:N |
| AcademicYear | ClassSection | 1:N |
| GradeLevel | ClassSection | 1:N |
| ClassSection | SectionSubject | 1:N |
| Subject | SectionSubject | 1:N |
| Teacher | TeachingAssignment | 1:N |
| SectionSubject | TeachingAssignment | 1:N |
| SectionSubject | AssessmentComponent | 1:N |
| AcademicTerm | AssessmentComponent | 1:N |
| Enrollment | StudentGrade | 1:N |
| AssessmentComponent | StudentGrade | 1:N |
| SectionSubject | GradeFinalization | 1:N |
| AcademicTerm | GradeFinalization | 1:N |
| AcademicYear | FeeStructure | 1:N |
| GradeLevel | FeeStructure | 1:N |
| Enrollment | StudentCharge | 1:N |
| FeeStructure | StudentCharge | 1:N |
| Enrollment | Payment | 1:N |
| Payment | PaymentAllocation | 1:N |
| StudentCharge | PaymentAllocation | 1:N |
| Enrollment | FinancialAdjustment | 1:N |
| Payment | FinancialAdjustment | 1:N |
| UserAccount | UserRole | 1:N |
| Role | UserRole | 1:N |
| Role | RolePermission | 1:N |
| Permission | RolePermission | 1:N |
| UserAccount | AuditLog | 1:N |

Additional foreign-key relationships connect user accounts to grade recording, grade finalization, payment recording, and financial approval operations.

## 6. Integrity and Validation Strategy

The system adopts **Multi-Layer Validation**, combining database constraints, business logic, and transaction management.

### 6.1 Database Constraints

Database-level constraints shall enforce:

- Primary and foreign keys.
- Required attributes.
- Unique institutional identifiers.
- Valid numerical ranges.
- Referential integrity.
- Appropriate uniqueness restrictions.

Where supported, conditional unique indexes and database constraints shall enforce rules concerning active records.

### 6.2 Business Logic Validation

The business logic layer shall verify rules involving multiple entities, including:

- Academic-year consistency between enrollments and section assignments.
- Non-overlapping student section assignments.
- Teacher authorization for grade recording.
- Assessment eligibility and scoring rules.
- Restrictions on finalized grades.
- Financial allocation and outstanding-balance rules.

### 6.3 Transaction Management

Related database operations shall execute within transactions.

Transaction isolation, locking, or equivalent concurrency controls must prevent conflicting updates from violating institutional rules.

Validation at the application level alone is insufficient for operations vulnerable to concurrent modifications.

## 7. Data Normalization

The proposed schema follows relational normalization principles.

**First Normal Form (1NF):** Attributes contain atomic values, and repeating groups are separated into related tables.

**Second Normal Form (2NF):** Non-key attributes depend on the complete primary key.

**Third Normal Form (3NF):** Non-key attributes do not depend transitively on primary keys through other non-key attributes.

Normalization shall be verified against the finalized schema before physical implementation.

## 8. Data Security and Recovery

The database shall support:

- Restricted database access.
- Secure password-hash storage.
- Role-based application authorization.
- Protected audit records.
- Controlled access to sensitive student and financial information.
- Centralized database backups.
- Reliable restoration procedures.
- Transactional consistency during concurrent operations.

Backup schedules and retention periods shall follow approved institutional policies.

## 9. ERD Deliverables

The Entity Relationship Diagram (ERD) represents the proposed logical entities and their relationships.

**Diagram files:**

- [ERD Diagram](diagrams/ERD.png)
- [Editable ERD Source](diagrams/ERD.drawio)

The ERD must remain consistent with this document.

The diagram and logical schema require final consistency verification before implementation, particularly for historical section assignments, assessment eligibility, and financial corrections.

## 10. Approved Design Decisions

| Decision ID | Decision | Status |
|---|---|---|
| DD-01 | Separate student enrollment from historical section assignments. | Approved |
| DD-02 | Introduce academic terms and section-subject associations. | Approved |
| DD-03 | Support configurable assessment components and weights. | Approved |
| DD-04 | Allocate payments to individual student charges. | Approved |
| DD-05 | Reject payments exceeding outstanding balances. | Approved |
| DD-06 | Preserve original financial transactions when recording corrections. | Approved |
| DD-07 | Apply database, business-logic, and transaction-level validation. | Approved |

## 11. Pending Implementation Decisions

| Decision ID | Pending Decision |
|---|---|
| PD-01 | Select the relational DBMS and database access technology. |
| PD-02 | Confirm assessment periods, grading formulas, and finalization authority. |
| PD-03 | Specify the detailed accounting procedure for reversing and reallocating corrected payments. |
| PD-04 | Define record retention and archival periods. |
| PD-05 | Finalize the role-permission matrix. |
| PD-06 | Define physical data types, indexes, and DBMS-specific constraints. |

## 12. Related Documents

| Document | Relationship |
|---|---|
| [SRS](../02-Requirements-Analysis/SRS.md) | Defines the overall system requirements. |
| [Functional Requirements](../02-Requirements-Analysis/Functional-Requirements.md) | Defines required operations and data management functions. |
| [Non-Functional Requirements](../02-Requirements-Analysis/Non-Functional-Requirements.md) | Defines security, reliability, and performance requirements. |
| [Business Rules](../03-System-Analysis/Business-Rules.md) | Defines institutional rules governing data and transactions. |
| [Process Analysis](../03-System-Analysis/Process-Analysis.md) | Defines processes and logical data flows. |
| [System Architecture](System-Architecture.md) | Defines the desktop application architecture and database deployment model. |
| [Interface Design](Interface-Design.md) | Defines user interfaces and interactions with stored data. |

## 13. Conclusion

The revised database design defines 25 relational entities supporting the School Management System's seven functional modules.

It incorporates historical student section assignments, configurable academic assessments, detailed payment allocations, overpayment prevention, and multi-layer data validation.

The design provides a structured foundation for final ERD verification, physical database implementation, and integration with the desktop application.

---

**End of Document**
