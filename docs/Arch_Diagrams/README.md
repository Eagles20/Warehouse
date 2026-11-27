# Architecture diagrams (C4)

C4 views of the Warehouse system, in the same set as the POS Speckit example.

**Shape of the system:** one web application and one PostgreSQL database. Employees use it from separate devices (office computer, tablet, handheld, phone). See [ADR-001](../adr/0001-modular-monolith.md) and [ADR-002](../adr/0002-technology-stack.md).

| Level | File |
|-------|------|
| Context | [c4-context.md](./c4-context.md) |
| Container | [c4-container.md](./c4-container.md) |
| Component | [c4-component.md](./c4-component.md) — User Interface, Problem Domain, Data Management, Database |
| Deployment | [c4-deployment.md](./c4-deployment.md) |

## Preview

1. The fence must start with `C4Context`, `C4Container`, `C4Component`, or `C4Deployment`.
2. Use a Mermaid preview that understands C4. A plain Markdown preview often shows the code instead of the picture.

**Related:** [ADR index](../adr/README.md)
