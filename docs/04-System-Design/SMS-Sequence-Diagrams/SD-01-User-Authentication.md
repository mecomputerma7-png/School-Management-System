# SD-01 — User Authentication Sequence Diagram

**System:** School Management System (SMS)  
**Related Use Case:** UC-01 — Authenticate User  
**Application:** Desktop-only; centralized database over school LAN  
**Status:** Proposed detailed design

## Purpose
Describe the interaction between the user, desktop login view, authentication service, data access layer, and central database during sign-in.

## Participants
| Participant | Responsibility |
|---|---|
| School User | Submits credentials. |
| `LoginView` | Checks required inputs and displays the result. |
| `AuthenticationService` | Verifies credentials and account status; establishes authorized session. |
| `UserRepository` | Retrieves account and access information. |
| Central Database | Stores accounts, password hashes, roles, and permissions. |

## Main Scenario
1. The user enters a username and password and selects **Login**.
2. `LoginView` checks that mandatory fields are present.
3. `AuthenticationService` retrieves the account through `UserRepository`.
4. The service verifies the password against the stored hash and checks account status.
5. For a valid active account, the service retrieves roles and permissions.
6. The service creates an application session context, and the desktop application opens the authorized dashboard.

## Alternative Flows
- **Missing inputs:** The login view displays field-level validation without querying the database.
- **Unknown account, disabled account, or incorrect password:** Authentication fails with a generic error message; no session is established.
- **Database unavailable:** The system reports that sign-in could not be completed, without treating it as an authentication success.

## Design Constraints
- Store password hashes, never plaintext passwords.
- Never return stored password hashes to the UI.
- Apply RBAC authorization in business logic, not solely through hidden UI controls.
- Do not reveal whether a username exists through login errors.
- Session lifetime, failed-login rate limits, and password policy are governed by the approved security requirements and must be finalized for implementation.

## Diagram Source
- [PlantUML source](diagrams/SD-01-User-Authentication.puml)
- [Mermaid source](diagrams/SD-01-User-Authentication.mmd)
- [SVG diagram](diagrams/SD-01-User-Authentication.svg)

## Related Documents
- [Use Case Descriptions](../03-System-Analysis/Use-Case-Descriptions.md)
- [Business Rules](../03-System-Analysis/Business-Rules.md)
- [System Architecture](../04-System-Design/System-Architecture.md)
- [Database Design](../04-System-Design/Database-Design.md)
- [Interface Design](../04-System-Design/Interface-Design.md)
