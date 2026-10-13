# Feature: Receive Store Orders

**Feature ID:** 18
**Branch pattern:** `feature/18-receive-store-orders`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Deliver shipped orders to stores and confirm that the store received them.
**Depends on:** [Feature 17 — Ship Store Orders](feature-17-ship-store-orders.md)

---

## User Stories

### US-15.1: Receive store order at the store

**As a** driver
**I want to** see and deliver the orders on my route
**So that** stores receive their products.
**Priority:** P1
**Independent test:** Deliver an order and verify its status changes to Delivered. 
**Acceptance Scenarios:** see ### US-15.1 under Gherkin AC

### US-15.2: Confirm receipt by store

**As a** driver
**I want to** record who received the order at the store
**So that** there is proof of delivery.
**Priority:** P1
**Independent test:** Deliver an order and verify the receiver name and time are saved. 
**Acceptance Scenarios:** see ### US-15.2 under Gherkin AC

### US-15.3: Record delivery exception

**As a** driver
**I want to** record damaged or missing items
**So that** the difference is known.
**Priority:** P1
**Independent test:** Deliver fewer items than ordered and verify an exception is saved. 
**Acceptance Scenarios:** see ### US-15.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST list shipped orders for the driver's route.
- **FR-002:** The system MUST record quantity delivered for each item.
- **FR-003:** The system MUST record the name of the person who received the order.
- **FR-004:** The system MUST record the delivery date and time.
- **FR-005:** The system MUST set the order to Delivered when delivery is confirmed.
- **FR-006:** The system MUST NOT deliver an order that is not Shipped.
- **FR-007:** The system MUST record a note for damaged or missing items.
- **FR-008:** The system MUST let a driver and a manager use this feature (main user: driver), and MUST NOT let other roles change its information.
- **FR-009:** The system MUST be usable on a mobile device at the store.
- **FR-010:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Delivery** — Record of an order delivered to a store.
- **Shipment** — Shipment being delivered.
- **Store Order** — Order being delivered.

---

## Initial Data Model

### Delivery

- delivery_id: String — Required, unique
- shipment_id: Shipment reference — Required
- received_by_name: String — Required
- delivered_at: Date/Time — Required
- exception: Boolean — True when quantity delivered differs
- note: String — Optional

---

## Gherkin AC

### US-15.1 — Receive store order at the store

#### Scenario: Orders are listed by route

- **Given** shipped orders exist on a route
- **When** the driver opens the route
- **Then** the system lists the orders by stop

#### Scenario: Order is delivered

- **Given** a store order has status Shipped
- **When** the driver confirms the delivery
- **Then** the system sets the order to Delivered

#### Scenario: Wrong role cannot change information

- **Given** a shipping clerk is signed in
- **When** the shipping clerk tries to change information in this feature
- **Then** the system rejects the change

### US-15.2 — Confirm receipt by store

#### Scenario: Receipt is confirmed

- **Given** the driver is at the store
- **When** the store confirms the order and gives a name
- **Then** the system saves the receiver name, date, and time

#### Scenario: Missing receiver name

- **Given** the store confirms the order
- **When** no receiver name is entered
- **Then** the system does not complete the delivery

### US-15.3 — Record delivery exception

#### Scenario: Damaged item

- **Given** the store reports a damaged item
- **When** the driver records the damage
- **Then** the system saves the note on the delivery

#### Scenario: Quantity delivered differs

- **Given** quantity shipped is 10
- **And** quantity delivered is 8
- **When** the driver saves the delivery
- **Then** the system marks a delivery exception
