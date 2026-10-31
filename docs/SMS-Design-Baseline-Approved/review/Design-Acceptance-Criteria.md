# Design Acceptance Criteria and Test Scenarios

| ID | Acceptance criterion | Evidence expected during implementation |
|---|---|---|
| AC-01 | Desktop client has no DB connection string or credentials | Inspect packaged client and network trace |
| AC-02 | Server denies unauthorized endpoint requests | Negative API tests for all RBAC actions |
| AC-03 | Only System Administrator changes role permissions | Permission test for five roles |
| AC-04 | Teacher sees/edits only assigned grade records | Scope tests with two sections |
| AC-05 | Grade finalization requires Principal or explicit delegate | Authorization tests and audit check |
| AC-06 | Grade corrections retain approved version | DB revision history test |
| AC-07 | Section transfers preserve history, no overlaps | Concurrency and rollback tests |
| AC-08 | Exactly two terms and at most one active academic year | Schema/service validation tests |
| AC-09 | Payment allocations equal payment and never exceed outstanding charge | Positive/negative DB transaction tests |
| AC-10 | Financial corrections preserve original entries | Audit/reversal consistency test |
| AC-11 | Duplicate payment request is idempotent | Network retry test |
| AC-12 | Network outage produces no false success or offline commit | LAN disconnect test |
| AC-13 | Daily backup can be restored | Documented restore drill |
| AC-14 | Arabic RTL / English LTR screens work | UI review and input tests |
| AC-15 | PDF/XLSX exports enforce report scope | Role-based report tests |
| AC-16 | 10–30 concurrent users meet measured performance targets | Load test with recorded timings |

**Status:** Acceptance scenarios specified, not executed. Quantitative response-time targets must be set and measured during implementation.
