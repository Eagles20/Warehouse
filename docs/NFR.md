# Non-Functional Requirements (NFR)

App-wide quality targets for the **Warehouse** system.

**How to use this file:** Features say what to build. This table says how good the system must be. Each feature lists, in its **Definition of Done**, the NFR codes it must pass. The traceability table at the end shows which codes apply to which feature.

**Draft values:** The numbers (users, data size, times) are based on a small warehouse. Please check them with the instructor.

**Assumed size:** 1 to 5 companies, 1 to 3 warehouses, 10,000 items, 20,000 inventory records, 50 users at the same time, 500 store orders per month.

## Column meanings


| Column            | Meaning                                                               |
| ----------------- | --------------------------------------------------------------------- |
| **ID**            | Code used in feature files (for example, NFR-P1)                      |
| **Target**        | The measure or bar. We use a number when we can.                      |
| **Approach**      | How we reach the target                                               |
| **How we verify** | How we prove it works                                                 |
| **Status**        | **Accepted** = must pass. **Deferred** = good idea, not required yet. |
| **Links**         | ADR (why we chose this approach) or the features it applies to        |


---



## Attribute table


| ID         | Attribute                     | Target                                                                                                                                 | Approach                               | How we verify                                          | Status   | Links                                    |
| ---------- | ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | ------------------------------------------------------ | -------- | ---------------------------------------- |
| **NFR-U1** | Usability (office)            | Works in a browser **1280 px wide or more**                                                                                            | Normal web layout for office computers | Manual check at 1280 px                                | Accepted | Features that say "office computer"      |
| **NFR-U2** | Usability (floor)             | Works on a handheld (**390 px**) and a tablet (**820 px**); tap targets **≥ 44 px** high                                               | Responsive layout, large buttons       | Manual check at both widths                            | Accepted | Floor features                           |
| **NFR-U3** | Usability (store phone)       | Phone screen with **1 column** and large buttons                                                                                       | Mobile-first layout                    | Manual check on a phone                                | Accepted | Feature 18                               |
| **NFR-U4** | Usability (daily)             | Main action in **≤ 5 taps** after the first scan or search; search box is focused on open                                              | Few steps, auto focus                  | Count taps in a manual walkthrough                     | Accepted | Daily features                           |
| **NFR-U5** | Usability (rare)              | Ask to **confirm** before saving                                                                                                       | Confirm dialog                         | Gherkin scenario                                       | Accepted | Features 1, 2, 6                         |
| **NFR-U6** | Usability (weekly)            | Lists and details show **date and person** of the last change                                                                          | Show last-change fields                | Gherkin scenario                                       | Accepted | Features 4, 5, 7                         |
| **NFR-U7** | Usability (monthly)           | Due or open monthly tasks show on the **home screen**                                                                                  | Home screen task list                  | Gherkin scenario                                       | Accepted | Features 19–22; ADR-009                  |
| **NFR-U8** | Usability (reports)           | Reports open fast and **keep the last filters**                                                                                        | Save filters for each user             | Manual check                                           | Accepted | Feature 23                               |
| **NFR-U9** | Usability (errors)            | Error messages in **plain words**, next to the field, and the input is kept                                                            | Field-level errors                     | Manual check                                           | Accepted | All features                             |
| **NFR-P1** | Performance                   | Normal screens (list, details, save) respond in **< 1 s** for 95% of requests at the assumed size                                      | Database indexes, paging               | Load test with 50 users                                | Accepted | All features                             |
| **NFR-P2** | Performance (search)          | Search and lookup respond in **< 1 s** for 95% of requests                                                                             | Indexed search                         | Load test                                              | Accepted | Features 13, 14, 23                      |
| **NFR-P3** | Performance (reports)         | Reports open in **< 3 s** for 95% of requests at the assumed size                                                                      | Query tuning                           | Load test                                              | Accepted | Feature 23                               |
| **NFR-S1** | Security (sign-in)            | **100%** of users sign in; HTTPS only; session ends after **8 h** idle; passwords stored as **salted hashes**                          | Login, HTTPS, hashing                  | Security tests                                         | Accepted | All features; ADR-006                    |
| **NFR-S2** | Security (roles)              | The server checks the **role on every endpoint**; wrong role gets **403 FORBIDDEN_ROLE** and is logged                                 | Role check on the server               | Role tests for each endpoint                           | Accepted | All features; ADR-006                    |
| **NFR-S3** | Security (logs)               | **No** passwords or personal data (phone numbers) in logs or error messages                                                            | Log filter                             | Review of logs                                         | Accepted | All features                             |
| **NFR-S4** | Security (input)              | All input **validated on the server**; safe database queries; output escaped                                                           | Validation, prepared statements        | Tests with bad input                                   | Accepted | All features                             |
| **NFR-D1** | Data integrity                | Each create, edit, delete, and status change is saved in **one transaction** with its audit entry                                      | Database transaction                   | Test: break the audit write, the change is rolled back | Accepted | Write features; ADR-008                  |
| **NFR-D2** | Data integrity (multi-record) | Changes on several records **all succeed or all fail**                                                                                 | Database transaction                   | Test with a forced failure                             | Accepted | Features 2, 9–12, 15–18, 20, 22; ADR-005 |
| **NFR-D3** | Data integrity (same record)  | Two people editing the same record **never lose a change**; old `version` gets **409 VERSION_CONFLICT**                                | Version number on each record          | Test with two updates                                  | Accepted | Features with a PUT; ADR-003             |
| **NFR-D4** | Data integrity (delete)       | Delete sets the record **inactive**; history keeps the old data                                                                        | Soft delete                            | Gherkin scenario                                       | Accepted | Features 1–7; ADR-007                    |
| **NFR-A1** | Audit                         | **100%** of create, edit, delete, status change, and denied attempts write an audit entry (who, when in UTC, what, old and new values) | Audit log table                        | Check the audit log after each action                  | Accepted | Write features; ADR-008                  |
| **NFR-A2** | Audit (inventory)             | **100%** of inventory quantity changes write an inventory change entry with **reason** and **source feature**                          | Inventory change log                   | Check the log after each action                        | Accepted | Features 9, 10, 15–17, 20, 22; ADR-005   |
| **NFR-R1** | Reliability                   | Available **≥ 99%** each month during warehouse hours (**06:00–20:00**, every day)                                                     | Simple deployment, monitoring          | Monthly uptime report                                  | Accepted | All features                             |
| **NFR-R2** | Reliability (backup)          | **Daily** backup; restore tested **every quarter**; lose **≤ 1 day** of data                                                           | Scheduled backup                       | Restore test                                           | Accepted | All features                             |
| **NFR-M1** | Maintainability (tests)       | **100%** of Gherkin scenarios are automated; the build **fails** when one fails                                                        | Automated tests in the build           | Build result                                           | Accepted | All features; ADR-014                    |
| **NFR-M2** | Maintainability (structure)   | Code follows the module boundaries; an architecture test **fails** on direct table access across modules                               | Architecture test                      | Build result                                           | Accepted | All features; ADR-001, ADR-004           |
| **NFR-C1** | Compatibility (browsers)      | Works on the **current and previous** version of Chrome, Edge, and Safari                                                              | Standard web features only             | Manual check in each browser                           | Accepted | All features                             |
| **NFR-C2** | Compatibility (gloves)        | Handheld and tablet screens work **with gloves**: large rows, large buttons, confirm before every delete                               | Large touch design                     | Manual check                                           | Accepted | Features 7–8, 10, 13–18, 20, 22          |




---



## Traceability: NFR codes per feature


| Feature                                                                                                   | NFR codes                                          |
| --------------------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| [Feature 1 — Maintain Company](../features/feature-1-maintain-company.md)                                 | U1, U5, U9, P1, S1, S2, S4, D1, D3, D4, A1, M1     |
| [Feature 2 — Maintain Warehouse](../features/feature-2-maintain-warehouse.md)                             | U1, U5, U9, P1, S1, S2, S4, D1, D2, D3, D4, A1, M1 |
| [Feature 3 — Maintain Items](../features/feature-3-maintain-items.md)                                     | U1, U4, U9, P1, S1, S2, S4, D1, D3, D4, A1, M1     |
| [Feature 4 — Maintain Suppliers](../features/feature-4-maintain-suppliers.md)                             | U1, U6, U9, P1, S1, S2, S4, D1, D3, D4, A1, M1     |
| [Feature 5 — Maintain Stores](../features/feature-5-maintain-stores.md)                                   | U1, U6, U9, P1, S1, S2, S4, D1, D3, D4, A1, M1     |
| [Feature 6 — Maintain Employees](../features/feature-6-maintain-employess.md)                             | U1, U5, U9, P1, S1, S2, S4, D1, D3, D4, A1, M1     |
| [Feature 7 — Maintain Routes](../features/feature-7-maintain-routes.md)                                   | U2, U6, U9, P1, S1, S2, S4, D1, D3, D4, A1, M1     |
| [Feature 8 — Receive Supplier Orders](../features/feature-8-receive-supplier-orders.md)                   | U2, U9, P1, S1, S2, S4, D1, A1, M1                 |
| [Feature 9 — Maintain Inventory](../features/feature-9-maintain-inventory.md)                             | U1, U4, U9, P1, S1, S2, S4, D1, D2, D3, A1, A2, M1 |
| [Feature 10 — Put Away Supplier Orders](../features/feature-10-put-away-supplier-orders.md)               | U2, U4, U9, P1, S1, S2, S4, D1, D2, A1, A2, M1     |
| [Feature 11 — Create Supplier Orders](../features/feature-11-create-supplier-orders.md)                   | U1, U9, P1, S1, S2, S4, D1, D2, D3, A1, M1         |
| [Feature 12 — Enter Store Order](../features/feature-12-enter-store-order.md)                             | U1, U9, P1, S1, S2, S4, D1, D2, D3, A1, M1         |
| [Feature 13 — Find Item](../features/feature-13-find-item.md)                                             | U2, U4, U9, P1, P2, S1, S2, S4, M1                 |
| [Feature 14 — Find Empty Slot](../features/feature-14-find-empty-slot.md)                                 | U2, U4, U9, P1, P2, S1, S2, S4, M1                 |
| [Feature 15 — Pick Store Orders](../features/feature-15-pick-store-orders.md)                             | U2, U4, U9, P1, S1, S2, S4, D1, D2, D3, A1, A2, M1 |
| [Feature 16 — Move Inventory](../features/feature-16-move-inventory.md)                                   | U2, U4, U9, P1, S1, S2, S4, D1, D2, A1, A2, M1     |
| [Feature 17 — Ship Store Orders](../features/feature-17-ship-store-orders.md)                             | U2, U4, U9, P1, S1, S2, S4, D1, D2, A1, A2, M1     |
| [Feature 18 — Receive Store Orders](../features/feature-18-receive-store-orders.md)                       | U3, U4, U9, P1, S1, S2, S4, D1, D2, A1, M1         |
| [Feature 19 — Initiate Inventory Count](../features/feature-19-initiate-inventory-count.md)               | U1, U7, U9, P1, S1, S2, S4, D1, A1, M1             |
| [Feature 20 — Count Inventory](../features/feature-20-count-inventory.md)                                 | U2, U7, U9, P1, S1, S2, S4, D1, D2, D3, A1, A2, M1 |
| [Feature 21 — Initial Check for Exp Inventory](../features/feature-21-initial-check-for-exp-inventory.md) | U1, U7, U9, P1, S1, S2, S4, D1, A1, M1             |
| [Feature 22 — Remove Expired Inventory](../features/feature-22-remove-expired-inventory.md)               | U2, U7, U9, P1, S1, S2, S4, D1, D2, D3, A1, A2, M1 |
| [Feature 23 — View Reports](../features/feature-23-view-reports.md)                                       | U1, U8, U9, P1, P2, P3, S1, S2, S4, M1             |


**Codes for all features** (add to every Definition of Done): R1, R2, M2, C1.

---



## Changing a requirement

1. Edit the **Target**, **Approach**, **Status**, or **Links** in the table above.
2. If the **Approach** changes, write or update an [ADR](ADRs/README.md) and add it to **Links**.
3. If a code is added or removed for a feature, update the traceability table and that feature's **Definition of Done**.
4. Change **Deferred** to **Accepted** only when a test or check can prove it.

See also: [ADRs](ADRs/README.md), [Architecture diagrams](Arch_Diagrams/README.md), [UI screens](UI/README.md).