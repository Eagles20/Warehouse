# Feature: Maintain Routes

**Feature ID:** 7
**Branch pattern:** `feature/7-maintain-routes`
**Status:** Draft
**Created:** 2026-09-27
**Input:** Maintain delivery routes and the driver of each route.
**Depends on:** [Feature 6 — Maintain Employees](feature-6-maintain-employees.md)

---

## User Stories

### US-22.1: Add route

**As a** shipping clerk
**I want to** add a route
**So that** the system can maintain routes.
**Priority:** P1
**Independent test:** Add a route and verify that it is saved. 
 **Acceptance Scenarios:** see ### US-22.1 under Gherkin AC

### US-22.2: Edit route

**As a** shipping clerk
**I want to** edit a route
**So that** route information stays accurate.
**Priority:** P1
**Independent test:** Edit a route and verify that the change is saved. 
 **Acceptance Scenarios:** see ### US-22.2 under Gherkin AC

### US-22.3: Delete route

**As a** shipping clerk
**I want to** delete a route
**So that** incorrect or unused routes can be removed.
**Priority:** P1
**Independent test:** Delete a route and verify that it is no longer active. 
**Acceptance Scenarios:** see ### US-22.3 under Gherkin AC

### US-22.4: View route

**As a** driver
**I want to** view route information
**So that** I can see my route and my stops.
**Priority:** P1
**Independent test:** Open a route and verify the current information is displayed.  
**Acceptance Scenarios:** see ### US-22.4 under Gherkin AC

---



## Functional Requirements

- **FR-001:** The system MUST maintain routes.
- **FR-002:** Each route MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add routes.
- **FR-004:** The system MUST allow authorized users to edit routes.
- **FR-005:** The system MUST allow authorized users to delete routes.
- **FR-006:** The system MUST allow authorized users to view routes.
- **FR-007:** Required route information MUST be provided before the route can be saved (name, driver, delivery day).
- **FR-008:** The system MUST reject duplicate route identifiers.
- **FR-009:** The system MUST NOT delete a route that is used by a store or an open shipment.
- **FR-010:** Deleting a route MUST only remove it from active records, so other features keep the original route information.
- **FR-011:** Only an employee with the Driver role MUST be assigned to a route.
- **FR-012:** The system MUST let a shipping clerk, a driver, and a manager use this feature (main user: shipping clerk), and MUST NOT let other roles change its information.
- **FR-013:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-014:** This feature is used every week, so the system MUST show the date each record was last changed.

---



## Key Entities

- **Route** — A delivery route driven by one driver to a group of stores.

---



## Initial Data Model



### Route

- route_id: String — Required, unique
- name: String — Required
- driver_id: Employee reference — Required, employee role is Driver
- vehicle: String — Optional
- delivery_day: String — Required
- is_active: Boolean — Required, false after delete

---



## Gherkin AC



### US-22.1 — Add route



#### Scenario: Route is saved

- **Given** a route does not already exist
- **When** the shipping clerk enters the required route information and saves
- **Then** the system saves the route



#### Scenario: Route is not saved due to missing information

- **Given** a route is being added
- **When** required route information is missing
- **Then** the system does not save the route



#### Scenario: Duplicate route ID is rejected

- **Given** a route with the same identifier already exists
- **When** the shipping clerk tries to add a route with that identifier
- **Then** the system rejects the duplicate route



#### Scenario: Non-driver cannot be assigned

- **Given** an employee has the role Warehouse Worker
- **When** the clerk assigns the employee to a route
- **Then** the system rejects the assignment



#### Scenario: Wrong role cannot change information

- **Given** a warehouse worker is signed in
- **When** the warehouse worker tries to change information in this feature
- **Then** the system rejects the change



### US-22.2 — Edit route



#### Scenario: Route is edited successfully

- **Given** a route exists
- **When** the shipping clerk changes route information and saves
- **Then** the system saves the updated route



#### Scenario: Route edit fails

- **Given** a route exists
- **When** required information is removed during edit
- **Then** the system does not save the update



### US-22.3 — Delete route



#### Scenario: Route is deleted successfully

- **Given** a route exists and is not in use
- **When** the shipping clerk deletes the route
- **Then** the system removes the route from active records



#### Scenario: Route used by a store cannot be deleted

- **Given** a route is used by a store
- **When** the shipping clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the route and the other records stay unchanged



#### Scenario: Route used by an open shipment cannot be deleted

- **Given** a route is used by an open shipment
- **When** the shipping clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the route and the other records stay unchanged



#### Scenario: Deleted route does not change past records

- **Given** a route was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original route information



#### Scenario: Delete unknown route fails

- **Given** the route does not exist
- **When** the shipping clerk tries to delete it
- **Then** the system rejects the delete operation



### US-22.4 — View route



#### Scenario: Route information is displayed

- **Given** a route exists
- **When** the driver views the route
- **Then** the system displays the route information



#### Scenario: Last changed date is shown

- **Given** a record was changed last week
- **When** the user views the record
- **Then** the system shows the date it was last changed

