# ADR-011: Supplier orders are printed or faxed from a PDF

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** Feature 11, Feature 4

## Context

Suppliers receive orders by fax or phone (see the supplier contact fields in Feature 4). The system does not need to talk to the supplier's computer.

## Decision

When the clerk sends an order (Feature 11), the system sets the status to Sent and creates a **PDF** of the order. The clerk prints it, faxes it, or e-mails it with his or her own tools. The system does not send the fax or e-mail. The PDF can be downloaded again later.

## Alternatives considered

- **Built-in e-mail or fax service:** costs money and needs a provider contract.
- **EDI with suppliers:** too large for this project.

## Consequences

- No outside service is needed.
- The system cannot know if the supplier got the order. The clerk confirms by phone when needed.

---

[Back to the ADR index](README.md)
