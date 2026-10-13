# Feature: Maintain Suppliers

**Feature ID:** 4  
**Branch pattern:** `feature/4-maintain-suppliers`  
**Status:** Draft  
**Created:** 2026-09-26 
**Input:** Maintain supplier information used to place supplier orders.  
**Depends on:** None

---

## User Stories

### US-4.1: Add supplier

**As an** inventory clerk
**I want to** add a supplier
**So that** the system can maintain suppliers.
**Priority:** P1
**Independent test:** Add a supplier and verify that it is saved. 
**Acceptance Scenarios:** see ### US-4.1 under Gherkin AC

### US-4.2: Edit supplier

**As an** inventory clerk
**I want to** edit a supplier
**So that** supplier information stays accurate.
**Priority:** P1
**Independent test:** Edit a supplier and verify that the change is saved. 
**Acceptance Scenarios:** see ### US-4.2 under Gherkin AC

### US-4.3: Delete supplier

**As an** inventory clerk
**I want to** delete a supplier
**So that** incorrect or unused suppliers can be removed.
**Priority:** P1
**Independent test:** Delete a supplier and verify that it is no longer active. 
**Acceptance Scenarios:** see ### US-4.3 under Gherkin AC

### US-4.4: View supplier

**As a** warehouse worker
**I want to** view supplier information
**So that** I can contact the supplier when I place an order.
**Priority:** P1
**Independent test:** Open a supplier and verify the current information is displayed. 
**Acceptance Scenarios:** see ### US-4.4 under Gherkin AC

---



## Functional Requirements

- **FR-001:** The system MUST maintain suppliers.
- **FR-002:** Each supplier MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add suppliers.
- **FR-004:** The system MUST allow authorized users to edit suppliers.
- **FR-005:** The system MUST allow authorized users to delete suppliers.
- **FR-006:** The system MUST allow authorized users to view suppliers.
- **FR-007:** Required supplier information MUST be provided before the supplier can be saved (name, contact person, phone, address).
- **FR-008:** The system MUST reject duplicate supplier identifiers.
- **FR-009:** The system MUST NOT delete a supplier that is used by an item or an open supplier order.
- **FR-010:** Deleting a supplier MUST only remove it from active records, so other features keep the original supplier information.
- **FR-011:** The system MUST NOT allow a supplier order for a supplier that does not exist.
- **FR-012:** The system MUST let an inventory clerk, a warehouse worker, and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-013:** The system MUST work on an office computer.
- **FR-014:** This feature is used every week, so the system MUST show the date each record was last changed.

---



## Key Entities

- **Supplier** — A vendor that sells products to the warehouse.

---



## Initial Data Model



### Supplier

- supplier_id: String — Required, unique
- name: String — Required
- contact_person: String — Required
- phone: String — Required
- fax: String — Optional
- email: String — Optional
- address: String — Required
- is_active: Boolean — Required, false after delete

---



## Gherkin AC



### US-4.1 — Add supplier



#### Scenario: Supplier is saved

- **Given** a supplier does not already exist
- **When** the inventory clerk enters the required supplier information and saves
- **Then** the system saves the supplier



#### Scenario: Supplier is not saved due to missing information

- **Given** a supplier is being added
- **When** required supplier information is missing
- **Then** the system does not save the supplier



#### Scenario: Duplicate supplier ID is rejected

- **Given** a supplier with the same identifier already exists
- **When** the inventory clerk tries to add a supplier with that identifier
- **Then** the system rejects the duplicate supplier



#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change



### US-4.2 — Edit supplier



#### Scenario: Supplier is edited successfully

- **Given** a supplier exists
- **When** the inventory clerk changes supplier information and saves
- **Then** the system saves the updated supplier



#### Scenario: Supplier edit fails

- **Given** a supplier exists
- **When** required information is removed during edit
- **Then** the system does not save the update



### US-4.3 — Delete supplier



#### Scenario: Supplier is deleted successfully

- **Given** a supplier exists and is not in use
- **When** the inventory clerk deletes the supplier
- **Then** the system removes the supplier from active records



#### Scenario: Supplier used by an item cannot be deleted

- **Given** a supplier is used by an item
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the supplier and the other records stay unchanged



#### Scenario: Supplier used by an open supplier order cannot be deleted

- **Given** a supplier is used by an open supplier order
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the supplier and the other records stay unchanged



#### Scenario: Deleted supplier does not change past records

- **Given** a supplier was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original supplier information



#### Scenario: Delete unknown supplier fails

- **Given** the supplier does not exist
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation



### US-4.4 — View supplier



#### Scenario: Supplier information is displayed

- **Given** a supplier exists
- **When** the warehouse worker views the supplier
- **Then** the system displays the supplier information



#### Scenario: Last changed date is shown

- **Given** a record was changed last week
- **When** the user views the record
- **Then** the system shows the date it was last changed

