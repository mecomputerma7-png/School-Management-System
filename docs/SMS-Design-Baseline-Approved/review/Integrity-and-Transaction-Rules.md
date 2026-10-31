# Integrity and Transaction Rules — Design Baseline

## Student registration
Validate mandatory fields and unique student identifier; create student atomically; return stable ID. Enrollment/section placement is a separate explicit step, not an implicit side effect.

## Section transfer
Validate student enrollment, destination section capacity and academic term; acquire lock on active section assignment; close old assignment and open new assignment in one transaction; prevent overlapping effective dates and preserve historical records. If transaction fails, neither update persists.

## Grade entry and approval
Verify teacher's section/subject assignment, assessment max score and current term. Grade entry is editable only until finalization. Authorized Principal/delegate finalizes an immutable version; corrections after finalization are linked, approved revisions with old/new values, reason, actor and timestamp. Never silently overwrite approved history.

## Payment registration
The Accountant selects eligible outstanding charges and allocation amounts. Under one DB transaction, lock charges, verify all amounts positive and `sum(allocations) == payment_amount`, verify allocations do not exceed each charge's remaining balance; reject overpayment. Insert payment and allocation records, write audit event and commit. Unique idempotency key avoids duplicate posting after retries.

## Financial correction
Never UPDATE/DELETE original posted payment or allocation. Authorized Accountant submits reason and reference; server locks impacted balances, writes compensating reversal/adjustment entry and new allocations (if applicable) in one transaction; prevent negative balance or excess credit. Store correlation ID and audit event. Repeat correction request with same idempotency key returns existing result.

## Data consistency
StudentGrade's EnrollmentID and SectionAssignmentID, if both retained, must refer to the same student and academic period; enforce with server transaction and suitable composite FKs/unique keys where feasible. AcademicTerm is exactly 1 or 2 per academic year, and only one year is active. Financial columns use DECIMAL, never float.

## Operational failure
If server response is lost after commit, client queries by request/idempotency ID instead of posting again. Backup and restore testing is mandatory before real deployment.
