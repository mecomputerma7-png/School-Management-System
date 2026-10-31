# School Management System — Requirements Traceability Matrix (RTM)

**Version:** 0.1 — WORKING DRAFT / NOT VERIFIED  
**Scope:** Single private school; desktop-only; seven modules  
**Coverage:** FR-001 through FR-069 (69 placeholder records; 0 source-verified mappings)

> **Integrity warning:** The original `Functional-Requirements.md` and `SRS.md` were not present in the available files. Therefore this is a traceability **framework**, not a validated mapping. Requirement statements, IDs-to-modules, business rules and exact test expectations MUST be taken from those source documents. Never interpret the empty columns as approved mappings.

## 1. Traceability method

For each FR: copy its exact wording from the authoritative source, identify the owning module, link the documented UC(s), DFD process, data entities, UI screens, business rules, and executable test cases. Verify each link by comparing actual artifact content; mark `Verified` only after inspection. Use `Not applicable` with justification for non-applicable links.

## 2. Known system-level artifact crosswalk (NOT FR-ID assignments)

| Module | DFD process | Candidate UCs | Candidate UI | Candidate data entities |
|---|---|---|---|---|
| Student Management | P1 | UC-02; UC-03 | UI-03; UI-04; UI-05 | Student; Enrollment; SectionAssignment |
| Teacher Management | P2 | UC-04; UC-05 | UI-06; UI-07 | Teacher; TeachingAssignment |
| Class Management | P3 | UC-06 | UI-08; UI-09 | AcademicYear; AcademicTerm; GradeLevel; ClassSection; Subject; SectionSubject |
| Grade Management | P4 | UC-07; UC-08 | UI-10; UI-11; UI-12 | AssessmentComponent; StudentGrade; GradeFinalization |
| Fee Management | P5 | UC-09; UC-10; UC-11 | UI-13; UI-14; UI-15 | FeeStructure; StudentCharge; Payment; PaymentAllocation; FinancialAdjustment |
| User Management | P6 | UC-01; UC-12 | UI-01; UI-16; UI-17 | UserAccount; Role; Permission; UserRole; RolePermission |
| Reports Management | P7 | UC-13 | UI-18 | Operational entities (read only) |

The table above summarizes documented module-level relationships only. It does **not** establish that any particular `FR-###` maps to those artifacts.

## 3. Test identification

`TC-FR-001` to `TC-FR-069` are **proposed test IDs**, not completed test specifications or executed tests. For each FR, add at least one positive test and relevant negative/boundary tests. Separate test scenarios may share a requirement ID but need unique final test IDs.

## 4. Priority unresolved decisions

- FDR-02: approve RBAC permissions for grade finalization, financial correction, and sensitive reports.
- FDR-03: reconcile `StudentGrade` references to enrollment versus section assignment in every artifact.
- FDR-04: define transaction-safe financial correction and allocation restoration.
- FDR-05: produce Context-to-Level-0 DFD flow balancing evidence.
- FDR-06: approve stack, database connectivity security, backup and restore.
- FDR-10: complete and verify all 69 FR traceability rows.

## 5. Acceptance criteria

1. Every FR has the **exact authoritative text** and correct module.
2. Every FR is linked to its relevant design artifacts, or a justified `Not applicable`.
3. Every FR has a reviewed test case and pass/fail evidence once implementation begins.
4. No invented relationships, role permissions, or requirement wording.
5. All high-priority FDR findings are closed before final design sign-off.

## 6. Files

- `RTM-69-Requirements.csv` — 69 editable requirement records (all explicitly unverified).
- `RTM.md` — methodology, module-level crosswalk and acceptance criteria.

**Recommended repository location:** `docs/02-Requirements-Analysis/RTM.md` and `docs/02-Requirements-Analysis/RTM-69-Requirements.csv`.
