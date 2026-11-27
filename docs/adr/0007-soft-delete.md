# ADR-007: "Delete" means set inactive (soft delete)

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** Features 1–7, 9, 11, 12

## Context

Past records (receipts, put aways, shipments, reports) must keep the original names and addresses of items, stores, suppliers, and employees. The requirements say that deleting must only remove a record from the active lists.

## Decision

Master data and other records are never removed from the database. "Delete" sets `is_active = false`. Inactive records are hidden from normal lists and from drop-down lists, but are still used to show history. Delete is blocked when the record is still used (for example, an item on an open order). Restore is not offered in this version.

Unique rules (such as SKU and UPC) still include inactive records.

## Alternatives considered

- **Hard delete:** history would break or need many copies of data.
- **Archive tables:** more code, and the same result.

## Consequences

- Reports and history stay correct.
- The database grows slowly. This is not a problem at this size.
- Every list query must filter `is_active` (a shared helper does this).

---

[Back to the ADR index](README.md)
