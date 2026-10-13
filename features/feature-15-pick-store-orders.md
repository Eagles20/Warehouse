# Feature: Pick Store Orders

**Feature ID:** 15
**Branch pattern:** `feature/15-pick-store-orders`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Pick the items on a store order and take them to the shipping cart.
**Depends on:** [Feature 12 — Enter Store Order](feature-12-enter-store-order.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-12.1: Create pick list

**As a** warehouse worker
**I want to** get a pick list for a store order
**So that** I know what to collect.
**Priority:** P1
**Independent test:** Confirm an order and verify a pick list is created. 
**Acceptance Scenarios:** see ### US-12.1 under Gherkin AC

### US-12.2: Record quantity picked

**As a** warehouse worker
**I want to** record the quantity I picked for each line
**So that** the order is correct.
**Priority:** P1
**Independent test:** Enter quantities picked and verify they are saved. 
**Acceptance Scenarios:** see ### US-12.2 under Gherkin AC

### US-12.3: Identify short pick

**As a** manager
**I want to** see lines that could not be picked in full
**So that** I can fix stock problems.
**Priority:** P1
**Independent test:** Pick less than ordered and verify a short pick is flagged. 
**Acceptance Scenarios:** see ### US-12.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST create a pick list for a store order with status Picking.
- **FR-002:** The system MUST sort pick lines by aisle number.
- **FR-003:** The system MUST choose the oldest stock first (FIFO).
- **FR-004:** The system MUST NOT pick expired stock.
- **FR-005:** The system MUST record quantity picked for each line.
- **FR-006:** Quantity picked MUST NOT be negative.
- **FR-007:** The system MUST flag a short pick when quantity picked is less than quantity ordered.
- **FR-008:** The system MUST set the order to Picked when all lines are picked.
- **FR-009:** The system MUST let a warehouse worker and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-010:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-011:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Store Order** — Order being picked.
- **Pick Line** — One item and slot to pick from.
- **Inventory** — Stock the items are taken from.

---

## Initial Data Model

### Pick Line

- pick_line_id: String — Required, unique
- store_order_id: Store Order reference — Required
- item_id: Item reference — Required
- inventory_id: Inventory reference — Required
- quantity_to_pick: Integer — Required, greater than zero
- quantity_picked: Integer — Zero or greater
- short_pick: Boolean — True when picked is less than ordered
- picked_by: Employee reference — Required when picked

---

## Gherkin AC

### US-12.1 — Create pick list

#### Scenario: Pick list is created

- **Given** a store order has status Picking
- **When** the worker opens the order
- **Then** the system creates a pick list

#### Scenario: Pick list is sorted by aisle

- **Given** an order has items in aisles 9, 2, and 5
- **When** the pick list is created
- **Then** the system lists the lines as aisle 2, aisle 5, then aisle 9

#### Scenario: Oldest stock is picked first

- **Given** two slots hold the item with different received dates
- **When** the pick list is created
- **Then** the system uses the slot with the older received date

#### Scenario: Expired stock is not picked

- **Given** a slot holds expired stock
- **When** the pick list is created
- **Then** the system does not use that slot

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-12.2 — Record quantity picked

#### Scenario: Full order is picked

- **Given** an order for 5 units and stock is available
- **When** the worker confirms 5 units picked
- **Then** the system moves the order to Picked

#### Scenario: Negative quantity

- **Given** the worker enters a negative quantity picked
- **When** the worker tries to save
- **Then** the system rejects the quantity

### US-12.3 — Identify short pick

#### Scenario: Short pick is flagged

- **Given** an order for 5 units but only 3 are in stock
- **When** the worker confirms 3 units picked
- **Then** the system flags the line as a short pick
