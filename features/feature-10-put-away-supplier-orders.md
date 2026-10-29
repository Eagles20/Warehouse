# Feature: Put Away Supplier Orders

**Feature ID:** 10
**Branch pattern:** `feature/10-put-away-supplier-orders`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Put received products into storage slots and update inventory.
**Depends on:** [Feature 11 — Receive Supplier Orders](feature-11-receive-supplier-orders.md); [Feature 14 — Find Empty Slot](feature-14-find-empty-slot.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-8.1: Put away received items

**As a** warehouse worker
**I want to** put received items into a storage slot
**So that** items can be stored and found later.
**Priority:** P1
**Independent test:** Put away a received item and verify the put away record is saved. 
**Acceptance Scenarios:** see ### US-8.1 under Gherkin AC

### US-8.2: Record quantity put away

**As a** warehouse worker
**I want to** record how many items I put in the slot
**So that** the quantity stored is correct.
**Priority:** P1
**Independent test:** Enter a quantity on a put away record and verify it is saved. 
**Acceptance Scenarios:** see ### US-8.2 under Gherkin AC

### US-8.3: Update inventory

**As a** warehouse worker
**I want to** have inventory updated when I put items away
**So that** the system knows what is in each slot.
**Priority:** P1
**Independent test:** Put away items and verify inventory and the slot are updated. 
**Acceptance Scenarios:** see ### US-8.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST allow received items to be assigned to a storage slot.
- **FR-002:** The system MUST maintain the storage slot of received items.
- **FR-003:** The system MUST NOT allow an item to be put away without a slot.
- **FR-004:** The system MUST NOT put away more than the quantity received.
- **FR-005:** The system MUST create an inventory record with quantity, received date, and expiration date when items are put away.
- **FR-006:** The system MUST change the slot status to Occupied.
- **FR-007:** The system MUST record who put away the items and when.
- **FR-008:** The system MUST let a warehouse worker and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-009:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-010:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Put Away Record** — Record of where a received item was stored.
- **Item** — Product being put away.
- **Slot** — Location where the item is stored.
- **Inventory** — Quantity of the item stored in the slot.

---

## Initial Data Model

### Put Away Record

- put_away_id: String — Required, unique
- receipt_id: Supplier Order Receipt reference — Required
- item_id: Item reference — Required
- slot_id: Slot reference — Required
- quantity: Integer — Required, greater than zero
- put_away_by: Employee reference — Required
- put_away_date: Date — Required

---

## Gherkin AC

### US-8.1 — Put away received items

#### Scenario: Successful put away

- **Given** received items exist for a supplier order
- **When** the worker puts the items away to an empty slot
- **Then** the system saves the put away record

#### Scenario: Missing slot

- **Given** no slot is selected
- **When** the worker tries to put the items away
- **Then** the system does not save the put away record

#### Scenario: Occupied slot is rejected

- **Given** a slot holds a different item
- **When** the worker tries to put the items away to that slot
- **Then** the system rejects the put away

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-8.2 — Record quantity put away

#### Scenario: Invalid quantity

- **Given** the worker enters a quantity of zero or less
- **When** the worker tries to save the put away record
- **Then** the system rejects the invalid quantity

#### Scenario: Quantity is more than received

- **Given** 10 units were received
- **When** the worker tries to put away 12 units
- **Then** the system rejects the put away

### US-8.3 — Update inventory

#### Scenario: Inventory is updated

- **Given** 10 units were received and put away to an empty slot
- **When** the put away is saved
- **Then** the system adds an inventory record of 10 units
- **And** the slot status is Occupied
