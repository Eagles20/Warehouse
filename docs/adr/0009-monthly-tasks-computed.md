# ADR-009: Compute monthly tasks when they are read

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** Features 19, 20, 21, 22

## Context

Features 19–22 are monthly tasks. The requirements say the system must show each one as a task to do when it is due or open. A scheduler would need extra tools and could fail silently.

## Decision

No scheduler is used. When a user opens the home screen, the system **computes** the tasks for this user:
- **Inventory count (19):** due on the 1st of the month if the warehouse has no count that started this month. Open while a count is Open or Counted.
- **Expiration check (21):** due on the 1st of the month if no removal list was made this month.
- **Count (20) and Remove expired (22):** open while a count or a removal list is open.

The tasks are shown to the roles that may use the feature. Nothing is stored for the tasks.

## Alternatives considered

- **Scheduled job that creates tasks:** more moving parts, and missed runs when the server is off.
- **E-mail reminders:** need an e-mail service (out of scope).

## Consequences

- Always correct, even after downtime.
- Simple to test with a fixed date.
- The task is only seen when the user opens the system.

---

[Back to the ADR index](README.md)
