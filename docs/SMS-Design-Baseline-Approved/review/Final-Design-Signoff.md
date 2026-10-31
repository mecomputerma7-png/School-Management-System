# SMS — Design Section Completion Statement
**Date:** 2026-10-30 | **Version:** 1.0

## Decision
**Academic design baseline: COMPLETE WITH DOCUMENTED ASSUMPTIONS.** This means scope, architecture, data integrity approach, security model, UI direction, deployment model, and testable acceptance criteria are defined sufficiently to begin detailed implementation.

**Not claimed:** source-verified compliance with all 69 FRs, validated ERD SQL constraints, fully synchronized old UML drawings, working application, tested backups, or production certification. The provided original source requirements are unavailable in the working set; the RTM remains explicitly unverified.

## Priority before coding
1. Produce physical MySQL DDL from ERD, including composite integrity constraints.
2. Publish REST API contract and generate/align eight sequence diagrams.
3. Align JavaFX/server class diagram and permission checks with RBAC matrix.
4. Convert baseline acceptance scenarios into executable tests and finish source-based RTM when originals are available.

## Rule for conflicting legacy files
**User-approved decisions > Approved-Assumptions-and-Decisions.md > Integrity-and-Transaction-Rules.md > revised architecture/deployment documents > older draft diagrams.** No unrecorded design choices should be inferred from superseded diagrams.
