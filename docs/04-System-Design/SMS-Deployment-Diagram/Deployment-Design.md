# School Management System — Deployment Design

**Version:** 1.0  
**Status:** Proposed deployment baseline  
**Application:** Desktop-only, single private school

## 1. Purpose
Document the physical deployment of the SMS desktop application and its centralized relational database.

## 2. Nodes and artifacts
| Node | Multiplicity | Deployed artifact | Responsibility |
|---|---:|---|---|
| Desktop workstations | 1..N | SMS Desktop Client | Presentation, business logic and data access components |
| Central database server | 1 | SMS Database | Persistent relational data, constraints, transactions and auditing |
| Protected backup storage | Proposed | Database backups | Recoverability; placement and medium TBD |
| School LAN | 1 | Network communication path | Connectivity between clients and database server |

User roles (principal, administrator, administrative staff, teacher, accountant) are *logical access roles*, not necessarily separate computers. The workstation examples in the diagram illustrate possible deployments.

## 3. Communication
- Clients connect to the central database using the DBMS-supported protocol over the school LAN.
- Connections must be authenticated, encrypted where supported, and restricted by firewall/network access rules.
- Use least-privilege database credentials; end-user authorization must be enforced by the application business logic, not by hiding UI controls.
- Database writes are transactional, especially grade finalization, section transfers, payment allocation and financial corrections.
- Connectivity failures must not produce false success messages or partial committed business operations.

## 4. Logical versus physical layers
The three logical layers (Presentation, Business Logic, Data Access) run within the desktop client in the current architecture. The database server is a separate physical node. **Three-layer does not imply a three-tier deployment.** No web/application server or mobile client is in scope.

## 5. Operational and security requirements
- Restrict direct database access to authorized desktop endpoints.
- Protect data in transit, at rest, and in backups according to the selected DBMS and school policy.
- Configure centralized audit logging, backups, recovery procedures and monitoring.
- Plan for multiple simultaneous desktop sessions and database transaction isolation.
- Avoid embedding administrative database credentials in distributable client binaries.
- Review direct-to-database desktop architecture for credential exposure and deployment risks before production; a service-based middle tier would require a separately approved architecture change.

## 6. Pending decisions
1. Desktop operating system and UI implementation technology.
2. Relational DBMS and database server operating system.
3. Database connection authentication and secrets distribution approach.
4. LAN addressing, firewall rules and endpoint inventory.
5. Backup storage location, schedule, retention and restoration objectives.
6. High availability and server sizing, if required.

## 7. Diagram files
- [Deployment Diagram PNG](diagrams/Deployment-Diagram.png)
- [Deployment Diagram SVG](diagrams/Deployment-Diagram.svg)
- [Deployment Diagram PDF](diagrams/Deployment-Diagram.pdf)
- [Editable Graphviz Source](diagrams/Deployment-Diagram.dot)

## 8. Related documents
- [System Architecture](System-Architecture.md)
- [Database Design](Database-Design.md)
- [Interface Design](Interface-Design.md)
- [Non-Functional Requirements](../02-Requirements-Analysis/Non-Functional-Requirements.md)

**Note:** This is a proposed UML-style deployment view. Physical devices and implementation technologies remain to be approved.
