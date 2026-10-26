# School Management System
## Business Rules

**Document Version:** 1.0  
**Date:** October 2026  
**Status:** Draft

---

## 1. Purpose

This document defines the institutional policies and operational constraints governing the School Management System (SMS).

Business rules establish the conditions under which school operations are permitted, restricted, calculated, or approved.

Each rule has a unique identifier (`BR-XXX`) to support traceability, implementation, and testing.

The rules in this document represent the proposed operational policy baseline and require institutional approval before implementation.

## 2. Rule Classification

| Category | Description |
|---|---|
| Validation | Conditions that determine whether submitted information is acceptable. |
| Constraint | Restrictions on records, relationships, or permitted operations. |
| Calculation | Rules governing derived academic or financial values. |
| Authorization | Conditions requiring specific permissions or approval. |
| Lifecycle | Rules controlling changes in record status. |

## 3. Student Management Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-001 | Each student shall have one permanent, unique student identifier that is not reused. | Constraint |
| BR-002 | A student shall not have more than one active enrollment in the same academic year. | Constraint |
| BR-003 | Student registration requires completion of all mandatory identity and enrollment fields. | Validation |
| BR-004 | Student records referenced by academic or financial transactions shall not be permanently deleted. | Constraint |
| BR-005 | Changes to a student's enrollment status shall preserve the previous status and its effective date. | Lifecycle |

## 4. Teacher Management Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-006 | Each teacher shall have a unique institutional identifier. | Constraint |
| BR-007 | Only active teachers may receive new teaching assignments. | Constraint |
| BR-008 | A teaching assignment shall identify the teacher, academic year, subject, and class section. | Validation |
| BR-009 | Duplicate active assignments for the same teacher, subject, and class section within an academic year are prohibited. | Constraint |
| BR-010 | Deactivating a teacher shall prevent new assignments without removing historical teaching records. | Lifecycle |

## 5. Class Management Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-011 | Each class section shall belong to one academic year and one grade level. | Constraint |
| BR-012 | A class section identifier shall be unique within its academic year and grade level. | Constraint |
| BR-013 | A student may belong to only one active class section within an academic year. | Constraint |
| BR-014 | Student transfers between class sections shall preserve the previous assignment and its effective dates. | Lifecycle |
| BR-015 | A class section containing active student enrollments shall not be deleted. | Constraint |

## 6. Grade Management Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-016 | Grade values shall remain within the minimum and maximum limits configured for the applicable assessment component. | Validation |
| BR-017 | Only teachers assigned to the relevant subject and class section may enter or modify grades, unless an explicitly authorized exception applies. | Authorization |
| BR-018 | Final grades shall be calculated using the approved assessment weights and grading formula. | Calculation |
| BR-019 | A grading period shall not be finalized while mandatory grade components remain incomplete. | Constraint |
| BR-020 | Finalized grades shall be read-only until reopened through an authorized procedure. | Lifecycle |
| BR-021 | Reopening finalized grades shall require an authorized decision, a documented reason, and an audit record. | Authorization |
| BR-022 | Grade corrections shall preserve an audit trail identifying the previous value, new value, responsible user, and modification time. | Lifecycle |

## 7. Fee Management Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-023 | Each tuition fee structure shall identify its applicable academic year and grade level. | Validation |
| BR-024 | Fee amounts shall be non-negative and expressed using the institution's approved currency. | Validation |
| BR-025 | A student's outstanding balance shall equal assessed fees minus valid payments and approved adjustments. | Calculation |
| BR-026 | A recorded payment amount shall be greater than zero. | Validation |
| BR-027 | A payment shall not exceed the outstanding balance unless an approved overpayment policy exists. | Constraint |
| BR-028 | Each payment transaction shall have a unique, non-reusable transaction identifier. | Constraint |
| BR-029 | Posted payment transactions shall not be permanently deleted or silently overwritten. | Constraint |
| BR-030 | Payment corrections or reversals shall preserve the original transaction and record the reason, responsible user, and correction time. | Lifecycle |
| BR-031 | Fee adjustments shall require appropriate authorization and a documented justification. | Authorization |

## 8. User and Access Control Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-032 | Every system user shall have a unique account identifier. | Constraint |
| BR-033 | Access to system functions and records shall be determined by assigned roles and permissions. | Authorization |
| BR-034 | Inactive user accounts shall not be permitted to authenticate. | Authorization |
| BR-035 | Users shall not grant themselves additional permissions. | Authorization |
| BR-036 | Sensitive administrative, academic, and financial changes shall be attributable to an authenticated user. | Constraint |
| BR-037 | Changes to user roles and account status shall be recorded in the audit log. | Lifecycle |

## 9. Reporting and Information Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-038 | Reports shall include only records the requesting user is authorized to access. | Authorization |
| BR-039 | Financial reports shall reflect posted transactions and approved adjustments. | Calculation |
| BR-040 | Academic reports shall distinguish finalized grades from provisional grades. | Constraint |
| BR-041 | Reports containing personal or financial information shall be accessible only to authorized roles. | Authorization |

## 10. Cross-Module Rules

| Rule ID | Business Rule | Category |
|---|---|---|
| BR-042 | Academic and financial records shall reference valid institutional entities. | Constraint |
| BR-043 | The academic year associated with a transaction shall remain identifiable after the year is closed. | Lifecycle |
| BR-044 | Operations affecting multiple related records shall either complete successfully as one transaction or leave the original records unchanged. | Constraint |
| BR-045 | Changes to approved institutional policies shall take effect only after authorization and documentation. | Authorization |

## 11. Rule Traceability

The following table identifies the primary operational areas in which the rules apply.

| Business Rule Range | Related Use Cases |
|---|---|
| BR-001–BR-005 | UC-02, UC-03, UC-06 |
| BR-006–BR-010 | UC-04, UC-05 |
| BR-011–BR-015 | UC-06 |
| BR-016–BR-022 | UC-07, UC-08 |
| BR-023–BR-031 | UC-09, UC-10, UC-11 |
| BR-032–BR-037 | UC-01, UC-12 |
| BR-038–BR-041 | UC-13 |
| BR-042–BR-045 | Multiple Use Cases |

Detailed functional requirement references are maintained in `../02-Requirements-Analysis/Functional-Requirements.md`.

## 12. Policy Decisions Requiring Approval

The following decisions must be confirmed by the school before the corresponding rules can be finalized.

| Decision ID | Pending Decision | Affected Rules |
|---|---|---|
| PD-01 | Define the official academic grading scale, assessment components, and calculation weights. | BR-016, BR-018 |
| PD-02 | Identify the roles authorized to finalize and reopen grades. | BR-020, BR-021 |
| PD-03 | Confirm the tuition currency, applicable fee structures, and adjustment approval authority. | BR-023, BR-024, BR-031 |
| PD-04 | Determine whether overpayments are prohibited or handled through an approved credit policy. | BR-027 |
| PD-05 | Define the retention periods for student, academic, financial, and audit records. | BR-004, BR-022, BR-029, BR-037 |
| PD-06 | Approve the role-permission matrix for sensitive operations and reporting. | BR-017, BR-033, BR-038, BR-041 |

These decisions shall be recorded before the affected functionality is approved for production use.

## 13. Rule Governance

Business rules shall be reviewed whenever institutional policies or approved system requirements change.

Any modification must preserve the original rule identifier where possible, document the reason for change, and identify the affected requirements, use cases, and test cases.

Approved changes shall be reflected in the relevant system documentation before implementation.

---

**End of Document**
