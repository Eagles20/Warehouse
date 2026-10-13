# Feature: Maintain Employees

**Feature ID:** 6
**Branch pattern:** `feature/6-maintain-employees`
**Status:** Draft
**Created:** 2026-09-26
**Input:** Maintain employee information and roles.
**Depends on:** None

---

## User Stories

### US-21.1: Add employee

**As a** manager
**I want to** add an employee
**So that** the system can maintain employees.
**Priority:** P1
**Independent test:** Add an employee and verify that it is saved. 
 **Acceptance Scenarios:** see ### US-21.1 under Gherkin AC

### US-21.2: Edit employee

**As a** manager
**I want to** edit an employee
**So that** employee information stays accurate.
**Priority:** P1
**Independent test:** Edit an employee and verify that the change is saved. 
**Acceptance Scenarios:** see ### US-21.2 under Gherkin AC

### US-21.3: Delete employee

**As a** manager
**I want to** delete an employee
**So that** incorrect or unused employees can be removed.
**Priority:** P1
**Independent test:** Delete an employee and verify that it is no longer active. 
**Acceptance Scenarios:** see ### US-21.3 under Gherkin AC

### US-21.4: View employee

**As a** manager
**I want to** view employee information
**So that** I can see who works in each role.
**Priority:** P1
**Independent test:** Open an employee and verify the current information is displayed. 
 **Acceptance Scenarios:** see ### US-21.4 under Gherkin AC

---



## Functional Requirements

- **FR-001:** The system MUST maintain employees.
- **FR-002:** Each employee MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add employees.
- **FR-004:** The system MUST allow authorized users to edit employees.
- **FR-005:** The system MUST allow authorized users to delete employees.
- **FR-006:** The system MUST allow authorized users to view employees.
- **FR-007:** Required employee information MUST be provided before the employee can be saved (name, phone, role).
- **FR-008:** The system MUST reject duplicate employee identifiers.
- **FR-009:** The system MUST NOT delete an employee that is used by a route as its driver, an open pick list or an open inventory count.
- **FR-010:** Deleting an employee MUST only remove it from active records, so other features keep the original employee information.
- **FR-011:** Each employee MUST have exactly one role.
- **FR-012:** The system MUST let a manager use this feature (main user: manager), and MUST NOT let other roles change its information.
- **FR-013:** The system MUST work on an office computer.
- **FR-014:** This feature is not used very often, so the system MUST ask the user to confirm before saving a change.

---



## Key Entities

- **Employee** — A person who works in the warehouse, office, or delivery fleet.

---



## Initial Data Model



### Employee

- employee_id: String — Required, unique
- name: String — Required
- phone: String — Required
- role: String/Enum — Required, Inventory Clerk, Warehouse Worker, Shipping Clerk, Driver, Manager
- is_active: Boolean — Required, false after delete

---



## Gherkin AC



### US-21.1 — Add employee



#### Scenario: Employee is saved

- **Given** an employee does not already exist
- **When** the manager enters the required employee information and saves
- **Then** the system saves the employee



#### Scenario: Employee is not saved due to missing information

- **Given** an employee is being added
- **When** required employee information is missing
- **Then** the system does not save the employee



#### Scenario: Duplicate employee ID is rejected

- **Given** an employee with the same identifier already exists
- **When** the manager tries to add an employee with that identifier
- **Then** the system rejects the duplicate employee



#### Scenario: Invalid role is rejected

- **Given** an employee is being added
- **When** the manager enters a role that is not in the role list
- **Then** the system does not save the employee



#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change



#### Scenario: Confirm before saving

- **Given** the user changes information
- **When** the user clicks save
- **Then** the system asks the user to confirm



### US-21.2 — Edit employee



#### Scenario: Employee is edited successfully

- **Given** an employee exists
- **When** the manager changes employee information and saves
- **Then** the system saves the updated employee



#### Scenario: Employee edit fails

- **Given** an employee exists
- **When** required information is removed during edit
- **Then** the system does not save the update



### US-21.3 — Delete employee



#### Scenario: Employee is deleted successfully

- **Given** an employee exists and is not in use
- **When** the manager deletes the employee
- **Then** the system removes the employee from active records



#### Scenario: Employee used by a route as its driver cannot be deleted

- **Given** an employee is used by a route as its driver
- **When** the manager tries to delete it
- **Then** the system rejects the delete operation
- **And** the employee and the other records stay unchanged



#### Scenario: Employee used by an open pick list cannot be deleted

- **Given** an employee is used by an open pick list
- **When** the manager tries to delete it
- **Then** the system rejects the delete operation
- **And** the employee and the other records stay unchanged



#### Scenario: Employee used by an open inventory count cannot be deleted

- **Given** an employee is used by an open inventory count
- **When** the manager tries to delete it
- **Then** the system rejects the delete operation
- **And** the employee and the other records stay unchanged



#### Scenario: Deleted employee does not change past records

- **Given** an employee was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original employee information



#### Scenario: Delete unknown employee fails

- **Given** the employee does not exist
- **When** the manager tries to delete it
- **Then** the system rejects the delete operation



### US-21.4 — View employee



#### Scenario: Employee information is displayed

- **Given** an employee exists
- **When** the manager views the employee
- **Then** the system displays the employee information

