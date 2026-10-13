# Feature: View Reports

**Feature ID:** 23
**Branch pattern:** `feature/23-view-reports`
**Status:** Final
**Created:** 2026-09-28
**Input:** View management reports, including delivered quantity and low stock.
**Depends on:** [Feature 12 — Enter Store Order](feature-12-enter-store-order.md); [Feature 18 — Receive Store Orders](feature-18-receive-store-orders.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-20.1: View delivered quantity

**As a** manager
**I want to** see the quantity delivered by store and item
**So that** I know what we sold.
**Priority:** P1
**Independent test:** Run the delivered quantity report and verify totals are shown. 
**Acceptance Scenarios:** see ### US-20.1 under Gherkin AC

### US-20.2: View low stock

**As a** manager
**I want to** see items with on hand quantity below the minimum
**So that** I can fix stock problems before stores order.
**Priority:** P1
**Independent test:** Run the low stock report and verify low items are shown. 
**Acceptance Scenarios:** see ### US-20.2 under Gherkin AC

### US-20.3: View inventory and expiring items

**As a** manager
**I want to** see inventory on hand and expiring inventory
**So that** I know what we have.
**Priority:** P1
**Independent test:** Run the inventory reports and verify they match inventory. 
**Acceptance Scenarios:** see ### US-20.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST provide these reports: Delivered Quantity, Low Stock, Inventory On Hand, and Expiring Inventory.
- **FR-002:** The system MUST allow filtering by warehouse, store, item, and date range.
- **FR-003:** The system MUST show totals for each report.
- **FR-004:** The Low Stock report MUST list items where quantity on hand is below minimum quantity.
- **FR-005:** Reports MUST be read only.
- **FR-006:** The system MUST show No data when a report is empty.
- **FR-007:** The system MUST let a manager use this feature (main user: manager), and MUST NOT let other roles change its information.
- **FR-008:** The system MUST work on an office computer.
- **FR-009:** This feature is used every day or every week, so reports MUST open quickly and keep the last filters used.

---

## Key Entities

- **Report** — A read-only view of warehouse data.
- **Delivery** — Source of delivered quantity.
- **Inventory** — Source of on hand and expiring quantity.
- **Item** — Source of minimum quantity.

---

## Initial Data Model

### Report Request (parameters)

- report_type: String/Enum — Required, Delivered Quantity, Low Stock, Inventory On Hand, Expiring Inventory
- warehouse_id: Warehouse reference — Optional
- store_id: Store reference — Optional
- item_id: Item reference — Optional
- start_date: Date — Optional
- end_date: Date — Optional

---

## Gherkin AC

### US-20.1 — View delivered quantity

#### Scenario: Delivered quantity is shown

- **Given** orders were delivered this week
- **When** the manager runs Delivered Quantity for this week
- **Then** the system shows each item with the total quantity delivered

#### Scenario: Filter by store

- **Given** orders were delivered to several stores
- **When** the manager filters by one store
- **Then** the system shows only that store's deliveries

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-20.2 — View low stock

#### Scenario: Low stock is listed

- **Given** an item has 10 on hand and a minimum of 15
- **When** the manager runs Low Stock
- **Then** the system lists the item

#### Scenario: Item above minimum

- **Given** an item has 30 on hand and a minimum of 15
- **When** the manager runs Low Stock
- **Then** the system does not list the item

### US-20.3 — View inventory and expiring items

#### Scenario: Inventory on hand is shown

- **Given** inventory exists in two warehouses
- **When** the manager runs Inventory On Hand
- **Then** the system shows each item with its total quantity

#### Scenario: Empty report

- **Given** no data exists for the date range
- **When** the manager runs a report
- **Then** the system displays No data
