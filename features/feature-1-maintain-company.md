# Feature: Maintain Company

**Feature ID:** 1
**Branch pattern:** `feature/1-maintain-company`
**Status:** Draft
**Created:** 2026-09-26
**Input:** Maintain company information for the Warehouse system.
**Depends on:** None

------

## User Stories

### US-1.1: Add company

**As an** inventory clerk
**I want to** add a company
**So that** the system can maintain companies.
**Priority:** P1
**Independent test:** Add a company and verify that it is saved. 
**Acceptance Scenarios:** see ### US-1.1 under Gherkin AC

### US-1.2: Edit company

**As an** inventory clerk
**I want to** edit a company
**So that** company information stays accurate.
**Priority:** P1
**Independent test:** Edit a company and verify that the change is saved. 
**Acceptance Scenarios:** see ### US-1.2 under Gherkin AC

### US-1.3: Delete company

**As an** inventory clerk
**I want to** delete a company
**So that** incorrect or unused companies can be removed.
**Priority:** P1
**Independent test:** Delete a company and verify that it is no longer active. 
**Acceptance Scenarios:** see ### US-1.3 under Gherkin AC

### US-1.4: View company

**As an** inventory clerk
**I want to** view company information
**So that** I can see the current company information.
**Priority:** P1
**Independent test:** Open a company and verify the current information is displayed. 
**Acceptance Scenarios:** see ### US-1.4 under Gherkin AC

-----

## Functional Requirements

- **FR-001:** The system MUST maintain companies.
- **FR-002:** Each company MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add companies.
- **FR-004:** The system MUST allow authorized users to edit companies.
- **FR-005:** The system MUST allow authorized users to delete companies.
- **FR-006:** The system MUST allow authorized users to view companies.
- **FR-007:** Required company information MUST be provided before the company can be saved (name, address, phone).
- **FR-008:** The system MUST reject duplicate company identifiers.
- **FR-009:** The system MUST NOT delete a company that is used by a warehouse.
- **FR-010:** Deleting a company MUST only remove it from active records, so other features keep the original company information.
- **FR-011:** The system MUST let an inventory clerk and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-012:** The system MUST work on an office computer.
- **FR-013:** This feature is not used very often, so the system MUST ask the user to confirm before saving a change.

-----

## Key Entities

- **Company** — The company operating the warehouses.

-----

## Initial Data Model

### Company

- company_id: String — Required, unique
- name: String — Required
- address: String — Required
- phone: String — Required
- email: String — Optional
- is_active: Boolean — Required, false after delete

-----

## Gherkin AC

### US-1.1 — Add company

#### Scenario: Company is saved

- **Given** a company does not already exist
- **When** the inventory clerk enters the required company information and saves
- **Then** the system saves the company

#### Scenario: Company is not saved due to missing information

- **Given** a company is being added
- **When** required company information is missing
- **Then** the system does not save the company

#### Scenario: Duplicate company ID is rejected

- **Given** a company with the same identifier already exists
- **When** the inventory clerk tries to add a company with that identifier
- **Then** the system rejects the duplicate company

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

#### Scenario: Confirm before saving

- **Given** the user changes information
- **When** the user clicks save
- **Then** the system asks the user to confirm

### US-1.2 — Edit company

#### Scenario: Company is edited successfully

- **Given** a company exists
- **When** the inventory clerk changes company information and saves
- **Then** the system saves the updated company

#### Scenario: Company edit fails

- **Given** a company exists
- **When** required information is removed during edit
- **Then** the system does not save the update

### US-1.3 — Delete company

#### Scenario: Company is deleted successfully

- **Given** a company exists and is not in use
- **When** the inventory clerk deletes the company
- **Then** the system removes the company from active records

#### Scenario: Company used by a warehouse cannot be deleted

- **Given** a company is used by a warehouse
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the company and the other records stay unchanged

#### Scenario: Deleted company does not change past records

- **Given** a company was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original company information

#### Scenario: Delete unknown company fails

- **Given** the company does not exist
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation

### US-1.4 — View company

#### Scenario: Company information is displayed

- **Given** a company exists
- **When** the inventory clerk views the company
- **Then** the system displays the company information
