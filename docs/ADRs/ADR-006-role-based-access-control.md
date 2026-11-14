# ADR-006: Control access by employee role, checked on the server

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** All features (role rules), Feature 6

## Context

The system has five roles: Inventory Clerk, Warehouse Worker, Shipping Clerk, Driver, and Manager. Each feature says which roles may use it. Hiding a button is not enough for security.

## Decision

- Every employee has exactly one role (Feature 6). Everyone signs in before using the system.
- The role is read from the employee record on **every request**. A role change works at once, and a deleted employee is blocked at once.
- The server checks the role for every endpoint, using the "Allowed roles" column of the API Requirements table. A wrong role gets `403 FORBIDDEN_ROLE` and the attempt is written to the audit log.
- Some data is limited by owner as well: a Driver sees only the routes and shipments where he or she is the driver.
- Master data lists (Features 1–5, 7) may be read by any signed-in role when other screens need them. Employee data (Feature 6) is for Managers only.
- Sign-in uses user name and password, with passwords stored as strong hashes, and a session token that expires after 8 hours of inactivity.

The sign-in screen is a platform part. It is not one of the 23 features.

## Alternatives considered

- **Check roles only in the user interface:** can be bypassed.
- **Several roles per employee:** more flexible, but the requirements say one role.

## Consequences

- Clear and testable: every endpoint has a "wrong role" test.
- Credentials are stored in the security module, not in the employee record.

---

[Back to the ADR index](README.md)
