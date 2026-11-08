# ADR-005: Change inventory only through one inventory service, in one transaction

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** Features 9, 10, 15, 16, 17, 20, 22

## Context

Quantity on hand and slot status must always agree. Many features change inventory: Put Away (10), Move (16), Ship (17), Count (20), Remove Expired (22), and hand corrections (9). Two people can work at the same time on the same stock.

## Decision

Only Feature 9's **inventory service** changes `quantity_on_hand`. Other features call `adjust(recordId, delta, reason, source)` or `create(...)`.

The service, in one database transaction:
1. locks the record (version check),
2. checks that the new quantity is not negative,
3. saves the change (and writes an inventory change entry: who, when, old and new quantity, reason, source feature),
4. asks Feature 2 to set the slot status (Occupied when quantity is above 0, Empty when no active record is left),
5. commits. If anything fails, all steps are rolled back.

A database check rule also blocks a negative quantity. **Reservations:** picking (Feature 15) only reserves stock. Shipping (Feature 17) reduces quantity on hand.

## Alternatives considered

- **Each feature updates inventory itself:** duplicated rules, and slot status could be wrong.
- **Event-based, eventually consistent updates:** more complex, and a worker could see wrong stock for a short time.

## Consequences

- Quantity never goes negative and the slot status is always correct.
- Every change is traceable.
- Feature 9 becomes a critical module and needs the most tests (concurrency and failure tests).

---

[Back to the ADR index](README.md)
