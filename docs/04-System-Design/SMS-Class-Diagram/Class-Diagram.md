# School Management System — Class Diagram

**Status:** Proposed detailed design · **Application:** Desktop-only · **Version:** 1.0

## Purpose

This design describes the principal software classes and responsibilities across the Presentation, Business Logic, and Data Access layers. Domain classes are logical business objects, not necessarily one-to-one mappings to database tables. All desktop clients use a centrally managed relational database over the school LAN.

## Architectural responsibilities

| Layer | Representative classes | Responsibility |
|---|---|---|
| Presentation | LoginView, StudentView, GradeEntryView, PaymentView, ReportsView | Collect input, present output, and call authorized services. |
| Business Logic | AuthenticationService, StudentService, AcademicService, GradeService, PaymentService, ReportService | Validate business rules, enforce permissions, and coordinate transactions. |
| Data Access | StudentRepository, AcademicRepository, GradeRepository, PaymentRepository, UserRepository | Persist and retrieve records through parameterized database operations. |
| Domain | Student, Enrollment, SectionAssignment, SectionSubject, AssessmentComponent, StudentGrade, StudentCharge, Payment, PaymentAllocation | Model institutional concepts and relationships. |

## UML diagrams

- [Overall Layered Class Diagram](diagrams/Class-Diagram-Overview.svg)
- [Student and Academic Domain](diagrams/Class-Diagram-Students.svg)
- [Grade Management Domain](diagrams/Class-Diagram-Grades.svg)
- [Fee Management Domain](diagrams/Class-Diagram-Fees.svg)
- [Editable Graphviz Sources](diagrams/)

## Key design decisions

1. Views never query the database directly; they invoke services.
2. Services enforce RBAC independently of whether controls are hidden in the UI.
3. Student enrollment and section assignment are separate domain concepts. Historical section assignments are retained; active periods cannot overlap.
4. Grade recording validates assessment, term, section membership, teacher assignment, and finalization status.
5. PaymentService records payments and their allocations in a single concurrency-safe transaction and rejects amounts above outstanding balances.
6. Financial corrections are append-only in principle; the precise reversal and allocation-restoration procedure remains a physical-design decision and must not be considered implemented by this diagram.
7. The diagram does not specify a programming language, framework, or DBMS.

## Related Documents

- [System Architecture](../System-Architecture.md)
- [Database Design](../Database-Design.md)
- [Interface Design](../Interface-Design.md)
- [Business Rules](../../03-System-Analysis/Business-Rules.md)

## Review status

These are proposed UML-style design diagrams, not a claim of an implemented codebase. The class inventory, method signatures, exception behavior, and persistence mappings should be finalized after the development stack is chosen.
