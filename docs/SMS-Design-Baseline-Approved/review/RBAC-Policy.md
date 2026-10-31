# RBAC — Role Permissions and Enforcement
**Status:** Adopted baseline assumption, subject to school operational validation.

See [RBAC-Permission-Matrix.csv](RBAC-Permission-Matrix.csv). `Y`=granted by default, `N`=denied, `Assigned`=only records linked to the teacher's current authorized assignment, `Fees only`=minimal identity/fee information, `Delegated`=explicit permission granted by System Administrator.

1. Default deny for every permission not listed. Principal does **not** edit role permissions.
2. System Administrator controls account access and permission configuration, but has no implicit right to edit grades, fees or payments; separate delegation is required for grade approval.
3. Financial corrections by Accountant require reason, source reference and audit; for separation of duties in production, a second reviewer is recommended, but not assumed mandatory for this school.
4. Teacher may enter grades only for assigned subject/section/term and cannot finalize own entries by default.
5. Every protected API endpoint checks action permission plus record scope. UI visibility is not a security boundary.
6. Permission changes and delegated grade approval are audited with actor, timestamp, old/new values and reason.
7. An account can have multiple roles; effective permissions are union of explicitly assigned grants unless an explicit deny rule applies. Least privilege governs initial assignments.
