# Architecture Decision Records (ADRs)

Why we chose a design that more than one feature depends on.

| Artifact | Question |
|----------|----------|
| **ADRs** (`docs/adr/`) | Why did we choose this approach? |
| [Feature specs](../../features/) | What must the product do? |
| [Quality attributes](../nfr/quality-attributes.md) | What quality bars apply across the product? |
| [C4 diagrams](../arch_diagrams/README.md) | What does the system look like? |

ADRs do not replace feature specs. They record the context, the decision, and the alternatives.

## File naming

```
docs/adr/
  README.md                  ← this file
  NNNN-short-kebab-title.md  ← one decision per file
```

- **Number:** four digits, sequential (`0001`, `0002`, …). The heading inside the file still says ADR-001, ADR-002, and so on.
- **Title:** short, specific, kebab-case.
- **Status:** `Proposed` | `Accepted` | `Deprecated` | `Superseded by ADR-NNNN`

## Index

| ADR | Title | Status |
|-----|-------|--------|
| [0001](./0001-modular-monolith.md) | Use a modular monolith | Proposed |
| [0002](./0002-technology-stack.md) | Java 21, Spring Boot, PostgreSQL, and a React web app | Proposed |
| [0003](./0003-rest-api-conventions.md) | REST API conventions | Proposed |
| [0004](./0004-data-ownership.md) | Each piece of data has exactly one owning feature | Proposed |
| [0005](./0005-inventory-service.md) | Change inventory only through one inventory service | Proposed |
| [0006](./0006-role-based-access-control.md) | Role-based access control | Proposed |
| [0007](./0007-soft-delete.md) | Soft delete | Proposed |
| [0008](./0008-audit-log.md) | Write an audit log entry for every change | Proposed |
| [0009](./0009-monthly-tasks-computed.md) | Compute monthly tasks when they are read | Proposed |
| [0010](./0010-read-only-reports.md) | Reports are read-only queries on live data | Proposed |
| [0011](./0011-supplier-order-document.md) | Supplier orders are printed or faxed from a PDF | Proposed |
| [0012](./0012-barcode-scanner-input.md) | Keyboard-style barcode scanners | Proposed |
| [0013](./0013-docker-and-ci.md) | Build and run with Docker and a CI pipeline | Proposed |
| [0014](./0014-test-strategy.md) | Test with the Gherkin scenarios of each feature | Proposed |
