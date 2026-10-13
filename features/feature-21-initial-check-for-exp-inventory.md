# Feature: Initial Check for Exp Inventory

**Feature ID:** 21
**Branch pattern:** `feature/21-initial-check-for-exp-inventory`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Check inventory for expired and soon-to-expire items and create a removal list.
**Depends on:** [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-18.1: Find expired inventory

**As an** inventory clerk
**I want to** find inventory that is already expired
**So that** it can be removed.
**Priority:** P3
**Independent test:** Run the check and verify expired inventory is listed. 
**Acceptance Scenarios:** see ### US-18.1 under Gherkin AC

### US-18.2: Find inventory expiring soon

**As an** inventory clerk
**I want to** find inventory that expires soon
**So that** we can sell it first.
**Priority:** P3
**Independent test:** Run the check with a 30 day window and verify soon-to-expire items are listed.
**Acceptance Scenarios:** see ### US-18.2 under Gherkin AC

### US-18.3: Create removal list

**As an** inventory clerk
**I want to** create a removal list of expired inventory
**So that** warehouse workers know what to remove.
**Priority:** P3
**Independent test:** Create a removal list and verify it contains the expired inventory. 
**Acceptance Scenarios:** see ### US-18.3 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST find inventory with an expiration date before today.
- **FR-002:** The system MUST find inventory expiring within a set number of days (default 30).
- **FR-003:** The system MUST show item, slot, quantity, and expiration date.
- **FR-004:** The system MUST create a removal list of expired inventory.
- **FR-005:** The system MUST show Nothing expired when no inventory is expired.
- **FR-006:** The system MUST let an inventory clerk and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-007:** The system MUST work on an office computer.
- **FR-008:** This task is done every month, so the system MUST show it as a task to do when it is due or open.

---

## Key Entities

- **Inventory** — Stock with an expiration date.
- **Removal List** — List of expired inventory to be removed.
- **Removal Line** — One inventory record to remove.

---

## Initial Data Model

### Removal List

- removal_id: String — Required, unique
- created_date: Date — Required
- created_by: Employee reference — Required
- status: String/Enum — Open, Completed

### Removal Line

- removal_line_id: String — Required, unique
- removal_id: Removal List reference — Required
- inventory_id: Inventory reference — Required
- quantity: Integer — Required, greater than zero
- quantity_removed: Integer — Zero or greater

---

## Gherkin AC

### US-18.1 — Find expired inventory

#### Scenario: Expired inventory is found

- **Given** a slot holds an item that expired yesterday
- **When** the clerk runs the expiration check
- **Then** the system lists the item with slot, quantity, and expiration date

#### Scenario: Nothing is expired

- **Given** no inventory is expired
- **When** the clerk runs the expiration check
- **Then** the system displays Nothing expired

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

#### Scenario: Monthly task is shown when due

- **Given** the monthly task is due or open
- **When** the user opens the system
- **Then** the system shows the task to do

### US-18.2 — Find inventory expiring soon

#### Scenario: Expiring soon is found

- **Given** an item expires in 10 days
- **When** the clerk runs the check with a 30 day window
- **Then** the system shows the item as Expiring soon

### US-18.3 — Create removal list

#### Scenario: Removal list is created

- **Given** expired inventory exists
- **When** the clerk creates the removal list
- **Then** the system saves a removal list with status Open
- **And** the list contains each expired inventory record
