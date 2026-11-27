# Non-functional requirements (quality attributes)

Quality bars for the whole Warehouse system: usability, performance, security, data integrity, audit, reliability, and related targets.

| Artifact | Question |
|----------|----------|
| [Feature specs](../../features/) | What must the product do? |
| **This folder** (`docs/nfr/`) | What quality bars apply across the product? |
| [ADRs](../adr/README.md) | Why did we choose a particular approach? |

These files do not add product behavior by themselves. Feature specs still say what to build.

## Files

| File | Contents |
|------|----------|
| [quality-attributes.md](./quality-attributes.md) | One row per quality, with status and links |
| [nfr.md](./nfr.md) | Numbered NFR codes (NFR-U1, NFR-P1, …) and which feature must pass each code |

## When to update

| When | Action |
|------|--------|
| A new app-wide quality bar | Update [quality-attributes.md](./quality-attributes.md) and [nfr.md](./nfr.md) together |
| A choice about how to meet a bar | Write or update an [ADR](../adr/README.md) and link it from the row |
| A quality that belongs to one feature only | Put it in that feature's requirements and success criteria |

## Related

- [ADR index](../adr/README.md)
- [Feature catalog](../../features/README.md)
