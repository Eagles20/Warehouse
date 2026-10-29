# Feature: Receive Supplier Orders

**Feature ID:** 8
**Branch pattern:** `feature/8-receive-supplier-orders`
**Status:** Draft
**Created:** 2026-09-27
**Input:** Receive products ordered from suppliers, check them in, and compare ordered and received quantities.
**Depends on:** [Feature 11 — Create Supplier Orders](feature-11-create-supplier-orders.md)

---

## User Stories

### US-7.1: Receive supplier order

**As a** warehouse worker
**I want to** check in a supplier order
**So that** received products can be recorded.
**Priority:** P1
**Independent test:** Receive a supplier order and verify a receipt record is created. 
**Acceptance Scenarios:** see ### US-7.1 under Gherkin AC

### US-7.2: Record quantity received

**As a** warehouse worker
**I want to** record quantity received and expiration date
**So that** actual received inventory is known.
**Priority:** P1
**Independent test:** Enter a quantity received on a receipt and verify it is saved. 
**Acceptance Scenarios:** see ### US-7.2 under Gherkin AC

### US-7.3: Identify receiving exception

**As a** manager
**I want to** compare quantity ordered with quantity received
**So that** I can see supplier receiving differences.
**Priority:** P1
**Independent test:** Create a receipt with a quantity mismatch and verify the system flags an exception. 
**Acceptance Scenarios:** see ### US-7.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST identify the supplier order being received.
- **FR-002:** The system MUST maintain quantity ordered separately from quantity received.
- **FR-003:** The system MUST record quantity received.
- **FR-004:** Quantity received MUST NOT be negative.
- **FR-005:** The system MUST record the expiration date for items that have a shelf life.
- **FR-006:** The system MUST compare quantity ordered and quantity received.
- **FR-007:** The system MUST identify when quantity received differs from quantity ordered.
- **FR-008:** The system MUST set the supplier order to Received when all items are received.
- **FR-009:** Receiving exceptions MUST be available to management.
- **FR-010:** The system MUST let a warehouse worker and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-011:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.

---

## Key Entities

- **Supplier Order** — The original order being received against.
- **Supplier Order Receipt** — Record of what was actually received for an order.
- **Item** — Product being received.
- **Supplier** — Vendor that shipped the order.

---

## Initial Data Model

### Supplier Order Receipt

- receipt_id: String — Required, unique
- supplier_order_id: Supplier Order reference — Required
- item_id: Item reference — Required
- quantity_ordered: Integer — From order
- quantity_received: Integer — Required, zero or greater
- expiration_date: Date — Optional, required when the item has a shelf life
- received_date: Date — Required
- received_by: Employee reference — Required
- exception: Boolean — True when quantities differ
- note: String — Optional, shortage or damage

---

## Gherkin AC

### US-7.1 — Receive supplier order

#### Scenario: Receive supplier order

- **Given** a supplier order was sent to a supplier
- **When** the worker receives the supplier order
- **Then** the system creates a supplier order receipt for the order

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-7.2 — Record quantity received

#### Scenario: Record quantity received

- **Given** a supplier order receipt is being created
- **When** the worker enters the quantity received
- **Then** the system saves the quantity received

#### Scenario: Record expiration date

- **Given** the item has a shelf life
- **When** the worker enters the expiration date
- **Then** the system saves the expiration date

#### Scenario: Invalid negative quantity

- **Given** the worker enters a negative quantity received
- **When** the worker tries to save the receipt
- **Then** the system rejects the negative quantity

### US-7.3 — Identify receiving exception

#### Scenario: Received quantity equals ordered quantity

- **Given** quantity ordered is 100
- **And** quantity received is 100
- **When** the system compares the quantities
- **Then** the system does not mark the receipt as an exception

#### Scenario: Received quantity is less than ordered quantity

- **Given** quantity ordered is 100
- **And** quantity received is 80
- **When** the system compares the quantities
- **Then** the system marks the receipt as a receiving exception

#### Scenario: Received quantity is greater than ordered quantity

- **Given** quantity ordered is 100
- **And** quantity received is 110
- **When** the system compares the quantities
- **Then** the system marks the receipt as a receiving exception

#### Scenario: Receiving exception is displayed

- **Given** a receiving exception exists
- **When** the manager views receiving exceptions
- **Then** the system displays the supplier order, quantity ordered, and quantity received
