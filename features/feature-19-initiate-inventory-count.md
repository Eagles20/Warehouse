# Feature: Initiate Inventory Count

**Feature ID:** 19
**Branch pattern:** `feature/19-initiate-inventory-count`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Start a monthly inventory count for a warehouse.
**Depends on:** [Feature 2 — Maintain Warehouse](feature-2-maintain-warehouse.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-16.1: Start inventory count

**As an** inventory clerk
**I want to** start an inventory count for a warehouse
**So that** workers know which slots to count.
**Priority:** P3
**Independent test:** Start a count and verify count lines are created. 
**Acceptance Scenarios:** see ### US-16.1 under Gherkin AC

### US-16.2: Prevent duplicate count

**As an** inventory clerk
**I want to** be stopped from starting a second count
**So that** counts do not overlap.
**Priority:** P3
**Independent test:** Start a second count and verify it is rejected. 
**Acceptance Scenarios:** see ### US-16.2 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST create an inventory count for one warehouse.
- **FR-002:** The system MUST create a count line for each slot that has inventory.
- **FR-003:** The system MUST store the expected quantity on each count line.
- **FR-004:** A new count MUST have status Open.
- **FR-005:** The system MUST NOT allow two open counts for the same warehouse.
- **FR-006:** The system MUST let an inventory clerk and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-007:** The system MUST work on an office computer.
- **FR-008:** This task is done every month, so the system MUST show it as a task to do when it is due or open.

---

## Key Entities

- **Inventory Count** — A count of all inventory in one warehouse.
- **Count Line** — Expected and counted quantity for one slot.

---

## Initial Data Model

### Inventory Count

- count_id: String — Required, unique
- warehouse_id: Warehouse reference — Required
- start_date: Date — Required
- started_by: Employee reference — Required
- status: String/Enum — Open, Counted, Closed

### Count Line

- count_line_id: String — Required, unique
- count_id: Inventory Count reference — Required
- slot_id: Slot reference — Required
- item_id: Item reference — Required
- expected_quantity: Integer — Required, zero or greater
- counted_quantity: Integer — Empty until counted, zero or greater
- variance: Integer — Counted minus expected

---

## Gherkin AC

### US-16.1 — Start inventory count

#### Scenario: Count is started

- **Given** a warehouse has slots with inventory
- **When** the clerk initiates a count
- **Then** the system creates a count with status Open
- **And** the system creates a count line for each slot with inventory

#### Scenario: Expected quantity is saved

- **Given** a slot holds 10 units
- **When** the clerk initiates a count
- **Then** the count line shows an expected quantity of 10

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

#### Scenario: Monthly task is shown when due

- **Given** the monthly task is due or open
- **When** the user opens the system
- **Then** the system shows the task to do

### US-16.2 — Prevent duplicate count

#### Scenario: Count already open

- **Given** an open count exists for the warehouse
- **When** the clerk tries to initiate another count
- **Then** the system rejects the request
