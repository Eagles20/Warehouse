# Feature: Move Inventory

**Feature ID:** 16
**Branch pattern:** `feature/16-move-inventory`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Move inventory from one storage slot to another.
**Depends on:** [Feature 14 — Find Empty Slot](feature-14-find-empty-slot.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-13.1: Move inventory

**As a** warehouse worker
**I want to** move items from one slot to another
**So that** stock is placed where it is needed.
**Priority:** P1
**Independent test:** Move items and verify both slots are updated. 
**Acceptance Scenarios:** see ### US-13.1 under Gherkin AC

### US-13.2: Record the move

**As a** manager
**I want to** see a record of every move
**So that** inventory changes can be traced.
**Priority:** P1
**Independent test:** Move items and verify a move record is saved. 
**Acceptance Scenarios:** see ### US-13.2 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST allow the user to choose a source slot and a destination slot.
- **FR-002:** The system MUST NOT move more than the quantity in the source slot.
- **FR-003:** The destination slot MUST be Empty or hold the same item.
- **FR-004:** The system MUST update the quantity and status of both slots.
- **FR-005:** The system MUST record each move.
- **FR-006:** The system MUST let a warehouse worker and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-007:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-008:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Move Record** — Record of items moved between slots.
- **Inventory** — Stock being moved.
- **Slot** — Source and destination location.

---

## Initial Data Model

### Move Record

- move_id: String — Required, unique
- item_id: Item reference — Required
- from_slot_id: Slot reference — Required
- to_slot_id: Slot reference — Required, different from from_slot_id
- quantity: Integer — Required, greater than zero
- moved_by: Employee reference — Required
- move_date: Date/Time — Required

---

## Gherkin AC

### US-13.1 — Move inventory

#### Scenario: Move all items to an empty slot

- **Given** slot A holds 10 units and slot B is Empty
- **When** the worker moves 10 units to slot B
- **Then** slot B holds 10 units
- **And** slot A is Empty

#### Scenario: Move part of the items

- **Given** slot A holds 10 units and slot B is Empty
- **When** the worker moves 4 units to slot B
- **Then** slot A holds 6 units
- **And** slot B holds 4 units

#### Scenario: Move too many

- **Given** slot A holds 10 units
- **When** the worker tries to move 12 units
- **Then** the system rejects the move

#### Scenario: Destination holds another item

- **Given** the destination slot holds a different item
- **When** the worker moves an item there
- **Then** the system rejects the move

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-13.2 — Record the move

#### Scenario: Move record is saved

- **Given** a move is completed
- **When** the system saves the move
- **Then** the system stores the item, both slots, quantity, worker, and date
