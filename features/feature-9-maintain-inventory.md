# Feature: Maintain Inventory

**Feature ID:** 9
**Branch pattern:** `feature/9-maintain-inventory`
**Status:** Draft
**Created:** 2026-09-27
**Input:** Maintain the quantity of each item in each storage slot.
**Depends on:** [Feature 2 — Maintain Warehouse](feature-2-maintain-warehouse.md); [Feature 3 — Maintain Items](feature-3-maintain-items.md)

---

## User Stories

### US-23.1: Add inventory record

**As an** inventory clerk
**I want to** add an inventory record
**So that** the system can maintain inventory records.
**Priority:** P1
**Independent test:** Add an inventory record and verify that it is saved. 
 **Acceptance Scenarios:** see ### US-23.1 under Gherkin AC

### US-23.2: Edit inventory record

**As an** inventory clerk
**I want to** edit an inventory record
**So that** inventory record information stays accurate.
**Priority:** P1
**Independent test:** Edit an inventory record and verify that the change is saved. 
**Acceptance Scenarios:** see ### US-23.2 under Gherkin AC

### US-23.3: Delete inventory record

**As an** inventory clerk
**I want to** delete an inventory record
**So that** incorrect or unused inventory records can be removed.
**Priority:** P1
**Independent test:** Delete an inventory record and verify that it is no longer active.
 **Acceptance Scenarios:** see ### US-23.3 under Gherkin AC

### US-23.4: View inventory record

**As a** warehouse worker
**I want to** view inventory record information
**So that** I can see how many items are in each slot.
**Priority:** P1
**Independent test:** Open an inventory record and verify the current information is displayed. 
**Acceptance Scenarios:** see ### US-23.4 under Gherkin AC

---



## Functional Requirements

- **FR-001:** The system MUST maintain inventory records.
- **FR-002:** Each inventory record MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add inventory records.
- **FR-004:** The system MUST allow authorized users to edit inventory records.
- **FR-005:** The system MUST allow authorized users to delete inventory records.
- **FR-006:** The system MUST allow authorized users to view inventory records.
- **FR-007:** Required inventory record information MUST be provided before the inventory record can be saved (item, slot, quantity on hand, received date).
- **FR-008:** The system MUST reject duplicate inventory record identifiers.
- **FR-009:** The system MUST NOT delete an inventory record that is used by an open pick list, an open inventory count or an open removal list.
- **FR-010:** Deleting an inventory record MUST only remove it from active records, so other features keep the original inventory record information.
- **FR-011:** Quantity on hand MUST NOT be negative.
- **FR-012:** The system MUST require a reason when quantity on hand is changed by hand.
- **FR-013:** The system MUST keep the received date so the oldest stock is used first (FIFO).
- **FR-014:** The system MUST let an inventory clerk, a warehouse worker, and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-015:** The system MUST work on an office computer.
- **FR-016:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---



## Key Entities

- **Inventory record** — Quantity of an item stored in one slot.

---



## Initial Data Model



### Inventory record

- inventory_id: String — Required, unique
- item_id: Item reference — Required
- slot_id: Slot reference — Required
- quantity_on_hand: Integer — Required, zero or greater
- received_date: Date — Required
- expiration_date: Date — Optional, required when the item has a shelf life
- is_active: Boolean — Required, false after delete

---



## Gherkin AC



### US-23.1 — Add inventory record



#### Scenario: Inventory record is saved

- **Given** an inventory record does not already exist
- **When** the inventory clerk enters the required inventory record information and saves
- **Then** the system saves the inventory record



#### Scenario: Inventory record is not saved due to missing information

- **Given** an inventory record is being added
- **When** required inventory record information is missing
- **Then** the system does not save the inventory record



#### Scenario: Duplicate inventory record ID is rejected

- **Given** an inventory record with the same identifier already exists
- **When** the inventory clerk tries to add an inventory record with that identifier
- **Then** the system rejects the duplicate inventory record



#### Scenario: Negative quantity is rejected

- **Given** the employee enters a negative quantity on hand
- **When** the employee tries to save the inventory record
- **Then** the system rejects the negative quantity



#### Scenario: Missing slot is rejected

- **Given** no slot is selected
- **When** the employee tries to save the inventory record
- **Then** the system does not save the inventory record



#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change



### US-23.2 — Edit inventory record



#### Scenario: Inventory record is edited successfully

- **Given** an inventory record exists
- **When** the inventory clerk changes inventory record information and saves
- **Then** the system saves the updated inventory record



#### Scenario: Inventory record edit fails

- **Given** an inventory record exists
- **When** required information is removed during edit
- **Then** the system does not save the update



### US-23.3 — Delete inventory record



#### Scenario: Inventory record is deleted successfully

- **Given** an inventory record exists and is not in use
- **When** the inventory clerk deletes the inventory record
- **Then** the system removes the inventory record from active records



#### Scenario: Inventory record used by an open pick list cannot be deleted

- **Given** an inventory record is used by an open pick list
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the inventory record and the other records stay unchanged



#### Scenario: Inventory record used by an open inventory count cannot be deleted

- **Given** an inventory record is used by an open inventory count
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the inventory record and the other records stay unchanged



#### Scenario: Inventory record used by an open removal list cannot be deleted

- **Given** an inventory record is used by an open removal list
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the inventory record and the other records stay unchanged



#### Scenario: Deleted inventory record does not change past records

- **Given** an inventory record was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original inventory record information



#### Scenario: Delete unknown inventory record fails

- **Given** the inventory record does not exist
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation



### US-23.4 — View inventory record



#### Scenario: Inventory record information is displayed

- **Given** an inventory record exists
- **When** the warehouse worker views the inventory record
- **Then** the system displays the inventory record information

