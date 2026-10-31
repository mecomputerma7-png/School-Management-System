# Deployment Design — Revised Baseline

**Status:** Updated to reflect approved three-tier architecture (2026-10-30).

## Nodes
1. **Windows desktop workstations (10–30 concurrent users):** JavaFX client, no database credentials.
2. **Windows Server — Java Application Server:** authentication, configurable RBAC, business services, validation, audit, transaction orchestration, REST-style HTTPS API (framework not yet chosen).
3. **Windows Server — MySQL Database:** restricted to application-server connectivity; JDBC from server only.
4. **Protected backup storage:** automated daily backup; restore testing and retention policy still to be defined.

The application and MySQL tiers may be deployed on isolated services on one host or separate hosts; final hardware topology is pending. This is a three-tier deployment even when tiers share hardware.

## Security and connectivity
- JavaFX ↔ Java server: HTTPS over LAN.
- Java server ↔ MySQL: JDBC, least-privilege account, protected network and TLS as configured.
- Client → MySQL: **prohibited**.
- System Administrator alone may edit role permissions.
- LAN outages stop operations; no offline write queue.

## Diagram
[Deployment Diagram](diagrams/Deployment-Diagram-Revised.svg)
