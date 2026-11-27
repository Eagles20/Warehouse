# C4 — Deployment

Each person uses a separate device. Those devices are separate hardware, so each one is its own box. The office computer, tablet, handheld, and phone only run the web app in a browser. One server runs the site, the API, and the database.

```mermaid
C4Deployment
title Deployment Diagram — Warehouse

Deployment_Node(office, "Office computer", "Desk PC for clerks and the manager") {
  Container(officeApp, "Web app", "Browser", "Office screens.")
}

Deployment_Node(tablet, "Tablet", "Floor tablet") {
  Container(tabletApp, "Web app", "Browser", "Larger floor screens.")
}

Deployment_Node(handheld, "Handheld", "Scanner handheld") {
  Container(handApp, "Web app", "Browser", "Scan screens. The scanner types like a keyboard.")
}

Deployment_Node(phone, "Phone", "Driver phone") {
  Container(phoneApp, "Web app", "Browser", "Delivery screens.")
}

Deployment_Node(server, "Web server", "One server for this course") {
  Container(site, "Web site", "Static files", "The built React app.")
  Container(api, "API", "Java, Spring Boot", "REST API and role checks.")
  ContainerDb(db, "Database", "PostgreSQL", "Warehouse data.")
}

Rel(site, officeApp, "Serves", "HTTPS")
Rel(site, tabletApp, "Serves", "HTTPS")
Rel(site, handApp, "Serves", "HTTPS")
Rel(site, phoneApp, "Serves", "HTTPS")
Rel(officeApp, api, "JSON", "HTTPS")
Rel(tabletApp, api, "JSON", "HTTPS")
Rel(handApp, api, "JSON", "HTTPS")
Rel(phoneApp, api, "JSON", "HTTPS")
Rel(api, db, "SQL", "TCP")

UpdateLayoutConfig($c4ShapeInRow="2", $c4BoundaryInRow="1")
```

## Notes

- The four devices are not one shared computer. Each employee device is drawn separately because it is its own hardware.
- The same web app is loaded on every device. The screen layout changes with the device.
- A daily backup is kept off this server. It is an operations copy, not another application.

**Related:** [ADR-002](../adr/0002-technology-stack.md) · [ADR-012](../adr/0012-barcode-scanner-input.md) · [ADR-013](../adr/0013-docker-and-ci.md)
