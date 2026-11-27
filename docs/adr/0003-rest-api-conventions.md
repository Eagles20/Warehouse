# ADR-003: Use REST JSON API rules for every feature

**Status:** Proposed
**Date:** 2026-11-06
**Related features:** All features 

## Context

Every feature has an API (see the API Requirements section of each feature). Without common rules, each feature would use different paths, error formats, and ways to handle two people changing the same record.

## Decision

- Base path `/api/v1`. Resource names are plural and in kebab-case (`/store-orders`, `/inventory-records`).
- Standard calls: `GET` list and one, `POST` create (201 + `Location`), `PUT` change, `DELETE` = set inactive (204, see ADR-007). Actions that are not plain CRUD use `POST` on an action path, for example `POST /inventory-counts/{countId}/close`.
- Data is JSON. Dates are ISO 8601. Timestamps are UTC.
- Errors use `application/problem+json` with a `code` such as `VALIDATION_FAILED`, `NOT_FOUND`, `DUPLICATE`, `IN_USE`, `WRONG_STATUS`, `VERSION_CONFLICT`, `FORBIDDEN_ROLE`, and `NOT_AUTHENTICATED`.
- **Optimistic locking:** every record that can be edited has an integer `version`. A `PUT` must send the version it read. If the version is old, the API answers `409 VERSION_CONFLICT` and changes nothing.
- Lists use `page` and `size`, and filters as query parameters.
- The API is described in an OpenAPI file that is checked in the build.

## Alternatives considered

- **GraphQL:** flexible, but our screens are simple and fixed.
- **Last write wins (no version):** simple, but one worker could silently overwrite another worker's inventory change.

## Consequences

- All 23 features look and behave the same for the front end.
- Two users who change the same record get a clear message instead of lost data.
- Each screen must handle the `VERSION_CONFLICT` message (reload and try again).

---

[Back to the ADR index](README.md)
