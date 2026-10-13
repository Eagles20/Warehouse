# Feature: Maintain Warehouse

**Feature ID:** 2
**Branch pattern:** `feature/2-maintain-warehouse`
**Status:** Draft
**Created:** 2026-09-25
**Input:** Maintain warehouse information, numbered aisles, and storage slots.
**Depends on:** [Feature 1 — Maintain Company](feature-1-maintain-company.md)

---

## User Stories

### US-2.1: Add warehouse

**As an** inventory clerk
**I want to** add a warehouse
**So that** the system can maintain warehouses.
**Priority:** P1
**Independent test:** Add a warehouse and verify that it is saved.
**Acceptance Scenarios:** see ### US-2.1 under Gherkin AC

### US-2.2: Edit warehouse

**As an** inventory clerk
**I want to** edit a warehouse
**So that** warehouse information stays accurate.
**Priority:** P1
**Independent test:** Edit a warehouse and verify that the change is saved.
**Acceptance Scenarios:** see ### US-2.2 under Gherkin AC

### US-2.3: Delete warehouse

**As an** inventory clerk
**I want to** delete a warehouse
**So that** incorrect or unused warehouses can be removed.
**Priority:** P1
**Independent test:** Delete a warehouse and verify that it is no longer active. 
**Acceptance Scenarios:** see ### US-2.3 under Gherkin AC

### US-2.4: View warehouse

**As a** warehouse worker
**I want to** view warehouse information
**So that** I can see where inventory is stored.
**Priority:** P1
**Independent test:** Open a warehouse and verify the current information is displayed. 
**Acceptance Scenarios:** see ### US-2.4 under Gherkin AC

### US-2.5: Add storage slot

**As an** inventory clerk
**I want to** add a storage slot to a numbered aisle
**So that** every item has a place to be stored.
**Priority:** P1
**Independent test:** Add a slot to a warehouse and verify it is saved as Empty. 
**Acceptance Scenarios:** see ### US-2.5 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST maintain warehouses.
- **FR-002:** Each warehouse MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add warehouses.
- **FR-004:** The system MUST allow authorized users to edit warehouses.
- **FR-005:** The system MUST allow authorized users to delete warehouses.
- **FR-006:** The system MUST allow authorized users to view warehouses.
- **FR-007:** Required warehouse information MUST be provided before the warehouse can be saved (name, address).
- **FR-008:** The system MUST reject duplicate warehouse identifiers.
- **FR-009:** The system MUST NOT delete a warehouse that is used by inventory in its slots or an open inventory count.
- **FR-010:** Deleting a warehouse MUST only remove it from active records, so other features keep the original warehouse information.
- **FR-011:** The system MUST allow authorized users to add numbered aisles and storage slots (aisle, shelf, bin) to a warehouse.
- **FR-012:** A new storage slot MUST start with status Empty.
- **FR-013:** The system MUST reject a duplicate slot in the same warehouse.
- **FR-014:** The system MUST let an inventory clerk, a warehouse worker, and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-015:** The system MUST work on an office computer.
- **FR-016:** This feature is not used very often, so the system MUST ask the user to confirm before saving a change.

---

## Key Entities

- **Warehouse** — A physical warehouse where inventory is stored.
- **Slot** — A numbered storage location (aisle, shelf, bin) inside a warehouse.

---

## Initial Data Model

### Warehouse

- warehouse_id: String — Required, unique
- company_id: Company reference — Required
- name: String — Required
- address: String — Required
- forklift_count: Integer — Optional, zero or greater
- is_active: Boolean — Required, false after delete

### Slot

- slot_id: String — Required, unique
- warehouse_id: Warehouse reference — Required
- aisle_number: Integer — Required, greater than zero
- shelf: String — Required
- bin: String — Required
- status: String/Enum — Empty, Occupied

---

## Gherkin AC

### US-2.1 — Add warehouse

#### Scenario: Warehouse is saved

- **Given** a warehouse does not already exist
- **When** the inventory clerk enters the required warehouse information and saves
- **Then** the system saves the warehouse

#### Scenario: Warehouse is not saved due to missing information

- **Given** a warehouse is being added
- **When** required warehouse information is missing
- **Then** the system does not save the warehouse

#### Scenario: Duplicate warehouse ID is rejected

- **Given** a warehouse with the same identifier already exists
- **When** the inventory clerk tries to add a warehouse with that identifier
- **Then** the system rejects the duplicate warehouse

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

#### Scenario: Confirm before saving

- **Given** the user changes information
- **When** the user clicks save
- **Then** the system asks the user to confirm

### US-2.2 — Edit warehouse

#### Scenario: Warehouse is edited successfully

- **Given** a warehouse exists
- **When** the inventory clerk changes warehouse information and saves
- **Then** the system saves the updated warehouse

#### Scenario: Warehouse edit fails

- **Given** a warehouse exists
- **When** required information is removed during edit
- **Then** the system does not save the update

### US-2.3 — Delete warehouse

#### Scenario: Warehouse is deleted successfully

- **Given** a warehouse exists and is not in use
- **When** the inventory clerk deletes the warehouse
- **Then** the system removes the warehouse from active records

#### Scenario: Warehouse used by inventory in its slots cannot be deleted

- **Given** a warehouse is used by inventory in its slots
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the warehouse and the other records stay unchanged

#### Scenario: Warehouse used by an open inventory count cannot be deleted

- **Given** a warehouse is used by an open inventory count
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the warehouse and the other records stay unchanged

#### Scenario: Deleted warehouse does not change past records

- **Given** a warehouse was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original warehouse information

#### Scenario: Delete unknown warehouse fails

- **Given** the warehouse does not exist
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation

### US-2.4 — View warehouse

#### Scenario: Warehouse information is displayed

- **Given** a warehouse exists
- **When** the warehouse worker views the warehouse
- **Then** the system displays the warehouse information

### US-2.5 — Add storage slot

#### Scenario: Slot is saved as empty

- **Given** a warehouse exists
- **When** the clerk enters an aisle number, shelf, and bin and saves
- **Then** the system saves the slot
- **And** the slot status is Empty

#### Scenario: Duplicate slot is rejected

- **Given** a slot already exists in the warehouse
- **When** the clerk tries to add the same aisle, shelf, and bin
- **Then** the system rejects the duplicate slot
