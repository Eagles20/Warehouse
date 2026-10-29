# Feature: Create Supplier Orders

**Feature ID:** 11
**Branch pattern:** `feature/11-create-supplier-orders`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Create supplier orders using the minimum and maximum quantity of each item.
**Depends on:** [Feature 3 — Maintain Items](feature-3-maintain-items.md); [Feature 4 — Maintain Suppliers](feature-4-maintain-suppliers.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-6.1: Create supplier order

**As an** inventory clerk
**I want to** create an order for a supplier
**So that** products can be reordered before inventory runs out.
**Priority:** P1
**Independent test:** Create a supplier order for a low-inventory item and verify that it is saved. 
**Acceptance Scenarios:** see ### US-6.1 under Gherkin AC

### US-6.2: Calculate supplier order quantity

**As an** inventory clerk
**I want to** have the system calculate the amount to order
**So that** inventory can be brought back to the maximum level.
**Priority:** P1
**Independent test:** Trigger the calculation for a low-inventory item and verify the calculated quantity reaches the maximum. 
**Acceptance Scenarios:** see ### US-6.2 under Gherkin AC

### US-6.3: Send supplier order

**As an** inventory clerk
**I want to** send the supplier order by fax or print
**So that** the supplier receives the order.
**Priority:** P1
**Independent test:** Send a saved supplier order and verify its status changes to Sent. 
**Acceptance Scenarios:** see ### US-6.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST identify inventory below the required minimum.
- **FR-002:** The system MUST consider quantity on hand and quantity on order.
- **FR-003:** If quantity on hand plus quantity on order is below minimum, the system MUST calculate the amount needed to reach maximum (maximum minus on hand minus on order).
- **FR-004:** The calculated order quantity MUST be rounded to the nearest case quantity when case quantity is used.
- **FR-005:** A supplier order MUST identify the supplier.
- **FR-006:** A supplier order MUST identify each item and quantity ordered.
- **FR-007:** The system MUST allow an authorized employee to send the supplier order.
- **FR-008:** The system MUST keep quantity ordered separate from quantity received.
- **FR-009:** The system MUST NOT save a supplier order without items.
- **FR-010:** The system MUST let an inventory clerk and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-011:** The system MUST work on an office computer.

---

## Key Entities

- **Supplier Order** — Request to reorder products from a supplier.
- **Supplier** — Vendor receiving the order.
- **Item** — Product being ordered.
- **Inventory** — Current stock levels driving the reorder decision.

---

## Initial Data Model

### Supplier Order

- supplier_order_id: String — Required, unique
- supplier_id: Supplier reference — Required
- order_date: Date — Required
- created_by: Employee reference — Required
- status: String/Enum — Draft, Sent, Received

### Supplier Order Item

- supplier_order_id: Supplier Order reference — Required
- item_id: Item reference — Required
- quantity_ordered: Integer — Required, greater than zero
- unit_cost: Decimal — Required, zero or greater

### Reorder Values (calculated from Item and Inventory)

- quantity_on_hand: Integer — Zero or greater
- quantity_on_order: Integer — Zero or greater
- minimum_quantity: Integer — Zero or greater
- maximum_quantity: Integer — Greater than or equal to minimum

---

## Gherkin AC

### US-6.1 — Create supplier order

#### Scenario: Low inventory creates an order quantity

- **Given** quantity on hand plus quantity on order is below the minimum quantity
- **When** the clerk creates a supplier order for the item
- **Then** the system includes the item on the supplier order

#### Scenario: Inventory does not need an order

- **Given** quantity on hand plus quantity on order is at or above the minimum quantity
- **When** the clerk reviews inventory for ordering
- **Then** the system does not include the item on a supplier order

#### Scenario: Supplier order saves successfully

- **Given** a supplier and at least one item needing reorder are identified
- **When** the clerk saves the supplier order
- **Then** the system saves the supplier order with status Draft

#### Scenario: Supplier order does not save when supplier is missing

- **Given** no supplier is selected
- **When** the clerk tries to save the supplier order
- **Then** the system does not save the supplier order

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-6.2 — Calculate supplier order quantity

#### Scenario: Quantity is below minimum

- **Given** quantity on hand plus quantity on order is below the minimum quantity
- **When** the system calculates the order quantity
- **Then** the system calculates the amount needed to reach the maximum quantity

#### Scenario: Example calculation

- **Given** an item has 10 on hand, 0 on order, a minimum of 15, and a maximum of 50
- **When** the system calculates the order quantity
- **Then** the order quantity is 40

#### Scenario: Quantity is above minimum

- **Given** an item has 30 on hand, a minimum of 15, and a maximum of 50
- **When** the system calculates the order quantity
- **Then** the system does not order the item

#### Scenario: Case quantity rounding

- **Given** the calculated order quantity does not match a whole case quantity
- **When** the system finalizes the order quantity
- **Then** the system rounds the order quantity to the nearest case quantity

### US-6.3 — Send supplier order

#### Scenario: Supplier order is sent successfully

- **Given** a supplier order has been saved with a supplier and items
- **When** the clerk sends the supplier order
- **Then** the system marks the supplier order as Sent

#### Scenario: Empty order cannot be sent

- **Given** a supplier order has no items
- **When** the clerk tries to send the supplier order
- **Then** the system does not send the supplier order
