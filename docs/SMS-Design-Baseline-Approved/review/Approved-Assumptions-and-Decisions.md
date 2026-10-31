# SMS — Approved Design Baseline and Assumptions
**Baseline:** v1.0 | **Date:** 2026-10-30 | **Status:** Approved for academic design completion; implementation validation pending.

## A. User-approved decisions (binding)
- One private school, 300–800 students; seven modules only; five roles.
- Windows JavaFX clients → HTTPS → standalone Java application server → JDBC → central MySQL on Windows Server. No direct client SQL access.
- 10–30 concurrent users; school LAN; no offline operation; one active academic year; exactly two terms.
- Username/password, configurable server-enforced RBAC; **only System Administrator edits role permissions**.
- Principal or explicitly delegated grade approver finalizes grades; later changes require approved, audited correction.
- Section transfers retain non-overlapping history. Financial records immutable; adjustments append compensating entries. Payment allocation to individual charges; reject overpayments.
- Daily automatic backups; bilingual Arabic/English UI; PDF/XLSX reports; controlled internal updates.
- Development goal is full implementation; no further professor-imposed requirements.

## B. Adopted reasonable implementation assumptions (provisional, not user-approved facts)
| ID | Topic | Adopted baseline |
|---|---|---|
| A-01 | API | Versioned REST/JSON over TLS 1.2+; server-side authentication and validation; OpenAPI contract before coding. |
| A-02 | Server technology | Java 21 LTS + Spring Boot (supported compatible release at implementation), Spring Security; MySQL Connector/J; JavaFX on Java 21 LTS. |
| A-03 | Hosting | App service and MySQL isolated on one Windows Server host for school-scale initial deployment; separate processes/accounts and loopback DB listener; separation into two hosts possible without client changes. |
| A-04 | DB engine | Supported MySQL 8.4 LTS or newer compatible LTS, InnoDB, utf8mb4, strict SQL mode, UTC storage, time zone conversion in UI. |
| A-05 | Accounts | Individual accounts, no shared credentials, salted Argon2id or BCrypt hashes, temporary lockout after repeated failures, audited account recovery. |
| A-06 | Authentication | Server-managed expiring sessions, 30-minute idle timeout; reauthentication for sensitive changes; TLS certificate trusted by managed clients. |
| A-07 | Money | DECIMAL(15,2) in configured local currency; no floating point for money; rounding HALF_UP only at defined charge/receipt boundaries. Currency and fee schedule configured by school. |
| A-08 | Grade scale | Per-assessment max points, 0 ≤ score ≤ max; aggregate rules configured per academic term; approval freezes snapshot; corrections create versioned revisions. |
| A-09 | Retention | No routine hard delete of students, grades, payments, audit events; deactivation and archival. Legal retention period remains institutional decision. |
| A-10 | Backup | Daily encrypted backup; 30 daily restore points, one weekly off-host copy; monthly restore drill; initial target RPO 24h and RTO 8h (targets, not guaranteed). |
| A-11 | Availability | No offline write queue. On connectivity loss show explicit failure/unknown outcome; request idempotency prevents duplicate payments. |
| A-12 | Concurrency | Transactions with row locks for financial allocation, grade finalization and section transfer; unique constraints and optimistic versioning where useful. |
| A-13 | Reporting | Server generates authorized, filtered PDF/XLSX; no raw unrestricted SQL or export of hidden data. |
| A-14 | UI | Arabic RTL and English LTR; accessibility-friendly labels, form validation, error states; role-filtered navigation is supplemental to server authorization. |
| A-15 | Academic structure | Two numbered terms per academic year, at most one active year; subjects/sections configurable, historical enrollments retained. |

## C. Interpretation
The user authorized reasonable assumptions to finish **the design section**. These assumptions establish a coherent **design baseline**, not evidence that source requirements have been verified or software tested. Deviations during implementation require an ADR and corresponding diagram updates.
