# ADR-014: Test with the Gherkin scenarios of each feature

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** All features

## Context

Each feature file has Gherkin acceptance scenarios, and the Definition of Done says that all scenarios pass.

## Decision

- Every Gherkin scenario becomes an automated test (Cucumber with the REST API).
- Unit tests cover rules with numbers (supplier order quantity, expiration dates, variance).
- Concurrency tests cover the shared rules: two users changing one inventory record, and two users starting one count.
- Failure tests check that a failed step rolls back the whole change (put away, move, ship, count close).
- Accessibility and device checks are done by hand on the screen sizes in the NFR file.

## Alternatives considered

- **Manual testing only:** does not protect against later changes.

## Consequences

- Requirements and tests stay linked, because the scenario text is the test text.
- Writing scenarios as code needs time in every feature.

---

[Back to the ADR index](README.md)
