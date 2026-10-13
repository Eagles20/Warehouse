# Feature: Maintain Items

**Feature ID:** 3
**Branch pattern:** `feature/3-maintain-items`
**Status:** Draft
**Created:** 2026-09-26
**Input:** Maintain item information, UPC numbers, prices, and minimum and maximum quantities.
**Depends on:** [Feature 4 — Maintain Suppliers](feature-4-maintain-suppliers.md)

---

## User Stories

### US-3.1: Add item

**As an** inventory clerk
**I want to** add an item
**So that** the system can maintain items.
**Priority:** P1
**Independent test:** Add an item and verify that it is saved. 
**Acceptance Scenarios:** see ### US-3.1 under Gherkin AC

### US-3.2: Edit item

**As an** inventory clerk
**I want to** edit an item
**So that** item information stays accurate.
**Priority:** P1
**Independent test:** Edit an item and verify that the change is saved. 
**Acceptance Scenarios:** see ### US-3.2 under Gherkin AC

### US-3.3: Delete item

**As an** inventory clerk
**I want to** delete an item
**So that** incorrect or unused items can be removed.
**Priority:** P1
**Independent test:** Delete an item and verify that it is no longer active. 
**Acceptance Scenarios:** see ### US-3.3 under Gherkin AC

### US-3.4: View item

**As a** warehouse worker
**I want to** view item information
**So that** I can identify the product.
**Priority:** P1
**Independent test:** Open an item and verify the current information is displayed. 
**Acceptance Scenarios:** see ### US-3.4 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST maintain items.
- **FR-002:** Each item MUST have a unique identifier.
- **FR-003:** The system MUST allow authorized users to add items.
- **FR-004:** The system MUST allow authorized users to edit items.
- **FR-005:** The system MUST allow authorized users to delete items.
- **FR-006:** The system MUST allow authorized users to view items.
- **FR-007:** Required item information MUST be provided before the item can be saved (SKU, UPC, description, supplier, cost, price, minimum quantity, maximum quantity).
- **FR-008:** The system MUST reject duplicate item identifiers.
- **FR-009:** The system MUST NOT delete an item that is used by an inventory record, an open supplier order or an open store order.
- **FR-010:** Deleting an item MUST only remove it from active records, so other features keep the original item information.
- **FR-011:** The system MUST reject duplicate SKU values.
- **FR-012:** The system MUST reject duplicate UPC values.
- **FR-013:** Maximum quantity MUST be greater than or equal to minimum quantity.
- **FR-014:** The system MUST keep minimum and maximum quantity so supplier orders can be calculated.
- **FR-015:** The system MUST let an inventory clerk, a warehouse worker, and a manager use this feature (main user: inventory clerk), and MUST NOT let other roles change its information.
- **FR-016:** The system MUST work on an office computer.
- **FR-017:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Item** — A product bought from a supplier and sold to stores.

---

## Initial Data Model

### Item

- item_id: String — Required, unique
- sku: String — Required, unique
- upc: String — Required, unique
- description: String — Required
- category: String/Enum — Grocery, Candy, Snack, Beauty, Auto Supply
- supplier_id: Supplier reference — Required
- unit_cost: Decimal — Required, zero or greater
- price: Decimal — Required, zero or greater
- case_quantity: Integer — Optional, greater than zero
- minimum_quantity: Integer — Required, zero or greater
- maximum_quantity: Integer — Required, greater than or equal to minimum
- shelf_life_days: Integer — Optional, greater than zero
- is_active: Boolean — Required, false after delete

---

## Gherkin AC

### US-3.1 — Add item

#### Scenario: Item is saved

- **Given** an item does not already exist
- **When** the inventory clerk enters the required item information and saves
- **Then** the system saves the item

#### Scenario: Item is not saved due to missing information

- **Given** an item is being added
- **When** required item information is missing
- **Then** the system does not save the item

#### Scenario: Duplicate item ID is rejected

- **Given** an item with the same identifier already exists
- **When** the inventory clerk tries to add an item with that identifier
- **Then** the system rejects the duplicate item

#### Scenario: Duplicate SKU is rejected

- **Given** an item with a SKU already exists
- **When** the clerk tries to add another item with the same SKU
- **Then** the system rejects the duplicate SKU

#### Scenario: Duplicate UPC is rejected

- **Given** an item with a UPC already exists
- **When** the clerk tries to add another item with the same UPC
- **Then** the system rejects the duplicate UPC

#### Scenario: Maximum is less than minimum

- **Given** an item is being added
- **When** the clerk enters a minimum of 15 and a maximum of 10
- **Then** the system does not save the item

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-3.2 — Edit item

#### Scenario: Item is edited successfully

- **Given** an item exists
- **When** the inventory clerk changes item information and saves
- **Then** the system saves the updated item

#### Scenario: Item edit fails

- **Given** an item exists
- **When** required information is removed during edit
- **Then** the system does not save the update

### US-3.3 — Delete item

#### Scenario: Item is deleted successfully

- **Given** an item exists and is not in use
- **When** the inventory clerk deletes the item
- **Then** the system removes the item from active records

#### Scenario: Item used by an inventory record cannot be deleted

- **Given** an item is used by an inventory record
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the item and the other records stay unchanged

#### Scenario: Item used by an open supplier order cannot be deleted

- **Given** an item is used by an open supplier order
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the item and the other records stay unchanged

#### Scenario: Item used by an open store order cannot be deleted

- **Given** an item is used by an open store order
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation
- **And** the item and the other records stay unchanged

#### Scenario: Deleted item does not change past records

- **Given** an item was deleted and past records refer to it
- **When** someone views those past records
- **Then** the system still shows the original item information

#### Scenario: Delete unknown item fails

- **Given** the item does not exist
- **When** the inventory clerk tries to delete it
- **Then** the system rejects the delete operation

### US-3.4 — View item

#### Scenario: Item information is displayed

- **Given** an item exists
- **When** the warehouse worker views the item
- **Then** the system displays the item information
