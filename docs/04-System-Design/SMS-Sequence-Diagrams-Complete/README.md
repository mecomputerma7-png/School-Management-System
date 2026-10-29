# SMS — Complete Sequence Diagrams

Eight UML sequence diagram scenarios for the desktop-only School Management System.

## Contents

| ID | Scenario | Traceability |
|---|---|---|
| SD-01 | User Authentication | UC-01 |
| SD-02 | Student Registration | UC-02 |
| SD-03 | Section Transfer | Student section assignment |
| SD-04 | Grade Entry | UC-07 |
| SD-05 | Grade Finalization | UC-08 |
| SD-06 | Payment Registration | UC-10 |
| SD-07 | Financial Correction | UC-11 |
| SD-08 | Report Generation | UC-13 |

Each scenario includes PNG, SVG, editable PlantUML (.puml), and editable Mermaid (.mmd).

## Design conventions

- Desktop UI → Business Service → Repository → Central relational database.
- Permission and cross-record validation are enforced in business services, not only in UI.
- Financial and academic state-changing operations require atomic, concurrency-safe transactions.
- Alternative outcomes are indicated as notes in visual exports; editable sources can be refined to formal UML combined fragments.
- Financial correction accounting mechanics remain subject to approved physical implementation policy.

## GitHub location

`docs/04-System-Design/diagrams/Sequence-Diagrams/`

## Related documents

- [Use Case Descriptions](../../03-System-Analysis/Use-Case-Descriptions.md)
- [Business Rules](../../03-System-Analysis/Business-Rules.md)
- [Database Design](../Database-Design.md)
- [System Architecture](../System-Architecture.md)
