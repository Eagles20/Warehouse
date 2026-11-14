# Quality attributes

App-wide quality targets for the **Warehouse** system.

**Source:** Warehouse requirements and the detailed requirement codes in [NFR.md](NFR.md).

**Policy:** Feature specs say what to build. This table says how good the system should be. Only **Accepted** rows (and feature **Functional Requirements** and **Success Criteria**) limit the implementation. **Deferred** is the quality backlog. **Out of scope** is what we do not build.

**Draft values:** The numbers are based on a small warehouse. Please check them with the instructor.

**Assumed size:** 1 to 5 companies, 1 to 3 warehouses, 10,000 items, 20,000 inventory records, 50 users at the same time, 500 store orders per month.

## Column meanings

| Column | Meaning |
|--------|---------|
| **Target** | The measure or bar. We use a **number** when we can. |
| **Approach** | How we reach the target |
| **How we verify** | How we prove it works |
| **Status** | **Accepted** / **Deferred** / **Out of scope** (see below) |
| **Links** | NFR codes, ADR (why), or the features it applies to |

## Status values

| Status | Meaning |
|--------|---------|
| **Accepted** | In force. Do not make it worse. |
| **Deferred** | Written down, but not required yet |
| **Out of scope** | Do not build it |

---

## Attribute table

| Attribute | Target | Approach | How we verify | Status | Links |
|-----------|--------|----------|---------------|--------|-------|
| **Usability** | Office screens work at **1280 px** or wider; floor screens work on a handheld (**390 px**) and a tablet (**820 px**) with tap targets **≥ 44 px**; daily main action in **≤ 5 taps**; errors in plain words next to the field | Responsive web layout; large buttons; search box focused on open; confirm before saving rare changes; show last change date and person | Manual checks at each screen width; tap count walkthrough; Gherkin scenarios | Accepted | NFR-U1 to U9; all features |
| **Performance** | Normal screens **< 1 s** and search **< 1 s** for 95% of requests; reports **< 3 s** for 95% of requests (at the assumed size) | Database indexes; paging; query tuning | Load test with 50 users at the assumed size | Accepted | NFR-P1 to P3; Features 13, 14, 23 |
| **Security** | **100%** of users sign in; HTTPS only; session ends after **8 h** idle; role checked on **every** endpoint (wrong role gets 403); no passwords or personal data in logs; all input validated on the server | Login with salted password hashes; role check on the server; safe database queries; output escaping | Role tests for each endpoint; tests with bad input; log review | Accepted | NFR-S1 to S4; all features; [ADR-006](ADRs/README.md) |
| **Data integrity** | Each change saved in **one transaction** with its audit entry; multi-record changes **all succeed or all fail**; no lost updates (stale version gets **409**); delete sets the record **inactive** | Database transactions; version number on each record; soft delete | Tests with forced failures; two-user update test | Accepted | NFR-D1 to D4; [ADR-003, 005, 007, 008](ADRs/README.md) |
| **Auditability** | **100%** of create, edit, delete, status change, and denied attempts write an audit entry; **100%** of inventory quantity changes write a change entry with reason and source feature | Audit log table; inventory change log | Check the logs after each action | Accepted | NFR-A1, A2; [ADR-005, 008](ADRs/README.md) |
| **Reliability / Availability** | Available **≥ 99%** each month during warehouse hours (**06:00–20:00**, every day); **daily** backup; restore tested **every quarter**; lose **≤ 1 day** of data | Simple deployment; monitoring; scheduled database backup | Monthly uptime report; restore test | Accepted | NFR-R1, R2; all features |
| **Maintainability** | **100%** of Gherkin scenarios automated; build **fails** when one fails; architecture test **fails** on direct table access across modules | Spec-driven development; automated tests in the build; module boundaries | Build result | Accepted | NFR-M1, M2; [ADR-001, 004, 014](ADRs/README.md) |
| **Compatibility** | Works on the **current and previous** version of Chrome, Edge, and Safari; handheld and tablet screens work **with gloves** (large rows, large buttons, confirm before every delete) | Standard web features only; large touch design | Manual check in each browser and on devices | Accepted | NFR-C1, C2; Features 7–8, 10, 13–18, 20, 22 |
| **Task visibility** | Due or open monthly tasks show on the **home screen**; reports keep the last filters | Home screen task list; saved filters for each user | Gherkin scenarios | Accepted | NFR-U7, U8; Features 19–23; [ADR-009](ADRs/README.md) |
| **Internationalization (i18n)** | **1** language | Fixed screen text | — | Out of scope | — |

---

## NFR code map

| Attribute row above | NFR codes |
|---------------------|-----------|
| **Usability** | U1, U2, U3, U4, U5, U6, U9 |
| **Performance** | P1, P2, P3 |
| **Security** | S1, S2, S3, S4 |
| **Data integrity** | D1, D2, D3, D4 |
| **Auditability** | A1, A2 |
| **Reliability / Availability** | R1, R2 |
| **Maintainability** | M1, M2 |
| **Compatibility** | C1, C2 |
| **Task visibility** | U7, U8 |

The full requirements and the list of NFR codes for each feature are in [NFR.md](NFR.md).

---

## Feature-local requirements

If only one feature needs a bar:

1. Put it in that feature's **Functional Requirements** or **Success Criteria**.
2. Add Gherkin when a test must prove it.
3. Optionally link it from a row here.

---

## Changing a bar

1. Edit **Target**, **Approach**, **Status**, or **Links**. Prefer a number in **Target**.
2. If the **Approach** changes, write or update an [ADR](ADRs/README.md) and add it to **Links**.
3. Keep [NFR.md](NFR.md) in the same change, so both files say the same thing.
4. Change **Deferred** to **Accepted** only when a test or check can prove it.
