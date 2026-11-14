# ADR-012: Use keyboard-style barcode scanners

**Status:** Proposed
**Date:** 2026-11-04
**Related features:** Features 10, 13, 15, 16, 20

## Context

Workers scan UPC codes and slot codes (Features 10, 13, 15, 16, 20). The screens run in a browser on handheld devices and tablets.

## Decision

We use scanners that work like a keyboard: they type the code and press Enter. Scan fields are normal text fields that are focused when the screen opens. The system runs the action on Enter. No scanner software or special driver is needed. The slot code on the shelf label is `<warehouse>-<aisle><shelf>-<bin>`, for example `W-01 A1-02`.

## Alternatives considered

- **Camera scanning in the browser:** works on phones, but slower and less reliable in a warehouse.
- **Native scanner SDK:** needs a native app (see ADR-002).

## Consequences

- Works on any device with a scanner and a browser.
- Labels must be printed with the right code format (printing is out of scope).

---

[Back to the ADR index](README.md)
