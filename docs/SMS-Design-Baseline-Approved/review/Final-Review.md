# SMS — Integrated Final Design Review

**Review date:** 2026-10-30  
**Decision:** **CONDITIONAL / NOT FINAL SIGN-OFF**  
**Review type:** Actual-file inspection + approved-decision reconciliation; not a complete validation against missing authoritative requirements.

## 1. Approved decisions reflected in the baseline

- One private school, desktop-only; seven functional modules, five roles.
- Java + JavaFX client; separate Java application server; MySQL centralized database on Windows Server; client-to-server HTTPS, server-to-database JDBC.
- 10–30 concurrent users, LAN only, no offline mode.
- Configurable RBAC; only System Administrator may edit role permissions.
- Principal or delegated authorized user may finalize grades; changes afterward use audited corrections.
- Historical section assignments, charge-level payment allocations, immutable original financial transactions and corrective entries.
- One active academic year; **two terms** per year; Arabic and English UI; PDF/Excel exports; daily automatic backups; internal updates.

## 2. Files reviewed and concrete discrepancies

| ID | Severity | Observed evidence | Status / action |
|---|---|---|---|
| R-01 | CRITICAL | Original Deployment-Design.md says clients access DB directly and explicitly rejects an application server. Conflicts with approved architecture. | **Corrected in this package:** Deployment-Design.md and revised deployment diagram; old image preserved only in original external ZIP, not as the authoritative revised diagram. |
| R-02 | HIGH | Original Database-Design.md said DBMS undecided and client holds business/data layers; academic terms configurable. | **Text corrected in packaged baseline:** MySQL, application server, two terms. Physical SQL schema still pending. |
| R-03 | HIGH | Original database model contains both StudentGrade.EnrollmentID and SectionAssignmentID. This is not necessarily a schema error, but cross-reference equality and temporal membership are required. | **OPEN:** enforce composite consistency in service/SQL transactions and compare ERD, UML and sequence sources. Do not arbitrarily remove one FK. |
| R-04 | HIGH | Eight sequence diagram source files predate final JavaFX→server→MySQL decision. | **OPEN:** review/re-render each source to show remote HTTPS boundary, server-side RBAC and transaction control. Originals included for comparison. |
| R-05 | HIGH | Class diagrams are marked preliminary and may represent services without explicit remote boundaries. | **OPEN:** refine JavaFX controllers/API clients/server services/repositories and exception contracts. |
| R-06 | HIGH | No original Functional-Requirements.md, SRS.md, Business-Rules.md, full UC descriptions, or Interface-Design.md were available in this local working set. | **OPEN / BLOCKER:** cannot truthfully certify FR-001–069 coverage or NFR compliance. Provide originals to finish formal RTM. |
| R-07 | HIGH | RTM contains 69 placeholder rows, 0 source-verified requirement mappings. | **OPEN:** do not claim RTM complete; map exact requirement text and test evidence after obtaining originals. |
| R-08 | HIGH | Final role-permission matrix not present. Only the right to edit role permissions is conclusively restricted to System Administrator. | **OPEN:** approve all actions for five roles, sensitive reports, grade overrides and financial corrections. |
| R-09 | HIGH | Financial correction rules specify preservation but not a complete approved idempotent reversal/reallocation algorithm. | **OPEN:** specify adjustment, restored balances, concurrency locking and negative test cases. |
| R-10 | MEDIUM | DFD balancing claim is not supported by a complete boundary-flow crosswalk. | **OPEN:** manually reconcile named flows in Context vs Level 0 and document exact match. |
| R-11 | MEDIUM | Individual editable wireframes for six key screens are not present in available package; only composite image exists. | **OPEN:** create editable screen-level wireframes and check Arabic/English layouts. |
| R-12 | MEDIUM | Hosting topology, server framework, API contracts, TLS certificate operations, MySQL version, backup retention and recovery objectives remain unspecified. | **OPEN:** settle before production implementation; not necessary to select a product without evidence. |
| R-13 | MEDIUM | Currency, rounding precision, fee and grade-scale rules are not approved. | **OPEN:** institutional policy decisions required. |
| R-14 | LOW | Markdown cross-file links cannot be certified because authoritative source documents are missing. | **OPEN:** validate against final complete GitHub tree. |

## 3. Quality checks performed

- PASS: Architecture explicitly three tier.
- PASS: MySQL stated in DB document.
- PASS: No client direct DB in revised deployment.
- PASS: Two terms in revised database design.
- PASS: Eight PlantUML sequence sources.
- PASS: Two DFD PNG images.
- PASS: ERD editable drawio.
- PASS: 69 RTM rows.

These are **file-presence / text-level checks**, not semantic proof that UML and business rules are correct.

## 4. Closure sequence

1. Obtain authoritative FR, NFR, SRS, Business Rules, UC descriptions, and Interface Design documents.
2. Finalize action-level RBAC matrix and grading/financial school policies.
3. Align ERD and MySQL physical constraints with section transfer and grade history.
4. Update all eight sequence diagrams and implementation-facing class diagrams to three-tier architecture.
5. Approve transaction-safe financial correction algorithm and tests.
6. Verify DFD balancing and complete exact FR→UC→DFD→Data→UI→Test RTM.
7. Validate editable wireframes, internal links, and backup/restore acceptance tests.
8. Sign off baseline only after all critical/high findings are closed.

## 5. Scope limitations

This archive consolidates **the locally available design artifacts**, not every previously drafted document in the user's GitHub project. Some older diagrams are included unchanged and should be treated as **reference drafts** until reconciled. No SQL migration, functional tests or restore tests were executed.
