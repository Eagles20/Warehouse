# Feature specifications

Source of truth for what the Warehouse system must do.

**Architecture decisions:** [docs/adr/](../docs/adr/README.md)
**Quality bars:** [docs/nfr/](../docs/nfr/README.md)
**C4 diagrams:** [docs/arch_diagrams/](../docs/arch_diagrams/README.md)
**Screens:** [docs/ui/](../docs/ui/README.md)

## Feature catalog

| ID | File | Branch | Status | Depends on |
|----|------|--------|--------|------------|
| 1 | [feature-1-maintain-company.md](./feature-1-maintain-company.md) | `feature/1-maintain-company` | Draft | — |
| 2 | [feature-2-maintain-warehouse.md](./feature-2-maintain-warehouse.md) | `feature/2-maintain-warehouse` | Draft | Feature 1 |
| 3 | [feature-3-maintain-items.md](./feature-3-maintain-items.md) | `feature/3-maintain-items` | Draft | Features 1, 4 |
| 4 | [feature-4-maintain-suppliers.md](./feature-4-maintain-suppliers.md) | `feature/4-maintain-suppliers` | Draft | Feature 1 |
| 5 | [feature-5-maintain-stores.md](./feature-5-maintain-stores.md) | `feature/5-maintain-stores` | Final | Features 1, 7 |
| 6 | [feature-6-maintain-employess.md](./feature-6-maintain-employess.md) | `feature/6-maintain-employees` | Draft | — |
| 7 | [feature-7-maintain-routes.md](./feature-7-maintain-routes.md) | `feature/7-maintain-routes` | Draft | Features 1, 6 |
| 8 | [feature-8-receive-supplier-orders.md](./feature-8-receive-supplier-orders.md) | `feature/8-receive-supplier-orders` | Draft | Feature 11 |
| 9 | [feature-9-maintain-inventory.md](./feature-9-maintain-inventory.md) | `feature/9-maintain-inventory` | Draft | Features 2, 3 |
| 10 | [feature-10-put-away-supplier-orders.md](./feature-10-put-away-supplier-orders.md) | `feature/10-put-away-supplier-orders` | Draft | Features 8, 9, 14 |
| 11 | [feature-11-create-supplier-orders.md](./feature-11-create-supplier-orders.md) | `feature/11-create-supplier-orders` | Draft | Features 3, 4, 9 |
| 12 | [feature-12-enter-store-order.md](./feature-12-enter-store-order.md) | `feature/12-enter-store-order` | Draft | Features 3, 5, 9 |
| 13 | [feature-13-find-item.md](./feature-13-find-item.md) | `feature/13-find-item` | Draft | Features 3, 9 |
| 14 | [feature-14-find-empty-slot.md](./feature-14-find-empty-slot.md) | `feature/14-find-empty-slot` | Draft | Feature 2 |
| 15 | [feature-15-pick-store-orders.md](./feature-15-pick-store-orders.md) | `feature/15-pick-store-orders` | Draft | Features 9, 12 |
| 16 | [feature-16-move-inventory.md](./feature-16-move-inventory.md) | `feature/16-move-inventory` | Draft | Features 9, 14 |
| 17 | [feature-17-ship-store-orders.md](./feature-17-ship-store-orders.md) | `feature/17-ship-store-orders` | Draft | Features 7, 15 |
| 18 | [feature-18-receive-store-orders.md](./feature-18-receive-store-orders.md) | `feature/18-receive-store-orders` | Draft | Feature 17 |
| 19 | [feature-19-initiate-inventory-count.md](./feature-19-initiate-inventory-count.md) | `feature/19-initiate-inventory-count` | Draft | Features 2, 9 |
| 20 | [feature-20-count-inventory.md](./feature-20-count-inventory.md) | `feature/20-count-inventory` | Draft | Feature 19 |
| 21 | [feature-21-initial-check-for-exp-inventory.md](./feature-21-initial-check-for-exp-inventory.md) | `feature/21-initial-check-for-exp-inventory` | Draft | Feature 9 |
| 22 | [feature-22-remove-expired-inventory.md](./feature-22-remove-expired-inventory.md) | `feature/22-remove-expired-inventory` | Draft | Feature 21 |
| 23 | [feature-23-view-reports.md](./feature-23-view-reports.md) | `feature/23-view-reports` | Final | Features 9, 12, 18 |

## Related

- [Docs index](../docs/README.md)
