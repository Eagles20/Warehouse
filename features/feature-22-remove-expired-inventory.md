# Feature: Remove Expired Inventory

**Feature ID:** 22
**Branch pattern:** `feature/22-remove-expired-inventory`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Remove expired inventory from the warehouse and update inventory.
**Depends on:** [Feature 21 — Initial Check for Exp Inventory](feature-21-initial-check-for-exp-inventory.md)

---

## User Stories

### US-19.1: Remove expired items

**As a** warehouse worker
**I want to** remove expired items from their slots
**So that** expired products are not shipped.
**Priority:** P3
**Independent test:** Remove expired items and verify inventory is reduced. 
**Acceptance Scenarios:** see ### US-19.1 under Gherkin AC

### US-19.2: Complete removal list

**As an** inventory clerk
**I want to** see when every expired item is removed
**So that** the list can be closed.
**Priority:** P3
**Independent test:** Remove all lines and verify the list is Completed. 
**Acceptance Scenarios:** see ### US-19.2 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST list the lines of an open removal list.
- **FR-002:** The system MUST record the quantity removed for each line.
- **FR-003:** The system MUST NOT allow quantity removed to be more than the quantity on the list.
- **FR-004:** The system MUST reduce inventory by the quantity removed.
- **FR-005:** The system MUST set a slot to Empty when its quantity reaches zero.
- **FR-006:** The system MUST set the removal list to Completed when all lines are removed.
- **FR-007:** The system MUST let a warehouse worker, an inventory clerk, and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-008:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-009:** This task is done every month, so the system MUST show it as a task to do when it is due or open.

---

## Key Entities

- **Removal List** — List being worked.
- **Removal Line** — Inventory record being removed.
- **Inventory** — Stock reduced by the removal.
- **Slot** — Location that may become Empty.

---

## Initial Data Model

### Removal List

- removal_id: String — Required, unique
- created_date: Date — Required
- status: String/Enum — Open, Completed

### Removal Line

- removal_line_id: String — Required, unique
- removal_id: Removal List reference — Required
- inventory_id: Inventory reference — Required
- quantity: Integer — Required, greater than zero
- quantity_removed: Integer — Zero or greater, not more than quantity
- removed_by: Employee reference — Required when removed
- removed_date: Date — Required when removed

---

## Gherkin AC

### US-19.1 — Remove expired items

#### Scenario: Expired items are removed

- **Given** a slot holds 5 expired units on the removal list
- **When** the worker confirms 5 units removed
- **Then** the system reduces inventory to 0
- **And** the slot status is Empty

#### Scenario: Partial removal

- **Given** 5 expired units are on the list
- **When** the worker confirms 3 units removed
- **Then** 2 units stay on the removal list

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

#### Scenario: Monthly task is shown when due

- **Given** the monthly task is due or open
- **When** the user opens the system
- **Then** the system shows the task to do

### US-19.2 — Complete removal list

#### Scenario: List is completed

- **Given** all removal lines are removed
- **When** the last line is confirmed
- **Then** the system sets the removal list to Completed

#### Scenario: Too many removed

- **Given** 5 units are on the list
- **When** the worker enters 6 removed
- **Then** the system rejects the quantity
