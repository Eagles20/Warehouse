# Feature: Find Empty Slot

**Feature ID:** 14
**Branch pattern:** `feature/14-find-empty-slot`
**Status:** Draft
**Created:** 2026-09-28
**Input:** Find an empty storage slot for items that need to be put away.
**Depends on:** [Feature 2 — Maintain Warehouse](feature-2-maintain-warehouse.md)

---

## User Stories

### US-11.1: Find empty slot

**As a** warehouse worker
**I want to** find an empty slot in a warehouse
**So that** I can store received items.
**Priority:** P1
**Independent test:** Search for an empty slot and verify an Empty slot is suggested. 
**Acceptance Scenarios:** see ### US-11.1 under Gherkin AC

### US-11.2: No empty slot available

**As a** warehouse worker
**I want to** be told when no empty slot exists
**So that** I can ask the clerk for help.
**Priority:** P1
**Independent test:** Fill every slot and verify the system shows a message. 
**Acceptance Scenarios:** see ### US-11.2 under Gherkin AC

---

## Functional Requirements

- **FR-001:** The system MUST list slots with status Empty in a chosen warehouse.
- **FR-002:** The system MUST suggest the first empty slot in aisle order.
- **FR-003:** The system MUST NOT suggest a slot with status Occupied.
- **FR-004:** The system MUST show No empty slots when none exist.
- **FR-005:** The system MUST let a warehouse worker and a manager use this feature (main user: warehouse worker), and MUST NOT let other roles change its information.
- **FR-006:** The system MUST be easy to use on the warehouse floor, on a handheld device or a tablet.
- **FR-007:** This feature is used every day, so each screen MUST need as few steps and clicks as possible.

---

## Key Entities

- **Slot** — Storage location that can be Empty or Occupied.
- **Warehouse** — Warehouse that contains the slots.

---

## Initial Data Model

### Slot (read only)

- slot_id: String — From Slot
- warehouse_id: Warehouse reference — From Slot
- aisle_number: Integer — From Slot
- status: String/Enum — Empty, Occupied

---

## Gherkin AC

### US-11.1 — Find empty slot

#### Scenario: Empty slot is found

- **Given** a slot has status Empty
- **When** the worker searches for an empty slot
- **Then** the system suggests that slot

#### Scenario: Occupied slots are skipped

- **Given** the first slot is Occupied and the second slot is Empty
- **When** the worker searches for an empty slot
- **Then** the system suggests the second slot

#### Scenario: Wrong role cannot change information

- **Given** a driver is signed in
- **When** the driver tries to change information in this feature
- **Then** the system rejects the change

### US-11.2 — No empty slot available

#### Scenario: Warehouse is full

- **Given** every slot is Occupied
- **When** the worker searches for an empty slot
- **Then** the system displays No empty slots
