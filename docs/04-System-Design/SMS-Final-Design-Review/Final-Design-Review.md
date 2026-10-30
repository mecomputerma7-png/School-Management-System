# School Management System — Final Design Review

**Version:** 1.0  
**Review date:** 29 October 2026  
**Decision:** **CONDITIONAL — NOT YET BASELINE-APPROVED**  
**Scope:** Desktop-only; one private school; seven functional modules.

## 1. Review basis and limitations

This review inspected the locally available Database Design, Class Diagram documentation, DFD documentation, Use Case documentation, Deployment Design, Activity Diagram documentation, and packaged diagram files. It is a **document-level preliminary consistency review**, not a complete verification of every FR, NFR, BR, and UC: those standalone source documents were not all available locally. Any unverified cross-reference remains open.

## 2. Deliverable inventory

| Artifact | Available | Review disposition |
|---|---|---|
| Use Case diagrams | Overview + seven module diagrams | Present; RBAC-to-use-case matrix needs sign-off |
| DFD Context + Level 0 | Two diagrams | Present; formal balancing not yet independently demonstrated |
| Activity diagrams | Four key workflows | Present; scenario coverage is selective |
| ERD and Database Design | 25 logical entities | Logical baseline documented; DBMS implementation pending |
| Class diagrams | Overview + three domain diagrams | Proposed, not implementation-complete |
| Sequence diagrams | Eight workflows | Present; reconcile identifiers and transaction rules |
| Deployment diagram | Client/server LAN topology | Present; infrastructure/security decisions pending |
| Interface design | Screen inventory and preliminary wireframe concept | Final individual screen wireframes not verified in local package |

## 3. Findings and actions

| ID | Priority | Finding | Required action | Acceptance evidence |
|---|---|---|---|---|
| FDR-01 | HIGH | Source documentation is distributed across separate bundles, with no single version-controlled, authoritative integrated baseline. | Consolidate approved `.md` files and diagrams into GitHub `docs/` tree. | Commit/tag with an artifact index and version numbers. |
| FDR-02 | HIGH | Exact permissions for grade finalization, financial correction, and sensitive reporting remain subject to policy confirmation. | Approve an RBAC matrix mapping five roles to protected use cases and permissions. | Signed permission matrix and corresponding UC/UI/service checks. |
| FDR-03 | HIGH | Grade record relationship must be consistent across ERD, class, sequence, and validation rules: `StudentGrade` enrollment vs section-assignment identity requires explicit reconciliation. | Choose canonical FK and validate term/section history eligibility, then update all diagrams. | One matching FK definition in DB design, ERD, classes and grade sequence diagrams. |
| FDR-04 | HIGH | Financial reversal and allocation restoration are documented as not yet fully specified. | Define correction transaction algorithm, invariants, idempotency and audit trail. | Approved correction scenarios, tests and database constraints. |
| FDR-05 | MEDIUM | DFD notes state balancing but do not provide an exhaustive context-to-Level-0 flow mapping. | Produce a flow balancing matrix with exact source/destination and data names. | Every context boundary flow matched in Level 0. |
| FDR-06 | MEDIUM | Development stack, DBMS, database deployment security and backup/restore specifics are still open. | Select technology and document connection encryption, credentials, migration and restore approach. | Technology decision record and deployment checklist. |
| FDR-07 | MEDIUM | Class diagrams are explicitly preliminary and do not fully specify all methods, exception contracts or persistence mappings. | Finalize implementation-facing classes after stack selection. | Updated UML consistent with service and repository interfaces. |
| FDR-08 | MEDIUM | Independent editable wireframes for all priority screens were not confirmed among reviewed local deliverables. | Produce and review individual wireframes for login, dashboard, student, grades, payment, reports. | Six reviewed wireframe artifacts. |
| FDR-09 | LOW | Relative Markdown links need testing after files are placed in final GitHub folders. | Run repository-wide link checks after consolidation. | Zero broken internal references. |
| FDR-10 | MEDIUM | End-to-end FR→UC→Process→Data→UI→Test traceability has not been demonstrated. | Create a requirements traceability matrix with evidence per requirement. | All in-scope requirements mapped or explicitly waived. |

## 4. Confirmed design alignments

- One school, desktop-only application, centralized relational database on the school LAN.
- Seven modules: Student, Teacher, Class, Grade, Fee, User and Reports Management.
- Five direct system roles: School Principal, System Administrator, Administrative Staff, Teacher and Accountant.
- Database logical inventory has 25 entities; reporting is a read capability, not a separate operational table.
- Enrollment and historical section assignment are separate concepts.
- Payments are allocated to charges, overpayments are rejected, and financial history is preserved.
- Authentication/RBAC and audit are cross-cutting, not extra functional modules.

## 5. Exit criteria for design approval

1. Close all HIGH-priority findings.
2. Resolve MEDIUM findings or document accepted deferrals with owners.
3. Produce the requirements traceability matrix and DFD balancing matrix.
4. Reconcile diagram identifiers and transaction rules against the approved database design.
5. Confirm role permissions, grading policy, currency/rounding and financial correction behavior.
6. Freeze the versioned documentation baseline before implementation.

## 6. Recommended next deliverable

**Requirements Traceability Matrix (RTM)**: map FR-001–FR-069 to UC-01–UC-13, seven processes, relevant DB entities, interfaces and tests. The original FR and UC files must be consulted before claiming complete mappings.

## 7. Review conclusion

The project has a broad and coherent **proposed design package** but is **not yet ready for unconditional design sign-off**. This report identifies the remaining approvals and validation tasks; it does not assert that every diagram has been fully validated.
