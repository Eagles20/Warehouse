# ADR-013: Build and run with Docker and a CI pipeline

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** All features (Definition of Done)

## Context

The team works on different computers, and the project must build the same way for everyone and for the instructor.

## Decision

The back end, the front end, and the database run in Docker containers started with Docker Compose. A CI pipeline (GitHub Actions) runs on every pull request: build, unit tests, Gherkin tests, API contract check, and a lint check. A pull request can be merged into `main` only when the pipeline passes.

## Alternatives considered

- **Install tools by hand:** different versions on each computer.
- **Cloud platform from the start:** not needed for the course.

## Consequences

- The same result on every computer.
- Docker must be installed on every team computer.

---

[Back to the ADR index](README.md)
