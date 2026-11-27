# ADR-001: Use a modular monolith

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** All features

## Context

The Warehouse system has 23 features that share the same data: items, slots, inventory, and orders. One team (a student team) will build it, and the warehouse has about 50 users. We need to change inventory and slot status together in one step, and we need to keep the system simple to build, test, and run.

## Decision

We build one application (a modular monolith) with one database. The code is split into six modules: **Master Data** (Features 1–7), **Inbound** (8, 10, 11), **Inventory** (9, 13, 14, 16), **Outbound** (12, 15, 17, 18), **Stock Control** (19–22), and **Reporting** (23). A module may call another module only through its public service interface, never through its tables (see ADR-004).

## Alternatives considered

- **Microservices:** each service has its own database, so one change to inventory and a slot would need distributed transactions. This is too complex for the size of the system.
- **One big unstructured application:** simple at the start, but any feature could change any table, and rules like "only Feature 9 changes quantity" would break.

## Consequences

- One database transaction can change inventory, slot status, and the audit log together (ADR-005).
- One deployment, one log, one backup.
- The module boundaries must be checked by tests (for example an architecture test that blocks direct table access across modules).
- If one module needs to scale later, it can be split out, because the boundaries are already clean.

---

[Back to the ADR index](README.md)
