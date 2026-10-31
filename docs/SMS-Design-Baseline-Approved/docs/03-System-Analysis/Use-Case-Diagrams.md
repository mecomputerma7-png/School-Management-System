# Use Case Diagrams — School Management System

**Version:** 1.0  
**Scope:** One private school; desktop-only SMS.  
**Actors:** School Principal, System Administrator, Administrative Staff, Teacher, Accountant.

## Diagram index

| Diagram | SVG | PNG | Editable DOT |
|---|---|---|---|
| UC-Overview | [SVG](diagrams/UC-Overview.svg) | [PNG](diagrams/UC-Overview.png) | [DOT](diagrams/UC-Overview.dot) |
| UC-Student-Management | [SVG](diagrams/UC-Student-Management.svg) | [PNG](diagrams/UC-Student-Management.png) | [DOT](diagrams/UC-Student-Management.dot) |
| UC-Teacher-Management | [SVG](diagrams/UC-Teacher-Management.svg) | [PNG](diagrams/UC-Teacher-Management.png) | [DOT](diagrams/UC-Teacher-Management.dot) |
| UC-Class-Management | [SVG](diagrams/UC-Class-Management.svg) | [PNG](diagrams/UC-Class-Management.png) | [DOT](diagrams/UC-Class-Management.dot) |
| UC-Grade-Management | [SVG](diagrams/UC-Grade-Management.svg) | [PNG](diagrams/UC-Grade-Management.png) | [DOT](diagrams/UC-Grade-Management.dot) |
| UC-Fee-Management | [SVG](diagrams/UC-Fee-Management.svg) | [PNG](diagrams/UC-Fee-Management.png) | [DOT](diagrams/UC-Fee-Management.dot) |
| UC-User-Management | [SVG](diagrams/UC-User-Management.svg) | [PNG](diagrams/UC-User-Management.png) | [DOT](diagrams/UC-User-Management.dot) |
| UC-Reports-Management | [SVG](diagrams/UC-Reports-Management.svg) | [PNG](diagrams/UC-Reports-Management.png) | [DOT](diagrams/UC-Reports-Management.dot) |

## Use-case mapping

| ID | Use case | Primary authorized actor(s) |
|---|---|---|
| UC-01 | Authenticate User | School Principal, System Administrator, Administrative Staff, Teacher, Accountant |
| UC-02 | Register Student | Administrative Staff |
| UC-03 | Update Student Information | Administrative Staff |
| UC-04 | Register Teacher | Administrative Staff |
| UC-05 | Assign Teacher | Administrative Staff |
| UC-06 | Manage Class Sections | Administrative Staff |
| UC-07 | Record Student Grades | Teacher |
| UC-08 | Finalize Grades | Administrative Staff |
| UC-09 | Configure Tuition Fees | Accountant |
| UC-10 | Record Student Payment | Accountant |
| UC-11 | Correct Payment Transaction | Accountant |
| UC-12 | Manage User Accounts | System Administrator |
| UC-13 | Generate Reports | School Principal, System Administrator, Administrative Staff, Teacher, Accountant |

## Interpretation and design notes

- Actor–use-case connections indicate participation, **not** complete authorization rules. The actual RBAC permission matrix governs access.
- UC-08 “Finalize Grades” is shown under Administrative Staff as an **illustrative authorized academic staff member**, not a new institutional role; final approval authority remains a pending policy decision.
- UC-13 connects all five roles to represent potential access to **different authorized reports**; it does not grant every role access to all reports.
- UC-01 is shown in the overview as a shared authentication use case. Authentication is a precondition for protected operations; no UML `include` relationship is asserted for every operation.
- Student section transfer, assessment configuration, and payment allocation are internal workflows or supporting functions, not additional numbered use cases in the current baseline.
- The diagrams are derived from the 13 previously documented use cases and do not imply modules such as attendance, timetable, or standalone examination management.
- This package documents **UML-style use-case associations and system boundaries**. Review authorization and use-case descriptions before final academic sign-off.

## Related documents

- [Use Case Descriptions](../../03-System-Analysis/Use-Case-Descriptions.md)
- [Functional Requirements](../../02-Requirements-Analysis/Functional-Requirements.md)
- [Business Rules](../../03-System-Analysis/Business-Rules.md)
