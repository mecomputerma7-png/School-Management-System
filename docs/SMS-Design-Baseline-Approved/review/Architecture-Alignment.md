# Alignment Checklist — Approved Three-Tier Architecture

**Canonical request path:** JavaFX View → API Client (HTTPS) → REST Controller → Authorization + Application Service → Repository/JDBC → MySQL. All writes, audit records and multi-row changes occur server-side.

| Diagram/document | Required alignment | Baseline status |
|---|---|---|
| System Architecture | JavaFX / Java server / MySQL tiers | Approved decision, document updated |
| Deployment | Clients reach API only; DB protected behind app server | Revised in integrated package |
| ERD | 25 logical entities; FK and time consistency; fee allocations | Logical reference, physical DDL pending |
| Class Diagram | Client controllers distinct from server services and repositories | Existing graphic preliminary; this document controls interpretation |
| SD-01…08 | Every persistent action crosses HTTPS and server authorization; financial writes transactional | Existing graphics preliminary; apply canonical path |
| DFD Context/Level 0 | Same external flows across boundary; seven functional processes | Flow-by-flow balancing not independently certified |
| Activity Diagrams | Server validates, commits or rolls back, then acknowledges | Existing activity images illustrative |
| UI | 18 screens, bilingual, role-scoped, no offline editing | Composite reference only; editable screen files pending |
| RTM | 69 requirement IDs, mappings only when supported by original text | Placeholder; not verified |

**Do not present the inherited class/sequence/DFD diagrams as source-verified or fully reconciled.** For academic delivery they are usable *reference diagrams*, with this approved decision register as precedence when they differ. The final implementation should re-render affected diagrams after API and schema are frozen.
