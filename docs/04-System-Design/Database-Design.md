# School Management System
## Database Design

**Document Version:** 1.0  
**Date:** October 2026  
**Status:** Proposed Design  
**Database Type:** Relational Database  
**Application Type:** Desktop Application

---

## 1. Purpose

This document defines the proposed logical database design for the School Management System (SMS).

It identifies the principal entities, attributes, relationships, keys, and integrity constraints required to support the system's seven functional modules.

The design provides a technology-independent foundation for database implementation and Entity Relationship Diagram (ERD) development.

## 2. Database Design Overview

The SMS uses a centralized relational database shared by authorized desktop workstations through the school's local area network.

The database is designed to:

- Maintain consistent institutional records.
- Support multiple authorized users.
- Minimize unnecessary data duplication.
- Preserve historical academic and financial information.
- Enforce referential integrity.
- Support secure and reliable transactions.

The proposed schema is logically normalized to **Third Normal Form (3NF)**, subject to verification during physical database implementation.

## 3. Entity Identification

The database is organized into seven logical domains.

| Domain | Entities |
|---|---|
| Student Management | Student, Enrollment |
| Teacher Management | Teacher, TeachingAssignment |
| Class Management | AcademicYear, GradeLevel, ClassSection, Subject |
| Grade Management | AssessmentComponent, StudentGrade, GradeFinalization |
| Fee Management | FeeStructure, StudentCharge, Payment, FinancialAdjustment |
| User Management | UserAccount, Role, Permission, UserRole, RolePermission |
| System Auditing | AuditLog |

**Total proposed entities: 20.**

## 4. Entity Definitions

### 4.1 Student Management

#### Student

Stores student identity and personal information.

| Attribute | Key | Description |
|---|---|---|
| StudentID | PK | Unique student identifier. |
| StudentNumber | UNIQUE | Institutional student number. |
| FirstName | — | Student's first name. |
| LastName | — | Student's last name. |
| DateOfBirth | — | Student's date of birth. |
| Gender | — | Student's recorded gender. |
| ContactInformation | — | Student contact details. |
| StudentStatus | — | Current institutional status. |
| CreatedAt | — | Record creation timestamp. |

#### Enrollment

Stores student enrollment information by academic year.

| Attribute | Key | Description |
|---|---|---|
| EnrollmentID | PK | Unique enrollment identifier. |
| StudentID | FK | References Student. |
| AcademicYearID | FK | References AcademicYear. |
| ClassSectionID | FK | References ClassSection. |
| EnrollmentDate | — | Date of enrollment. |
| EnrollmentStatus | — | Enrollment status. |
| EffectiveFrom | — | Beginning of the assignment period. |
| EffectiveTo | — | End of the assignment period, if applicable. |

**Constraint:** A student shall not have multiple active enrollments within the same academic year.

### 4.2 Teacher Management

#### Teacher

Stores teacher profiles.

| Attribute | Key | Description |
|---|---|---|
| TeacherID | PK | Unique teacher identifier. |
| TeacherNumber | UNIQUE | Institutional teacher number. |
| FirstName | — | Teacher's first name. |
| LastName | — | Teacher's last name. |
| ContactInformation | — | Teacher contact details. |
| TeacherStatus | — | Current employment status. |
| CreatedAt | — | Record creation timestamp. |

#### TeachingAssignment

Associates teachers with subjects and class sections.

| Attribute | Key | Description |
|---|---|---|
| AssignmentID | PK | Unique assignment identifier. |
| TeacherID | FK | References Teacher. |
| SubjectID | FK | References Subject. |
| ClassSectionID | FK | References ClassSection. |
| AcademicYearID | FK | References AcademicYear. |
| AssignmentStatus | — | Assignment status. |

**Constraint:** Duplicate active assignments for the same teacher, subject, section, and academic year are prohibited.

### 4.3 Class Management

#### AcademicYear

| Attribute | Key | Description |
|---|---|---|
| AcademicYearID | PK | Unique academic year identifier. |
| YearName | UNIQUE | Academic year label. |
| StartDate | — | Academic year start date. |
| EndDate | — | Academic year end date. |
| YearStatus | — | Academic year status. |

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
| SectionStatus | — | Current section status. |

**Constraint:** Section names shall be unique within each academic year and grade level.

#### Subject

| Attribute | Key | Description |
|---|---|---|
| SubjectID | PK | Unique subject identifier. |
| SubjectCode | UNIQUE | Institutional subject code. |
| SubjectName | — | Subject name. |
| SubjectStatus | — | Subject availability status. |

### 4.4 Grade Management

#### AssessmentComponent

Defines assessment components and their grading limits.

| Attribute | Key | Description |
|---|---|---|
| AssessmentComponentID | PK | Unique component identifier. |
| SubjectID | FK | References Subject. |
| AcademicYearID | FK | References AcademicYear. |
| ComponentName | — | Assessment component name. |
| MaximumScore | — | Maximum permitted score. |
| Weight | — | Contribution to the final grade. |

#### StudentGrade

Stores individual student assessment results.

| Attribute | Key | Description |
|---|---|---|
| StudentGradeID | PK | Unique grade record identifier. |
| EnrollmentID | FK | References Enrollment. |
| AssessmentComponentID | FK | References AssessmentComponent. |
| Score | — | Recorded student score. |
| GradeStatus | — | Provisional or finalized status. |
| RecordedBy | FK | References UserAccount. |
| RecordedAt | — | Grade entry timestamp. |

**Constraint:** Recorded scores must remain within the configured assessment limits.

#### GradeFinalization

Tracks grade finalization decisions.

| Attribute | Key | Description |
|---|---|---|
| FinalizationID | PK | Unique finalization identifier. |
| ClassSectionID | FK | References ClassSection. |
| SubjectID | FK | References Subject. |
| AcademicYearID | FK | References AcademicYear. |
| FinalizedBy | FK | References UserAccount. |
| FinalizedAt | — | Finalization timestamp. |
| FinalizationStatus | — | Current finalization status. |

### 4.5 Fee Management

#### FeeStructure

Defines tuition fees by academic year and grade level.

| Attribute | Key | Description |
|---|---|---|
| FeeStructureID | PK | Unique fee structure identifier. |
| AcademicYearID | FK | References AcademicYear. |
| GradeLevelID | FK | References GradeLevel. |
| FeeName | — | Fee designation. |
| FeeAmount | — | Approved fee amount. |
| FeeStatus | — | Fee structure status. |

#### StudentCharge

Stores assessed student fees.

| Attribute | Key | Description |
|---|---|---|
| ChargeID | PK | Unique charge identifier. |
| EnrollmentID | FK | References Enrollment. |
| FeeStructureID | FK | References FeeStructure. |
| ChargeAmount | — | Assessed amount. |
| ChargeDate | — | Date the charge was applied. |
| ChargeStatus | — | Charge status. |

#### Payment

Stores student payment transactions.

| Attribute | Key | Description |
|---|---|---|
| PaymentID | PK | Unique payment identifier. |
| PaymentNumber | UNIQUE | Institutional transaction number. |
| EnrollmentID | FK | References Enrollment. |
| PaymentAmount | — | Amount received. |
| PaymentDate | — | Date of payment. |
| PaymentStatus | — | Transaction status. |
| RecordedBy | FK | References UserAccount. |
| RecordedAt | — | Recording timestamp. |

#### FinancialAdjustment

Stores approved financial corrections and adjustments.

| Attribute | Key | Description |
|---|---|---|
| AdjustmentID | PK | Unique adjustment identifier. |
| EnrollmentID | FK | References Enrollment. |
| PaymentID | FK, NULL | References Payment when applicable. |
| AdjustmentAmount | — | Signed adjustment amount. |
| AdjustmentReason | — | Documented justification. |
| ApprovedBy | FK | References UserAccount. |
| CreatedAt | — | Adjustment timestamp. |

**Constraint:** Financial corrections shall preserve original payment transactions.

### 4.6 User Management

#### UserAccount

| Attribute | Key | Description |
|---|---|---|
| UserID | PK | Unique user identifier. |
| Username | UNIQUE | Login username. |
| PasswordHash | — | Securely stored password hash. |
| AccountStatus | — | Active or inactive status. |
| CreatedAt | — | Account creation timestamp. |

#### Role

| Attribute | Key | Description |
|---|---|---|
| RoleID | PK | Unique role identifier. |
| RoleName | UNIQUE | Institutional role name. |
| RoleDescription | — | Role responsibility description. |

#### Permission

| Attribute | Key | Description |
|---|---|---|
| PermissionID | PK | Unique permission identifier. |
| PermissionCode | UNIQUE | System permission code. |
| PermissionDescription | — | Description of the permitted operation. |

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

### 4.7 System Auditing

#### AuditLog

| Attribute | Key | Description |
|---|---|---|
| AuditID | PK | Unique audit record identifier. |
| UserID | FK | References UserAccount. |
| ActionType | — | Operation performed. |
| EntityName | — | Affected entity. |
| RecordIdentifier | — | Identifier of the affected record. |
| OldValue | — | Previous value, when applicable. |
| NewValue | — | Updated value, when applicable. |
| EventTimestamp | — | Time of the event. |

Audit records shall be protected against unauthorized modification.

## 5. Entity Relationships

The following table summarizes the primary relationships.

| Parent Entity | Child Entity | Cardinality |
|---|---|---|
| Student | Enrollment | 1:N |
| AcademicYear | Enrollment | 1:N |
| ClassSection | Enrollment | 1:N |
| Teacher | TeachingAssignment | 1:N |
| Subject | TeachingAssignment | 1:N |
| ClassSection | TeachingAssignment | 1:N |
| AcademicYear | TeachingAssignment | 1:N |
| AcademicYear | ClassSection | 1:N |
| GradeLevel | ClassSection | 1:N |
| Subject | AssessmentComponent | 1:N |
| AcademicYear | AssessmentComponent | 1:N |
| Enrollment | StudentGrade | 1:N |
| AssessmentComponent | StudentGrade | 1:N |
| ClassSection | GradeFinalization | 1:N |
| Subject | GradeFinalization | 1:N |
| AcademicYear | GradeFinalization | 1:N |
| AcademicYear | FeeStructure | 1:N |
| GradeLevel | FeeStructure | 1:N |
| Enrollment | StudentCharge | 1:N |
| FeeStructure | StudentCharge | 1:N |
| Enrollment | Payment | 1:N |
| Enrollment | FinancialAdjustment | 1:N |
| Payment | FinancialAdjustment | 1:N |
| UserAccount | UserRole | 1:N |
| Role | UserRole | 1:N |
| Role | RolePermission | 1:N |
| Permission | RolePermission | 1:N |
| UserAccount | AuditLog | 1:N |

Relationships involving user references in grade, payment, approval, and finalization records are additionally enforced through foreign keys.

## 6. Key and Integrity Constraints

### 6.1 Primary Keys

Every entity shall have a primary key identifying each record uniquely.

Composite primary keys are used in `UserRole` and `RolePermission`.

### 6.2 Foreign Keys

Foreign keys shall enforce valid relationships between institutional records.

Deletion or modification of referenced records shall not compromise historical academic or financial information.

### 6.3 Unique Constraints

Unique constraints shall be applied to institutional identifiers, usernames, and other attributes requiring uniqueness.

Conditional uniqueness rules shall be implemented where business rules restrict only active records.

### 6.4 Validation Constraints

The database and application shall enforce applicable constraints, including:

- Non-negative fee amounts.
- Positive payment amounts.
- Valid assessment score ranges.
- Valid academic year date ranges.
- Required fields for mandatory relationships.
- Permitted record status values.

## 7. Normalization Strategy

The proposed database follows relational normalization principles.

### First Normal Form (1NF)

Attributes shall contain atomic values, and repeating groups shall be avoided.

### Second Normal Form (2NF)

Non-key attributes shall depend on the complete primary key.

### Third Normal Form (3NF)

Non-key attributes shall not depend transitively on primary keys through other non-key attributes.

Normalization shall be verified against the finalized schema before physical implementation.

## 8. Transaction Management

Database transactions shall maintain consistency across related operations.

Examples include:

- Student enrollment and class assignment.
- Grade finalization and related status updates.
- Payment recording and financial balance updates.
- Approved corrections to financial transactions.

Transactions shall provide atomicity, consistency, isolation, and durability (ACID), according to the selected DBMS capabilities.

## 9. Data Security and Recovery

The database design shall support:

- Restricted database access.
- Secure storage of password hashes.
- Role-based application authorization.
- Audit logging of sensitive operations.
- Protected database backups.
- Controlled restoration procedures.
- Referential integrity during recovery.

Backup frequency and retention periods shall follow approved operational policies.

## 10. ERD Deliverable

The Entity Relationship Diagram shall represent the proposed entities and their relationships.

**Planned file:**

`diagrams/ERD.png`

An editable source version shall be maintained alongside the exported diagram.

The ERD shall be reviewed against the finalized entity definitions and relationship constraints before implementation.

## 11. Design Decisions Requiring Confirmation

| Decision ID | Pending Decision |
|---|---|
| DD-01 | Select the relational Database Management System (DBMS). |
| DD-02 | Define the final academic assessment structure and grading periods. |
| DD-03 | Confirm financial adjustment and overpayment policies. |
| DD-04 | Define institutional record retention periods. |
| DD-05 | Confirm user-role assignments and detailed permission mappings. |

## 12. Related Documents

| Document | Relationship |
|---|---|
| [SRS](../02-Requirements-Analysis/SRS.md) | Defines system requirements and scope. |
| [Functional Requirements](../02-Requirements-Analysis/Functional-Requirements.md) | Defines required data-related operations. |
| [Business Rules](../03-System-Analysis/Business-Rules.md) | Defines institutional data constraints. |
| [Process Analysis](../03-System-Analysis/Process-Analysis.md) | Identifies logical data stores and information flows. |
| [System Architecture](System-Architecture.md) | Defines the application's architecture and database deployment model. |
| [Interface Design](Interface-Design.md) | Defines user interfaces that interact with the database. |

## 13. Conclusion

The proposed database design establishes the principal relational entities, relationships, and integrity controls required by the SMS.

It supports centralized data management, institutional record consistency, controlled multi-user access, and historical information preservation.

The design provides the basis for detailed ERD development, physical database implementation, and integration with the desktop application.

---

**End of Document**
