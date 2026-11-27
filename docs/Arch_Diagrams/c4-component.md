# C4 — Components

Major parts of the Warehouse application. The names follow the same split as the POS example: **User Interface**, **Problem Domain**, **Data Management**, and **Database**.

```mermaid
C4Component
title Component diagram — Warehouse

Container_Boundary(app, "Warehouse application") {
    Component(ui, "User Interface", "React", "Office, tablet, handheld, and phone screens. A scan box is a focused text field. The scanner types like a keyboard.")
    Component(pd, "Problem Domain", "Spring Boot services", "Six modules: Master Data, Inbound, Inventory, Outbound, Stock Control, and Reporting. The role is checked on every request. Only the inventory service changes quantity on hand.")
    Component(dm, "Data Management", "JDBC", "Saves a change and its audit entry in one transaction. A module does not write another module's tables.")
    ComponentDb(db, "Database", "PostgreSQL", "Companies, suppliers, items, slots, inventory, and orders.")
}

Rel(ui, pd, "JSON", "HTTPS")
Rel(pd, dm, "Reads and writes")
Rel(dm, db, "SQL", "JDBC")

UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

## Notes

- Master Data is Features 1–7. Inbound is 8, 10, and 11. Inventory is 9, 13, 14, and 16. Outbound is 12, 15, 17, and 18. Stock Control is 19–22. Reporting is 23 and is read-only.
- Hiding a button on the screen does not replace the role check. The API rejects a wrong role.

**Related:** [ADR-001](../adr/0001-modular-monolith.md) · [ADR-005](../adr/0005-inventory-service.md) · [ADR-006](../adr/0006-role-based-access-control.md)
