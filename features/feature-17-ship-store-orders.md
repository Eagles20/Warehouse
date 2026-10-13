# Feature: Ship Store Orders

**Feature ID:** 17
**Branch pattern:** `feature/17-ship-store-orders`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Wrap, label, and ship picked store orders on the store's route.
**Depends on:** [Feature 15 — Pick Store Orders](feature-15-pick-store-orders.md); [Feature 7 — Maintain Routes](feature-7-maintain-routes.md)

---

## User Stories

### US-14.1: Ship picked order

**As a** shipping clerk
**I want to** ship a picked store order
**So that** the store receives the products.
**Priority:** P1
**Independent test:** Ship a picked order and verify its status changes to Shipped. 
**Acceptance Scenarios:** see ### US-14.1 under Gherkin AC

### US-14.2: Confirm order is wrapped

**As a** shipping clerk
**I want to** confirm that the order is wrapped and labeled
**So that** the order is not lost.
**Priority:** P1
**Independent test:** Ship an order that is not wrapped and verify it is rejected.
**Acceptance Scenarios:** see ### US-14.2 under Gherkin AC

### US-14.3: Assign order to route

**As a** shipping clerk
**I want to** put the order on the route of its store
**So that** the driver delivers it.
**Priority:** P1
**Independent test:** Ship an order and verify the route and driver are saved.
**Acceptance Scenarios:** see ### US-14.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST list store orders with status Picked.
- **FR-002:** The system MUST NOT ship an order that is not Picked.
- **FR-003:** The system MUST require confirmation that the order is wrapped and labeled.
- **FR-004:** The system MUST assign the shipment to the route and driver of the store.
- **FR-005:** The system MUST reduce inventory by the quantity shipped.
- **FR-006:** The system MUST set the order to Shipped and record the ship date.
- **FR-007:** The system MUST let a shipping clerk and a manager use this feature (main user: shipping clerk), and MUST NOT let other roles change its information.
- **FR-008:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-009:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Shipment** — Record that an order left the warehouse.
- **Store Order** — Order being shipped.
- **Route** — Route the order travels on.

---

## Initial Data Model

### Shipment

- shipment_id: String — Required, unique
- store_order_id: Store Order reference — Required
- route_id: Route reference — Required
- driver_id: Employee reference — Required, role is Driver
- wrapped: Boolean — Required, must be true to ship
- ship_date: Date — Required
- shipped_by: Employee reference — Required

---

## Gherkin AC

### US-14.1 — Ship picked order

#### Scenario: Order is shipped

- **Given** a store order has status Picked
- **When** the clerk ships the order
- **Then** the system sets the order to Shipped
- **And** the system reduces inventory by the quantity shipped

#### Scenario: Order is not picked

- **Given** a store order has status Received or Picking
- **When** the clerk tries to ship the order
- **Then** the system rejects the shipment

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-14.2 — Confirm order is wrapped

#### Scenario: Order is wrapped

- **Given** a picked order is wrapped and labeled with its order number
- **When** the clerk ships the order
- **Then** the system saves the shipment

#### Scenario: Order is not wrapped

- **Given** a picked order is not wrapped
- **When** the clerk tries to ship the order
- **Then** the system rejects the shipment

### US-14.3 — Assign order to route

#### Scenario: Route and driver are assigned

- **Given** a store belongs to a route with a driver
- **When** the clerk ships the store order
- **Then** the system saves the route and driver on the shipment

#### Scenario: Store has no route

- **Given** a store has no route
- **When** the clerk tries to ship the order
- **Then** the system rejects the shipment
