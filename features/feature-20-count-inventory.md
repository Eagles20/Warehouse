# Feature: Count Inventory

**Feature ID:** 20
**Branch pattern:** `feature/20-count-inventory`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Count inventory in each slot, find differences, and update inventory.
**Depends on:** [Feature 19 — Initiate Inventory Count](feature-19-initiate-inventory-count.md)

---

## User Stories

### US-17.1: Enter counted quantity

**As a** warehouse worker
**I want to** enter the quantity I counted in each slot
**So that** the real stock is recorded.
**Priority:** P3
**Independent test:** Enter counted quantities and verify they are saved.
**Acceptance Scenarios:** see ### US-17.1 under Gherkin AC

### US-17.2: Calculate variance

**As an** inventory clerk
**I want to** see the difference between counted and expected quantity
**So that** I can find stock problems.
**Priority:** P3
**Independent test:** Enter counts and verify variance is calculated. 
**Acceptance Scenarios:** see ### US-17.2 under Gherkin AC

### US-17.3: Close count

**As an** inventory clerk
**I want to** close the count
**So that** inventory matches what was counted.
**Priority:** P3
**Independent test:** Close a counted count and verify inventory is updated.
**Acceptance Scenarios:** see ### US-17.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST list the count lines of an open count.
- **FR-002:** The system MUST allow a counted quantity of zero or greater.
- **FR-003:** The system MUST calculate variance as counted quantity minus expected quantity.
- **FR-004:** The system MUST set the count to Counted when all lines are entered.
- **FR-005:** The system MUST update inventory to the counted quantities when the count is closed.
- **FR-006:** The system MUST NOT close a count that has uncounted lines.
- **FR-007:** The system MUST let a warehouse worker, an inventory clerk, and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-008:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-009:** This task is done every month, so the system MUST show it as a task to do when it is due or open.

---

## Key Entities

- **Inventory Count** — Count being worked.
- **Count Line** — Slot being counted.
- **Inventory** — Stock updated when the count is closed.

---

## Initial Data Model

### Inventory Count

- count_id: String — Required, unique
- warehouse_id: Warehouse reference — Required
- start_date: Date — Required
- status: String/Enum — Open, Counted, Closed

### Count Line

- count_line_id: String — Required, unique
- count_id: Inventory Count reference — Required
- slot_id: Slot reference — Required
- item_id: Item reference — Required
- expected_quantity: Integer — Required, zero or greater
- counted_quantity: Integer — Required to close, zero or greater
- variance: Integer — Counted minus expected
- counted_by: Employee reference — Required when counted

---

## Gherkin AC

### US-17.1 — Enter counted quantity

#### Scenario: Quantity is saved

- **Given** an open count has a count line
- **When** the worker enters a counted quantity
- **Then** the system saves the counted quantity

#### Scenario: Negative quantity

- **Given** the worker enters a negative counted quantity
- **When** the worker tries to save
- **Then** the system rejects the quantity

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

#### Scenario: Monthly task is shown when due

- **Given** the monthly task is due or open
- **When** the user opens the system
- **Then** the system shows the task to do

### US-17.2 — Calculate variance

#### Scenario: Count matches

- **Given** a count line expects 10 units
- **When** the worker enters 10
- **Then** the variance is 0

#### Scenario: Count is lower

- **Given** a count line expects 10 units
- **When** the worker enters 8
- **Then** the variance is -2

#### Scenario: All lines counted

- **Given** every count line has a counted quantity
- **When** the last line is saved
- **Then** the system sets the count to Counted

### US-17.3 — Close count

#### Scenario: Count is closed

- **Given** a count has status Counted
- **When** the clerk closes the count
- **Then** the system updates inventory to the counted quantities
- **And** the system sets the count to Closed

#### Scenario: Count is not finished

- **Given** a count line has no counted quantity
- **When** the clerk tries to close the count
- **Then** the system rejects the request
