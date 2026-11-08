# Architecture Decision Records (ADRs)

An ADR records one important design decision: why we need it, what we decided, what else we looked at, and what follows from it. These ADRs are a **draft proposal** for the Warehouse project. Status **Proposed** means the team has not accepted it yet. Change it to **Accepted** after the team agrees.

| ADR | Decision | Status | Main features |
|---|---|---|---|
| [ADR-001](ADR-001-modular-monolith.md) | Use a modular monolith | Proposed | All features |
| [ADR-002](ADR-002-technology-stack.md) | Use Java 21, Spring Boot, PostgreSQL, and a React web app | Proposed | All features |
| [ADR-003](ADR-003-rest-api-conventions.md) | Use REST JSON API rules for every feature | Proposed | All features (API Requirements sections) |
| [ADR-004](ADR-004-data-ownership.md) | Each piece of data has exactly one owning feature | Proposed | All features (Data Ownership sections) |
| [ADR-005](ADR-005-inventory-service.md) | Change inventory only through one inventory service, in one transaction | Proposed | Features 9, 10, 15, 16, 17, 20, 22 |
| [ADR-006](ADR-006-role-based-access-control.md) | Control access by employee role, checked on the server | Proposed | All features (role rules), Feature 6 |
| [ADR-007](ADR-007-soft-delete.md) | "Delete" means set inactive (soft delete) | Proposed | Features 1–7, 9, 11, 12 |
| [ADR-008](ADR-008-audit-log.md) | Write an audit log entry for every change | Proposed | All features that change data |
| [ADR-009](ADR-009-monthly-tasks-computed.md) | Compute monthly tasks when they are read | Proposed | Features 19, 20, 21, 22 |
| [ADR-010](ADR-010-read-only-reports.md) | Reports are read-only queries on live data | Proposed | Feature 23 |
| [ADR-011](ADR-011-supplier-order-document.md) | Supplier orders are printed or faxed from a PDF | Proposed | Feature 11, Feature 4 |
| [ADR-012](ADR-012-barcode-scanner-input.md) | Use keyboard-style barcode scanners | Proposed | Features 10, 13, 15, 16, 20 |
| [ADR-013](ADR-013-docker-and-ci.md) | Build and run with Docker and a CI pipeline | Proposed | All features (Definition of Done) |
| [ADR-014](ADR-014-test-strategy.md) | Test with the Gherkin scenarios of each feature | Proposed | All features |

See also: [Non-Functional Requirements](../NFR.md), [Architecture diagrams](../Arch_Diagrams/README.md), [UI screens](../UI/README.md).
