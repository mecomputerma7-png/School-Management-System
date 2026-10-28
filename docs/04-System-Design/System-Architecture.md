# School Management System
## System Architecture

**Document Version:** 1.0  
**Date:** October 2026  
**Status:** Proposed Design  
**Application Type:** Desktop Application

---

## 1. Purpose

This document defines the proposed software architecture of the School Management System (SMS).

It describes the system's structural organization, major components, communication mechanisms, data management approach, and deployment model.

The architecture provides a technical foundation for detailed database design, interface design, implementation, and testing.

## 2. Architectural Overview

The SMS is designed as a desktop-based application for a single private school.

The proposed architecture follows a **layered design**, separating presentation, business logic, and data access responsibilities.

The system supports multiple authorized users operating from school workstations and accessing a centralized relational database through the school's local area network (LAN).

### 2.1 Architectural Principles

The architecture is guided by the following principles:

- **Separation of Concerns:** Each layer has clearly defined responsibilities.
- **Modularity:** Functional modules remain logically independent while sharing common services.
- **Centralized Data Management:** Institutional records are maintained in a shared database.
- **Security by Design:** Authentication, authorization, and auditing are enforced across protected operations.
- **Data Integrity:** Business rules and database constraints protect institutional information.
- **Maintainability:** Components can be modified with limited impact on unrelated functionality.

## 3. Architecture Model

The system adopts a **Three-Layer Desktop Architecture**.

| Layer | Responsibility |
|---|---|
| Presentation Layer | Provides desktop interfaces and handles user interactions. |
| Business Logic Layer | Implements application operations, institutional rules, and access controls. |
| Data Access Layer | Manages database communication and persistence operations. |

The layers operate within the desktop application, while the relational database is hosted centrally.

### 3.1 Logical Architecture

```text
+--------------------------------------------------+
|               Desktop Application                |
|                                                  |
|  +--------------------------------------------+  |
|  |           Presentation Layer               |  |
|  |        Forms / Views / Navigation          |  |
|  +---------------------+----------------------+  |
|                        |                         |
|                        v                         |
|  +--------------------------------------------+  |
|  |          Business Logic Layer              |  |
|  |     Modules / Rules / Authorization        |  |
|  +---------------------+----------------------+  |
|                        |                         |
|                        v                         |
|  +--------------------------------------------+  |
|  |             Data Access Layer              |  |
|  |      Repositories / Database Access        |  |
|  +---------------------+----------------------+  |
+------------------------+-------------------------+
                         |
                         | LAN / Database Connection
                         v
             +--------------------------+
             | Centralized Relational   |
             |        Database          |
             +--------------------------+
```

This model represents the logical organization of the proposed system rather than a finalized technology stack.

## 4. Application Components

The application is divided into seven functional modules supported by shared infrastructure components.

### 4.1 Functional Modules

| Module | Primary Responsibility |
|---|---|
| Student Management | Student profiles, registration, and enrollment information. |
| Teacher Management | Teacher records and teaching assignments. |
| Class Management | Academic structures, class sections, and student assignments. |
| Grade Management | Grade entry, calculation, correction, and finalization. |
| Fee Management | Tuition structures, payment processing, and financial balances. |
| User Management | User accounts, roles, permissions, and account status. |
| Reports Management | Academic, administrative, and financial reporting. |

Each module communicates with shared services and accesses persistent data through the Data Access Layer.

### 4.2 Shared Components

| Component | Responsibility |
|---|---|
| Authentication Service | Validates user credentials and establishes authenticated sessions. |
| Authorization Service | Evaluates permissions before protected operations. |
| Validation Service | Enforces applicable input constraints and business rules. |
| Audit Service | Records sensitive operations and relevant changes. |
| Transaction Management | Coordinates database operations requiring atomic execution. |
| Reporting Service | Retrieves and prepares authorized report data. |
| Configuration Management | Maintains application and institutional settings. |

Shared components are logical responsibilities and may be implemented as separate services or internal application components.

## 5. Layer Responsibilities

### 5.1 Presentation Layer

The Presentation Layer provides the graphical desktop interface.

Its responsibilities include:

- Displaying application windows, forms, and reports.
- Collecting and validating basic user input.
- Presenting operation results and error messages.
- Providing navigation based on the user's authorized functions.

The Presentation Layer shall not execute database queries directly or independently enforce institutional policies.

### 5.2 Business Logic Layer

The Business Logic Layer coordinates application operations and applies institutional rules.

Its responsibilities include:

- Executing functional workflows.
- Enforcing business rules and authorization decisions.
- Validating relationships between institutional records.
- Calculating academic results and financial balances.
- Coordinating transactions across related records.

Business rules are defined in `../03-System-Analysis/Business-Rules.md`.

### 5.3 Data Access Layer

The Data Access Layer provides controlled access to persistent data.

Its responsibilities include:

- Executing database operations.
- Managing database connections.
- Mapping application entities to stored records.
- Supporting transactional consistency.
- Handling database-related exceptions.

Database operations shall use parameterized queries or equivalent safe data-access mechanisms.

## 6. Data Management Architecture

The system uses a centralized relational database to maintain consistent institutional records across authorized workstations.

### 6.1 Data Organization

The database supports the following logical data domains:

| Data Domain | Main Contents |
|---|---|
| Students | Student identities and enrollment records. |
| Teachers | Teacher profiles and assignments. |
| Academic Structure | Academic years, grade levels, subjects, and class sections. |
| Grades | Assessment records and academic results. |
| Finance | Tuition fees, charges, payments, and adjustments. |
| Access Control | User accounts, roles, and permissions. |
| Auditing | Records of security-relevant and sensitive operations. |

The physical database schema will be specified in `Database-Design.md`.

### 6.2 Data Integrity

Data integrity shall be maintained through:

- Primary and foreign key constraints.
- Unique identifiers and applicable uniqueness constraints.
- Referential integrity.
- Transactional operations.
- Application-level business validation.
- Controlled record status transitions.

Historical records shall be preserved according to approved retention and correction policies.

## 7. Security Architecture

Security controls are applied across the application and database.

### 7.1 Authentication

Users must authenticate before accessing protected functionality.

Credentials shall be verified using secure authentication mechanisms. Passwords shall not be stored in plaintext.

### 7.2 Authorization

The system uses **Role-Based Access Control (RBAC)**.

The five primary user roles are:

1. School Principal
2. System Administrator
3. Administrative Staff
4. Teacher
5. Accountant

Permissions shall be enforced in the Business Logic Layer rather than relying solely on hidden or disabled interface controls.

### 7.3 Database Protection

Database access shall use restricted application credentials.

Sensitive connection settings shall not be hardcoded or exposed through the user interface.

Where supported, database connections shall use encrypted communication.

### 7.4 Auditing

The system shall record security-relevant events and sensitive modifications according to the approved audit requirements.

Audit records shall identify the responsible user, affected operation, and event time.

## 8. Deployment Architecture

The proposed deployment model uses multiple desktop workstations connected to a centralized database server through the school's LAN.

### 8.1 Deployment Components

| Component | Description |
|---|---|
| Desktop Workstations | Run the SMS application for authorized school personnel. |
| Local Area Network | Connects application workstations to the database server. |
| Database Server | Hosts the centralized relational database. |
| Backup Storage | Stores protected database backups according to the approved backup policy. |

### 8.2 Deployment Model

```text
+--------------------+      +--------------------+
| Administration PC  |      |    Teacher PC      |
| SMS Desktop App    |      | SMS Desktop App    |
+---------+----------+      +----------+---------+
          |                            |
          +-------------+--------------+
                        |
                        |
                School LAN
                        |
          +-------------+--------------+
          |                            |
+---------+----------+      +----------+---------+
| Accountant PC      |      | Principal PC       |
| SMS Desktop App    |      | SMS Desktop App    |
+--------------------+      +--------------------+
                        |
                        v
              +----------------------+
              | Central Database     |
              | Server               |
              +----------+-----------+
                         |
                         v
              +----------------------+
              | Protected Backup     |
              | Storage              |
              +----------------------+
```

The diagram illustrates the intended deployment concept. Final network configuration and infrastructure specifications will be determined during implementation planning.

## 9. Communication and Data Flow

Application operations follow a controlled interaction sequence:

1. A user initiates an operation through the desktop interface.
2. The Presentation Layer forwards the request to the Business Logic Layer.
3. The Business Logic Layer verifies authorization and applicable rules.
4. The Data Access Layer performs the required database operations.
5. The result returns through the Business Logic Layer to the Presentation Layer.
6. The interface presents the outcome to the user.

The desktop application communicates with the centralized database through the school's LAN.

Direct access from the Presentation Layer to the database is prohibited by the logical architecture.

## 10. Reliability and Recovery

The architecture shall support operational reliability through:

- Database transaction management.
- Controlled error handling.
- Centralized backup procedures.
- Database recovery mechanisms.
- Connection failure detection.
- Consistent handling of concurrent data modifications.

If database connectivity is unavailable, operations requiring persistent data shall not be considered successfully completed.

Offline operation and local data synchronization are not included in the current architectural scope.

## 11. Architectural Decisions

| Decision ID | Decision | Rationale |
|---|---|---|
| AD-001 | Desktop-only application | Aligns implementation with the current project scope. |
| AD-002 | Three-layer logical architecture | Separates interface, business logic, and persistence responsibilities. |
| AD-003 | Centralized relational database | Supports consistent shared records across school workstations. |
| AD-004 | LAN-based database connectivity | Enables multi-user operation within the school. |
| AD-005 | Role-Based Access Control | Restricts operations according to institutional responsibilities. |
| AD-006 | Modular functional organization | Improves maintainability and separates major business domains. |
| AD-007 | Centralized backup strategy | Supports institutional data protection and recovery. |

## 12. Technology Decisions Pending

The following implementation technologies remain open for evaluation.

| Area | Pending Decision |
|---|---|
| Programming Language | Select the desktop application development language. |
| Desktop UI Framework | Select the graphical interface framework. |
| Database Management System | Select the relational DBMS. |
| Data Access Technology | Select the database connectivity and persistence approach. |
| Reporting Technology | Select the report generation and export tools. |
| Packaging and Installation | Define application distribution and update procedures. |

Technology selection shall consider compatibility, development complexity, maintainability, available resources, and project requirements.

## 13. Architectural Constraints

The proposed architecture is subject to the following constraints:

- The system serves a single school.
- Application access is provided through desktop workstations.
- The initial deployment depends on the school's local network.
- The database is centralized.
- Multi-school operation is excluded.
- Web and mobile clients are excluded.
- Offline synchronization is excluded.
- Institutional records must remain protected against unauthorized access and inconsistent modifications.

## 14. Related Documents

| Document | Relationship |
|---|---|
| `../02-Requirements-Analysis/SRS.md` | Defines the overall system requirements and boundaries. |
| `../02-Requirements-Analysis/Functional-Requirements.md` | Defines required application capabilities. |
| `../02-Requirements-Analysis/Non-Functional-Requirements.md` | Defines quality, security, and operational requirements. |
| `../03-System-Analysis/Business-Rules.md` | Defines institutional rules enforced by the application. |
| `../03-System-Analysis/Process-Analysis.md` | Defines operational processes and information dependencies. |
| `Database-Design.md` | Will define the database schema and relationships. |
| `Interface-Design.md` | Will define desktop screens and user interactions. |

## 15. Conclusion

The proposed SMS architecture establishes a modular, three-layer desktop application supported by a centralized relational database.

It separates user interaction, institutional business logic, and data persistence while supporting controlled access, shared records, and reliable multi-user operation.

The architecture provides a practical foundation for detailed database and interface design without committing prematurely to specific implementation technologies.

---

**End of Document**
