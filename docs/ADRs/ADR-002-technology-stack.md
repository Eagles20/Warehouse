# ADR-002: Use Java 21, Spring Boot, PostgreSQL, and a React web app

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** All features

## Context

The system needs a web application for an office computer, a tablet, a handheld device, and a phone. The team needs tools that are common in courses and have many examples. The data is relational (items, slots, orders) and needs strong consistency.

## Decision

- **Back end:** Java 21 with Spring Boot (REST API, validation, security).
- **Database:** PostgreSQL (see ADR-005 and ADR-008 for how it is used).
- **Front end:** one React and TypeScript single-page app that adapts to the screen size (office, tablet, handheld, phone).
- **Build and run:** Maven, Docker, and a CI pipeline (ADR-013).

This is a **proposal**. The team can change it before coding starts, and then this ADR must be updated.

## Alternatives considered

- **Native mobile apps** for handheld and phone: better scanner access, but two more code bases to build and test.
- **Python/Django or Node.js back end:** also good. Java was chosen because of the team's course work and the strong typing of money and quantity values.
- **MySQL or a document database:** MySQL would also work. PostgreSQL has better constraints (partial unique indexes, check rules) that we use for rules like "one open count per warehouse".

## Consequences

- One language per side (Java, TypeScript), many learning resources.
- The handheld and phone use the browser. A barcode scanner works as a keyboard (ADR-012).
- The team must learn the Spring Boot and React tools if new to them.

---

[Back to the ADR index](README.md)
