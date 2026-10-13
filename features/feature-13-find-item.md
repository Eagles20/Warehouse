# Feature: Find Item

**Feature ID:** 13
**Branch pattern:** `feature/13-find-item`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Find the slots that hold an item, using UPC or description.
**Depends on:** [Feature 3 — Maintain Items](feature-3-maintain-items.md); [Feature 9 — Maintain Inventory](feature-9-maintain-inventory.md)

---

## User Stories

### US-10.1: Search for an item

**As a** warehouse worker
**I want to** search for an item by UPC or description
**So that** I can find the product quickly.
**Priority:** P1
**Independent test:** Search for an item and verify matching items are shown. 
**Acceptance Scenarios:** see ### US-10.1 under Gherkin AC

### US-10.2: View item locations

**As a** warehouse worker
**I want to** see every slot that holds the item
**So that** I know where to get it.
**Priority:** P1
**Independent test:** Search for an item in stock and verify slots and quantities are shown. 
**Acceptance Scenarios:** see ### US-10.2 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST allow searching items by UPC, SKU, or description.
- **FR-002:** The system MUST list each slot that holds the item with aisle, quantity, received date, and expiration date.
- **FR-003:** The system MUST sort slots by oldest received date first (FIFO).
- **FR-004:** The system MUST show Not in stock when no slot holds the item.
- **FR-005:** The system MUST let a warehouse worker and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-006:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-007:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Item** — Product being searched for.
- **Inventory** — Quantity of the item in each slot.
- **Slot** — Storage location of the item.

---

## Initial Data Model

### Item Location Result (read only)

- item_id: Item reference — From Item
- slot_id: Slot reference — From Inventory
- aisle_number: Integer — From Slot
- quantity_on_hand: Integer — From Inventory
- received_date: Date — From Inventory
- expiration_date: Date — From Inventory

---

## Gherkin AC

### US-10.1 — Search for an item

#### Scenario: Item is found by UPC

- **Given** an item with a UPC exists
- **When** the worker searches by that UPC
- **Then** the system displays the item

#### Scenario: Item is found by description

- **Given** an item with a description exists
- **When** the worker searches by part of the description
- **Then** the system displays matching items

#### Scenario: No item matches

- **Given** no item matches the search
- **When** the worker searches
- **Then** the system displays No items found

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-10.2 — View item locations

#### Scenario: Item is in stock

- **Given** item A is stored in two slots
- **When** the worker views item A
- **Then** the system displays each slot with quantity, received date, and expiration date

#### Scenario: Oldest stock is listed first

- **Given** two slots hold item A with different received dates
- **When** the worker views item A
- **Then** the system lists the slot with the older received date first

#### Scenario: Item is not in stock

- **Given** item B has no inventory
- **When** the worker views item B
- **Then** the system displays Not in stock
