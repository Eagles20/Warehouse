# Feature: Maintain Stores

**Feature ID:** 5
**Branch pattern:** `feature/5-maintain-stores`
**Status:** Final
**Created:** 2026-09-26
**Input:** Maintain information about the retail stores we sell to and the route each one is delivered on.
**Depends on:** [Feature 7 — Maintain Routes](feature-7-maintain-routes.md)

---

## User Stories

### US-5.1: Add store

**As an** inventory clerk
**I want to** add a store
**So that** the system can maintain stores.
**Priority:** P1
**Independent test:** Add a store and verify that it is saved. 
**Acceptance Scenarios:** see ### US-5.1 under Gherkin AC

### US-5.2: Edit store

**As an** inventory clerk
**I want to** edit a store
**So that** store information stays accurate.
**Priority:** P1
**Independent test:** Edit a store and verify that the change is saved. 
**Acceptance Scenarios:** see ### US-5.2 under Gherkin AC

### US-5.3: Delete store

**As an** inventory clerk
**I want to** delete a store
**So that** incorrect or unused stores can be removed.
**Priority:** P1
**Independent test:** Delete a store and verify that it is no longer active.
**Acceptance Scenarios:** see ### US-5.3 under Gherkin AC

### US-5.4: View store

**As a** warehouse worker
**I want to** view store information
**So that** I can see where and when to deliver.
**Priority:** P1
**Independent test:** Open a store and verify the current information is displayed. 
**Acceptance Scenarios:** see ### US-5.4 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST maintain stores.
- **FR-002:** Each store MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add stores.
- **FR-004:** The system MUST allow authorized users to edit stores.
- **FR-005:** The system MUST allow authorized users to delete stores.
- **FR-006:** The system MUST allow authorized users to view stores.
- **FR-007:** Required store information MUST be provided before the store can be saved (name, phone, delivery address, route).
- **FR-008:** The system MUST reject duplicate store identifiers.
- **FR-009:** The system MUST NOT delete a store that is used by an open store order or an open shipment.
- **FR-010:** Deleting a store MUST only remove it from active records, so other features keep the original store information.
- **FR-011:** Each store MUST be assigned to one delivery route.
- **FR-012:** The system MUST let an inventory clerk, a warehouse worker, and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-013:** The system MUST work on an office computer.
- **FR-014:** This feature is used every week, so the system MUST show the date each record was last changed.

---

## Key Entities

- **Store** — A retail store that buys products from the warehouse.

---

## Initial Data Model

### Store

- store_id: String — Required, unique
- name: String — Required
- contact_person: String — Optional
- phone: String — Required
- delivery_address: String — Required
- route_id: Route reference — Required
- stop_sequence: Integer — Optional, greater than zero
- is_active: Boolean — Required, false after delete

---

## Gherkin AC

### US-5.1 — Add store

#### Scenario: Store is saved

- **Given** a store does not already exist
- **When** the inventory clerk enters the required store information and saves
- **Then** the system saves the store

#### Scenario: Store is not saved due to missing information

- **Given** a store is being added
- **When** required store information is missing
- **Then** the system does not save the store

#### Scenario: Duplicate store ID is rejected

- **Given** a store with the same identifier already exists
- **When** the inventory clerk tries to add a store with that identifier
- **Then** the system rejects the duplicate store

#### Scenario: Store without a route is rejected

- **Given** a store is being added
- **When** no route is selected
- **Then** the system does not save the store

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-5.2 — Edit store

#### Scenario: Store is edited successfully

- **Given** a store exists
- **When** the inventory clerk changes store information and saves
- **Then** the system saves the updated store

#### Scenario: Store edit fails

- **Given** a store exists
- **When** required information is removed during edit
- **Then** the system does not save the update

### US-5.3 — Delete store

#### Scenario: Store is deleted successfully

- **Given** a store exists and is not in use
- **When** the inventory clerk deletes the store
- **Then** the system removes the store from active records

#### Scenario: Store used by an open store order cannot be deleted

- **Given** a store is used by an open store order
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the store and the other records stay unchanged

#### Scenario: Store used by an open shipment cannot be deleted

- **Given** a store is used by an open shipment
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the store and the other records stay unchanged

#### Scenario: Deleted store does not change past records

- **Given** a store was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original store information

#### Scenario: Delete unknown store fails

- **Given** the store does not exist
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation

### US-5.4 — View store

#### Scenario: Store information is displayed

- **Given** a store exists
- **When** the warehouse worker views the store
- **Then** the system displays the store information

#### Scenario: Last changed date is shown

- **Given** a record was changed last week
- **When** the user views the record
- **Then** the system shows the date it was last changed
