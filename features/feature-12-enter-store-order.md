# Feature: Enter Store Order

**Feature ID:** 12
**Branch pattern:** `feature/12-enter-store-order`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Enter store orders from the store order sheet and check item availability.
**Depends on:** [Feature 3 — Maintain Items](feature-3-maintain-items.md); [Feature 5 — Maintain Stores](feature-5-maintain-stores.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-9.1: Enter store order

**As an** inventory clerk
**I want to** enter a store order
**So that** the warehouse knows what the store requested.
**Priority:** P1
**Independent test:** Enter a store order and verify it is saved. 
**Acceptance Scenarios:** see ### US-9.1 under Gherkin AC

### US-9.2: Record ordered items

**As an** inventory clerk
**I want to** record the items and quantities ordered by UPC or description
**So that** the order can be picked correctly.
**Priority:** P1
**Independent test:** Add ordered items with quantities and verify they are saved. 
**Acceptance Scenarios:** see ### US-9.2 under Gherkin AC

### US-9.3: Check item availability

**As an** inventory clerk
**I want to** see if each item is in stock when I enter the order
**So that** I can warn the store about out-of-stock items.
**Priority:** P1
**Independent test:** Enter an order for more than the available quantity and verify a warning is shown. 
**Acceptance Scenarios:** see ### US-9.3 under Gherkin AC

### US-9.4: Confirm store order

**As an** inventory clerk
**I want to** confirm the store order
**So that** it can move to picking.
**Priority:** P1
**Independent test:** Confirm a complete store order and verify its status moves to Picking. 
**Acceptance Scenarios:** see ### US-9.4 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST identify the store.
- **FR-002:** The system MUST identify each ordered item.
- **FR-003:** The system MUST maintain quantity ordered separately from quantity picked and quantity delivered.
- **FR-004:** Quantity ordered MUST be greater than zero.
- **FR-005:** The system MUST show available quantity for each ordered item.
- **FR-006:** The system MUST warn the user when quantity ordered is more than quantity available.
- **FR-007:** The system MUST allow a received store order to move to Picking after required information is complete.
- **FR-008:** The system MUST reject an order without a store.
- **FR-009:** The system MUST reject an order without ordered items.
- **FR-010:** The system MUST let an inventory clerk and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-011:** The system MUST work on an office computer.


---

## Key Entities

- **Store Order** — Order received from a store for products.
- **Store** — Retail store placing the order.
- **Item** — Product requested on the order.
- **Inventory** — Stock used to check availability.

---

## Initial Data Model

### Store Order

- store_order_id: String — Required, unique
- store_id: Store reference — Required
- order_date: Date — Required
- status: String/Enum — Received, Picking, Picked, Shipped, Delivered

### Store Order Item

- store_order_id: Store Order reference — Required
- item_id: Item reference — Required
- quantity_ordered: Integer — Required, greater than zero
- quantity_picked: Integer — Zero or greater
- quantity_delivered: Integer — Zero or greater
- unit_price: Decimal — Required, zero or greater

---

## Gherkin AC

### US-9.1 — Enter store order

#### Scenario: Enter order successfully

- **Given** a store and at least one item are identified
- **When** the clerk enters the store order
- **Then** the system saves the store order with status Received

#### Scenario: Missing store

- **Given** no store is selected
- **When** the clerk tries to enter the store order
- **Then** the system rejects the order

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-9.2 — Record ordered items

#### Scenario: Missing item

- **Given** no item is added to the order
- **When** the clerk tries to enter the store order
- **Then** the system rejects the order

#### Scenario: Invalid quantity

- **Given** the clerk enters a quantity ordered of zero or less
- **When** the clerk tries to save the store order
- **Then** the system rejects the invalid quantity

### US-9.3 — Check item availability

#### Scenario: Item is in stock

- **Given** item A has 20 on hand
- **When** the clerk orders 10 of item A
- **Then** the system shows the item as available

#### Scenario: Not enough stock

- **Given** item A has 3 on hand
- **When** the clerk orders 10 of item A
- **Then** the system warns that only 3 are available

### US-9.4 — Confirm store order

#### Scenario: Confirm complete order

- **Given** a store order has a store and valid ordered items
- **When** the clerk confirms the store order
- **Then** the system moves the store order to Picking

#### Scenario: Incomplete order cannot move to picking

- **Given** a store order is missing required information
- **When** the clerk tries to confirm the store order
- **Then** the system does not move the store order to Picking
