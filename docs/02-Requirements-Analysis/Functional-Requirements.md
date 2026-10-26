# School Management System
## Functional Requirements

**Document Version:** 1.0  
**Date:** October 2026  
**Status:** Draft

---

## 1. Purpose

This document specifies the functional requirements of the School Management System (SMS). Each requirement describes a capability the system must provide and serves as a basis for system design, implementation, and testing.

Requirements are identified using the format `FR-XXX` and grouped by functional module.

## 2. Student Management

| ID | Functional Requirement |
|---|---|
| FR-001 | The system shall allow authorized administrative staff to register students using required personal and enrollment information. |
| FR-002 | The system shall generate a unique student identifier upon successful registration. |
| FR-003 | The system shall validate required fields and prevent duplicate student records based on defined identification rules. |
| FR-004 | The system shall allow authorized staff to view and update student information. |
| FR-005 | The system shall allow authorized staff to search and filter student records by identifier, name, grade level, and enrollment status. |
| FR-006 | The system shall maintain each student's enrollment status, including active, transferred, graduated, and withdrawn. |
| FR-007 | The system shall preserve historical student records when enrollment status changes. |
| FR-008 | The system shall allow authorized staff to view a student's current class assignment and academic record. |

## 3. Teacher Management

| ID | Functional Requirement |
|---|---|
| FR-009 | The system shall allow authorized staff to register teachers and maintain their professional and contact information. |
| FR-010 | The system shall assign a unique identifier to each teacher. |
| FR-011 | The system shall allow authorized staff to view, search, and update teacher records. |
| FR-012 | The system shall maintain teacher employment status, including active and inactive. |
| FR-013 | The system shall allow authorized staff to assign teachers to class sections and subjects. |
| FR-014 | The system shall prevent the assignment of inactive teachers to new teaching responsibilities. |
| FR-015 | The system shall allow authorized users to view teaching assignments associated with each teacher. |

## 4. Class Management

| ID | Functional Requirement |
|---|---|
| FR-016 | The system shall allow authorized staff to create and manage academic years. |
| FR-017 | The system shall allow authorized staff to define grade levels and class sections. |
| FR-018 | The system shall assign a unique identifier to each class section within an academic year. |
| FR-019 | The system shall allow authorized staff to assign students to class sections. |
| FR-020 | The system shall prevent a student from being assigned to more than one active class section within the same academic year. |
| FR-021 | The system shall allow authorized staff to transfer students between class sections while preserving assignment history. |
| FR-022 | The system shall allow authorized staff to define subjects associated with each grade level. |
| FR-023 | The system shall allow authorized users to view class rosters, assigned teachers, and associated subjects. |

## 5. Grade Management

| ID | Functional Requirement |
|---|---|
| FR-024 | The system shall allow authorized staff to define grading periods within an academic year. |
| FR-025 | The system shall allow authorized staff to configure grading components, maximum scores, and calculation rules for each subject. |
| FR-026 | The system shall allow teachers to enter grades only for students, subjects, and class sections assigned to them. |
| FR-027 | The system shall validate entered grades against the configured minimum and maximum values. |
| FR-028 | The system shall allow authorized teachers to update grades before the grading period is finalized. |
| FR-029 | The system shall calculate subject totals and final results according to the configured grading rules. |
| FR-030 | The system shall prevent unauthorized modification of finalized grades. |
| FR-031 | The system shall allow authorized personnel to reopen finalized grades with a recorded reason and appropriate approval. |
| FR-032 | The system shall allow authorized users to view individual student results and class-level grade summaries. |

## 6. Fee Management

| ID | Functional Requirement |
|---|---|
| FR-033 | The system shall allow authorized accounting staff to define tuition fee structures by academic year and grade level. |
| FR-034 | The system shall allow authorized accounting staff to assign applicable fees to student accounts. |
| FR-035 | The system shall calculate each student's outstanding balance from assessed fees, approved adjustments, and recorded payments. |
| FR-036 | The system shall allow authorized accounting staff to record full and partial payments. |
| FR-037 | The system shall generate a unique transaction identifier for each recorded payment. |
| FR-038 | The system shall generate a receipt containing the payment identifier, student information, amount, date, and remaining balance. |
| FR-039 | The system shall allow authorized accounting staff to record fee discounts and adjustments with a documented reason. |
| FR-040 | The system shall prevent recorded payments from exceeding the outstanding balance unless an authorized overpayment policy is configured. |
| FR-041 | The system shall maintain a history of fee assessments, adjustments, and payments for each student. |
| FR-042 | The system shall allow authorized accounting staff to view paid, partially paid, and unpaid student accounts. |
| FR-043 | The system shall support controlled payment corrections or reversals without deleting the original transaction record. |

## 7. User Management

| ID | Functional Requirement |
|---|---|
| FR-044 | The system shall allow the System Administrator to create, update, activate, and deactivate user accounts. |
| FR-045 | The system shall authenticate users before granting access to protected functions. |
| FR-046 | The system shall enforce role-based access control for all protected operations. |
| FR-047 | The system shall support the following user roles: School Principal, System Administrator, Administrative Staff, Teacher, and Accountant. |
| FR-048 | The system shall allow the System Administrator to assign and update user roles within authorized administrative permissions. |
| FR-049 | The system shall allow authenticated users to change their passwords. |
| FR-050 | The system shall provide a controlled password reset process for authorized administrators. |
| FR-051 | The system shall terminate authenticated sessions when users log out. |
| FR-052 | The system shall deny access to protected functions when a user's account is inactive or the required permission is absent. |
| FR-053 | The system shall record security-relevant activities, including login attempts, permission changes, and account deactivation. |

## 8. Reports Management

| ID | Functional Requirement |
|---|---|
| FR-054 | The system shall allow authorized users to generate student enrollment reports. |
| FR-055 | The system shall allow authorized users to generate teacher and class assignment reports. |
| FR-056 | The system shall allow authorized users to generate student grade reports and class-level academic summaries. |
| FR-057 | The system shall allow authorized accounting staff and the School Principal to generate fee collection and outstanding balance reports. |
| FR-058 | The system shall allow users to filter reports by applicable criteria, including academic year, grade level, class section, and date range. |
| FR-059 | The system shall support exporting applicable reports to PDF. |
| FR-060 | The system shall provide print-ready report layouts containing report titles, generation dates, and relevant institutional information. |
| FR-061 | The system shall restrict report access according to user roles and data permissions. |
| FR-062 | The system shall allow the School Principal to view summary reports covering enrollment, academic results, and fee collection. |

## 9. Cross-Module Requirements

| ID | Functional Requirement |
|---|---|
| FR-063 | The system shall use a centralized database for information shared across modules. |
| FR-064 | The system shall maintain relationships between students, academic years, class sections, subjects, grades, and fee records. |
| FR-065 | The system shall prevent unauthorized users from accessing or modifying records through either the web or desktop interface. |
| FR-066 | The system shall validate submitted data before storing changes in the database. |
| FR-067 | The system shall provide confirmation or error messages for data-changing operations. |
| FR-068 | The system shall record audit information for sensitive operations, including grade reopening, payment reversals, and permission changes. |
| FR-069 | The system shall ensure that web and desktop components access shared business data through centralized application services. |

## 10. Requirement Priorities

Requirements are classified using the following priority levels:

| Priority | Definition |
|---|---|
| Must Have | Essential for the initial system release. |
| Should Have | Important functionality that may be deferred if necessary. |
| Could Have | Optional functionality considered for future improvement. |

All requirements in this document are initially classified as **Must Have**, except the following:

| Requirement | Priority |
|---|---|
| FR-031 — Reopen finalized grades | Should Have |
| FR-039 — Fee discounts and adjustments | Should Have |
| FR-058 — Advanced report filtering | Should Have |
| FR-059 — PDF report export | Should Have |

Priorities may be refined during requirements validation without changing the approved project scope.

## 11. Requirements Traceability

The following table links stakeholder requirements to the functional requirements that address them.

| Stakeholder Requirement | Related Functional Requirements |
|---|---|
| SR-01 — Institutional reporting | FR-054–FR-062 |
| SR-02 — Account and permission management | FR-044–FR-053 |
| SR-03 — Student registration and records | FR-001–FR-008 |
| SR-04 — Class and student assignments | FR-016–FR-023 |
| SR-05 — Grade recording and updates | FR-024–FR-032 |
| SR-06 — Fee and payment management | FR-033–FR-043 |
| SR-07 — Receipts and financial summaries | FR-038, FR-057 |
| SR-08 — Institutional performance summaries | FR-062 |
| SR-09 — Secure authorized access | FR-045–FR-046, FR-052, FR-061, FR-065 |
| SR-10 — System integrity and maintainability | FR-063–FR-069 |

## 12. Conclusion

This document defines **69 functional requirements** across the seven core modules and their shared operations.

These requirements establish the functional baseline for system analysis, database design, interface design, implementation, and testing.

Detailed business rules, use cases, and acceptance criteria will be documented in the subsequent analysis and specification artifacts.

---

**End of Document**
