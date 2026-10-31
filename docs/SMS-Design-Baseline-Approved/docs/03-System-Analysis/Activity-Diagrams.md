# Activity Diagrams — School Management System

## Purpose
These diagrams describe user-facing workflows, decisions, exceptions and data consistency checks, aligned with the desktop-only system architecture.

## Diagram catalog

### Student Registration (UC-02)

![Student Registration (UC-02)](diagrams/AD-01-Student-Registration.png)

### Grade Entry and Finalization (UC-07 / UC-08)

![Grade Entry and Finalization (UC-07 / UC-08)](diagrams/AD-02-Grade-Entry-and-Finalization.png)

### Payment Registration (UC-10)

![Payment Registration (UC-10)](diagrams/AD-03-Payment-Registration.png)

### Report Generation (UC-13)

![Report Generation (UC-13)](diagrams/AD-04-Report-Generation.png)

## Consistency notes
- Students are registered independently of enrollment/section assignment.
- Grade finalization locks approved grades; approval authority must be confirmed in RBAC.
- Payments cannot exceed outstanding balances and allocation is atomic.
- Reports enforce access permissions and filter data accordingly.
