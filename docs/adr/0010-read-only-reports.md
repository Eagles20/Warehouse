# ADR-010: Reports are read-only queries on live data

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** Feature 23

## Context

Feature 23 gives four management reports. The data volume is small (about 10,000 items and 20,000 inventory records).

## Decision

Reports run read-only queries on the main database. A separate database user without write rights runs them. There is no reporting database and no copy of the data. Filters are remembered in the manager's browser. Each response shows the time it was loaded. If reports become slow, a read replica can be added without changing the API.

## Alternatives considered

- **Data warehouse with a nightly load:** too heavy, and the data would be old.
- **Pre-calculated report tables:** faster, but they can become wrong.

## Consequences

- Reports are always current and cannot change any data.
- A heavy report can slow the main database. Date ranges are limited to 366 days.

---

[Back to the ADR index](README.md)
