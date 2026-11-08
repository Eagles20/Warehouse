# ADR-008: Write an audit log entry for every change

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** All features that change data

## Context

The warehouse needs to answer "who changed this, and when", for example for a stock difference. Several features show the last changed date (Features 4, 5, 7).

## Decision

Every create, edit, delete, status change, inventory change, and denied attempt writes an **audit log entry** in the same transaction as the change: who (employee), when (UTC), what (entity and ID), old and new values, and the reason when one is required. Audit entries are append-only: no screen or API can change or delete them. The "last changed" values on the screens come from the record itself (`updated_at`, `updated_by`).

## Alternatives considered

- **Log files only:** hard to search and not part of the transaction.
- **Database triggers:** work for all changes, but do not know the user or the reason.

## Consequences

- Complete history for the manager and for tests.
- Each write feature has a small extra cost per change.
- A way to view the audit log is not one of the 23 features (database query for now).

---

[Back to the ADR index](README.md)
