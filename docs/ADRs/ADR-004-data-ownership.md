# ADR-004: Each piece of data has exactly one owning feature

**Status:** Proposed
**Date:** 2026-11-06
**Related features:** All features (Data Ownership sections)

## Context

Many features use the same data. For example, the slot is used by Features 2, 9, 10, 14, 16, and 20. If every feature wrote to every table, the rules (for example "a slot is Occupied only when it holds stock") would be broken in many places.

## Decision

Every entity has **one owning feature**. The owner is the only code that writes the entity's tables. Other features read the data through the owner's service, and ask the owner to change it. Each feature file has a **Data Ownership** table that shows what it owns, what it reads, and what it updates through the owner.

Examples: Feature 2 owns Warehouse and Slot. Feature 9 owns Inventory Record. Feature 11 owns Supplier Order. Feature 19 owns Inventory Count and Count Line. Feature 21 owns Removal List. The audit log belongs to the platform (ADR-008).

## Alternatives considered

- **Shared tables, free access:** faster to write at first, but hard to keep rules correct.
- **Separate service per entity:** too heavy for this system (see ADR-001).

## Consequences

- Rules are written once, in one place.
- Some features (13, 14, 20, 22, 23) own no data, or only read it.
- Cross-feature calls add a little code (a service interface), but they make the rules testable.

---

[Back to the ADR index](README.md)
