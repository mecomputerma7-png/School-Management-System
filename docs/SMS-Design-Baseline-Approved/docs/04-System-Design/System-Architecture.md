# System Architecture

**Project:** School Management System (SMS)  
**Status:** Updated design baseline — approved architectural decisions  
**Scope:** One private school; desktop application; school LAN

## 1. Purpose
This document defines the logical and physical architecture of the SMS. It supersedes earlier desktop-to-database and hybrid architecture assumptions.

## 2. Architectural Style
The SMS uses a **three-tier client–server architecture**:

1. **Presentation tier — JavaFX desktop clients.** Installed on authorized Windows workstations. Presents forms, performs usability checks, and communicates with the application server through authenticated HTTPS requests. It does not connect to MySQL.
2. **Application tier — Java application server.** Runs as a separate server-side service. Implements authentication, authorization, business rules, validation, transactions, auditing, and report generation. Exposes an internal versioned API. The precise Java server framework is **not yet selected**.
3. **Data tier — MySQL.** Central relational database accessed only by the application server using JDBC and least-privilege database credentials. Clients have no database credentials.

The application server and MySQL may be installed on separate servers or isolated services on one server, subject to resource and security assessment. The three **logical responsibilities** remain separate regardless of physical deployment.

## 3. Deployment Context
- **Client OS:** Windows desktop workstations.
- **Server OS:** Windows Server.
- **Network:** School LAN; 10–30 expected concurrent users.
- **Transport:** HTTPS/TLS from clients to application API; protected JDBC/TLS connection to MySQL where supported and configured.
- **Availability:** Online-only. On network/server outage, changes cannot be committed; the client reports the interruption and prevents misleading success messages.
- **Academic configuration:** One active academic year and two academic terms per year; historical years retained.
- **Language:** Arabic and English desktop UI.
- **Updates:** Signed/verified application releases distributed from a trusted internal source, with version compatibility checks.

## 4. Functional Modules
The application tier implements exactly seven modules:

| Module | Core responsibilities |
|---|---|
| Student Management | Student profiles, enrollments, section transfer history |
| Teacher Management | Teacher profiles and assignments |
| Class Management | Academic years, two terms, grades/sections, subjects and section offerings |
| Grade Management | Assessments, entry, finalization, audited corrections |
| Fee Management | Charges, payments, allocations, correction entries |
| User Management | Accounts, authentication, roles and permissions |
| Reports Management | Permission-filtered reporting, PDF/Excel exports |

Attendance, timetable, standalone examination management, payroll, library, transport, inventory, and portals are outside scope.

## 5. Identity, Roles, and Permissions
The five application roles are **School Principal**, **System Administrator**, **Administrative Staff**, **Teacher**, and **Accountant**. Authorization is enforced **on the server for every protected operation**, never solely by hiding UI controls.

- Only the **System Administrator** may edit role permissions.
- The Principal or an explicitly authorized delegate may finalize grades; the exact permission assignment is configured and audited.
- Sensitive grade and financial changes require server-side authorization and an immutable audit trail.
- Passwords are stored as modern salted password hashes, never in plaintext. API sessions use short-lived authenticated credentials and secure expiry/revocation controls.

A complete permission-by-action matrix remains a required design deliverable; the statements above are binding constraints, not a substitute for that matrix.

## 6. Main Request Flow
1. User signs in through JavaFX.
2. Desktop client sends credentials over HTTPS to the application API.
3. Server authenticates user and issues a scoped session/token.
4. For each action, server verifies identity and permissions, validates input and business rules.
5. Application service opens a database transaction when a multi-record operation requires atomicity.
6. Data access layer executes parameterized JDBC operations against MySQL.
7. Server commits on success or rolls back on failure, records required audit events, and returns a structured response.
8. JavaFX shows a confirmed result only after server acknowledgment.

## 7. Data Integrity Rules
- Prevent overlapping active section assignments for the same student.
- Grade entry is constrained by the configured term, section subject, assessment and teacher authorization.
- Finalized grades cannot be silently overwritten; corrections follow an authorized audited workflow.
- Payment allocations must not exceed the recorded payment or outstanding eligible charges; **overpayments are rejected** under the approved design.
- Financial corrections preserve original records and create linked correction entries.
- Historical records are retained according to business rules; no routine permanent deletion of transactional history.
- Multi-row financial and grade operations are atomic and concurrency-safe, with database constraints supplementing application validation.

## 8. Security and Operations
- Firewall: only the application API is reachable from client VLANs; MySQL is reachable only from the application tier/authorized administrators.
- Least-privilege database account and protected secrets outside source control.
- Server-side input validation, prepared statements, centralized error handling, and audit logging.
- Daily automated database backups, periodic restore testing, and access-restricted backup storage. Backup retention and recovery objectives require operational approval.
- Health checks, log rotation, time synchronization, and controlled release/rollback procedures.
- No direct remote Internet exposure is required for the initial LAN-only deployment.

## 9. Architectural Decisions
| ID | Decision | Status |
|---|---|---|
| ADR-01 | Java + JavaFX desktop presentation | Approved |
| ADR-02 | Separate Java application server | Approved |
| ADR-03 | MySQL central database via JDBC from server only | Approved |
| ADR-04 | Windows Server hosting | Approved |
| ADR-05 | Online-only operation; 10–30 concurrent users | Approved |
| ADR-06 | Configurable RBAC; only System Administrator edits permissions | Approved |
| ADR-07 | Two academic terms per year; one active year | Approved |
| ADR-08 | Arabic/English UI; PDF/Excel reports | Approved |
| ADR-09 | Internal verified desktop updates; daily backups | Approved |

## 10. Open Implementation Choices
These are not blockers for the architectural baseline but must be chosen before coding/deployment:

- Java application server framework and API specification (e.g., REST JSON contracts).
- MySQL supported release and JDBC driver version.
- Hosting arrangement (one physical host vs. separate application and database servers).
- API session/token implementation, certificate management and key rotation.
- Backup retention, recovery point/time objectives, and deployment monitoring thresholds.
- Exact permission matrix and full error/exception contracts.

## 11. Design Consistency Updates Required
1. **Deployment Diagram:** JavaFX clients → Java application server → MySQL; no client-to-DB link.
2. **Sequence Diagrams:** every DB operation passes through application service and repository; add server-side authorization and transactions.
3. **Class Diagram:** distinguish JavaFX controllers/view models, application services, repositories, and domain entities.
4. **Interface Design:** bilingual text, role-filtered menus, network error states and session expiry.
5. **Database Design/ERD:** verify student grade-to-assignment FK, payment allocation and correction history against the final schema.
6. **SRS/NFR:** align security, availability and deployment requirements with online-only three-tier architecture.

## 12. Related Documents
- [Database Design](../04-System-Design/Database-Design.md)
- [Interface Design](../04-System-Design/Interface-Design.md)
- [Functional Requirements](../02-Requirements-Analysis/Functional-Requirements.md)
- [Non-Functional Requirements](../02-Requirements-Analysis/Non-Functional-Requirements.md)
- [Business Rules](../03-System-Analysis/Business-Rules.md)

**Diagram:** [Architecture Diagram](diagrams/Architecture-Diagram.svg)
