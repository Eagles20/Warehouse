# C4 Level 2 — Containers

One web app in the browser calls one API. The API is the only program that reads and writes the database.

```mermaid
C4Container
title Container Diagram — Warehouse

Person(employee, "Employee", "Uses Warehouse on an office computer, tablet, handheld, or phone.")

System_Boundary(warehouse, "Warehouse") {
  Container(spa, "Web app", "React, TypeScript", "One app. The layout changes with the screen size.")
  Container(api, "API", "Java, Spring Boot", "REST API. Checks the employee role on every request.")
  ContainerDb(db, "Database", "PostgreSQL", "Companies, items, slots, inventory, and orders.")
}

Rel(employee, spa, "Uses", "HTTPS")
Rel(spa, api, "JSON", "HTTPS")
Rel(api, db, "SQL", "JDBC")

UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
UpdateRelStyle(employee, spa, $offsetY="-20")
UpdateRelStyle(spa, api, $offsetY="-15")
UpdateRelStyle(api, db, $offsetX="15")
```

## Notes

- There is one application and one database (a modular monolith).
- The browser never decides the role. The API reads the employee record.

**Related:** [ADR-001](../adr/0001-modular-monolith.md) · [ADR-002](../adr/0002-technology-stack.md) · [ADR-003](../adr/0003-rest-api-conventions.md)
