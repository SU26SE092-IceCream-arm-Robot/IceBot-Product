# Business Flows

This document captures project-level business flows for IceBot. It is shared by backend, frontend, tablet, and IoT/edge work.

Backend-specific system flows live in `IceBot-Backend/docs/flows/SYSTEM_FLOWS.md`.

Detailed product actors, scope, operating ownership, workspace behavior, and
task-readiness rules live in [Product And Operations](README.md).

## Current Business Assumptions

- IceBot is a multi-location automated vending system.
- Customer ordering does not require a customer account.
- Customer uses a tablet at the kiosk.
- First payment method is bank transfer QR.
- The tablet and robot kiosk are separate systems on the local network.
- Payment success does not guarantee robot execution.
- A paid fulfillment issue requires staff inspection; successful and uncertain
  output must not be erased by a generic failure state.
- Manual staff compensation is the current phase when compensation is required.
- Auto refund/provider payout is future work.

## Customer Happy Path

```text
1. Customer opens Tablet at kiosk.
2. Customer sees currently available menu items.
3. Customer selects items and confirms checkout.
4. System creates an order.
5. System creates a payment QR/session.
6. Customer pays by bank transfer.
7. Tablet shows payment success.
8. System sends paid order to robot kiosk.
9. Robot prepares the item.
10. Tablet shows order progress.
11. Robot completes the item.
12. Tablet shows ready / pick up.
13. Cloud records completed order for reporting.
```

## Customer Payment Pending Flow

```text
1. Customer confirms checkout.
2. Tablet shows QR payment screen.
3. Customer has not paid yet.
4. Tablet keeps checking payment status.
5. If payment succeeds before expiry, continue to preparation.
6. If payment expires or fails, order remains unpaid/cancelled.
7. Customer can start a new checkout.
```

## Paid Fulfillment Issue

```text
1. Customer pays successfully.
2. Tablet shows payment success / preparing order.
3. Robot kiosk cannot accept or complete one or more production units safely.
4. System preserves payment and execution evidence and opens staff support.
5. Staff inspects the affected item/unit range.
6. Staff delivers confirmed-good output, requests a safe remake, or requests
   compensation according to evidence.
7. Staff records the resolution and any manual refund completion.
```

Current phase:

- Do not promise automatic provider refund.
- Do not use payout terminology for customer refund.
- Current production-incident compensation is full-order; partial-money refund
  is not a current workflow.

## Staff Refund Flow

```text
1. System flags paid order as failed/refund-required.
2. Staff opens admin UI.
3. Staff reviews order/payment/refund amount.
4. Staff gives customer cash refund.
5. Staff marks refund completed.
6. System records:
   - staff account
   - refund amount
   - refund method
   - confirmation time
   - optional note/evidence
```

## Internal Account Flow

```text
1. Bootstrap SystemAdmin is seeded from config/env when no admin exists.
2. Admin logs in with local credentials or Google/Firebase.
3. Admin creates internal accounts manually.
4. Admin assigns roles and scopes:
   - organization
   - store
   - kiosk
5. Internal users access admin/operations features by scoped RBAC.
```

Public signup is disabled for internal system accounts.

## Business Actors And Internal Roles

Customer is anonymous in the current system. Customer does not require an account or an internal role.

Internal roles:

| Actor | Role code | Responsibility |
| --- | --- | --- |
| Platform Admin | `SystemAdmin` | Manage accounts, roles, settings, permissions, security, and overall system health |
| Manager | `Manager` | Monitor kiosks, view reports, manage menu/pricing/promotions, coordinate maintenance |
| Staff | `Staff` | Refill ingredients/materials, clean machine, check on-site status, report issues |
| Technician | `Technician` | Extended operational staff role for initial installation, robot/kiosk setup, technical maintenance, and troubleshooting |
| Organization Admin | `OrgAdmin` | Manage organization-scoped resources and monitor activity/revenue within their assigned organization |

## Menu Management Flow

```text
1. Platform or organization defines products, variants, and recipes.
2. Admin creates menus for global, organization, store, or kiosk scope.
3. Admin adds menu items.
4. Product variants define size/portion/flavor/package variants such as S, M, L.
5. Recipe belongs to the product variant because each variant can use different ingredient quantities.
6. Menu item points to product variant and optional recipe.
7. Menu item defines sellable price, display name, availability window, and status.
8. Tablet/edge runtime projection shows only currently sellable items.
9. Order price is calculated from MenuItem price, not Product or ProductVariant base price.
```

## Robot Configuration Flow

```text
1. Technician installs kiosk and robot.
2. Technician uses Fairino tooling/SDK to teach local points and frames.
3. Cloud stores lightweight centralized configuration/version metadata.
4. Cloud stores recipe execution profiles as backup/config bindings for audit and rollback.
5. Edge keeps local execution source for robot points/frames.
6. Robot programs and execution profiles are synced as complete versioned packages.
7. Edge resolves recipe execution locally and runs robot steps through Fairino integration.
```

Cloud should not send realtime robot motion steps for each order.

The user-facing paths are intentionally split:

- a normal organization workflow installs a production package and operates its
  resulting release;
- a technician/custom authoring workflow imports a Fairino export through one
  guided workspace;
- low-level artifact, contract, and program CRUD are advanced technical tools,
  not the default franchise setup flow.

Read [Robot Authoring Workspace Journey](journeys/ROBOT_AUTHORING_WORKSPACE.md)
and [Production Package Lifecycle](journeys/PRODUCTION_PACKAGE_LIFECYCLE.md)
before assigning a frontend task in either area.

## Reporting Flow

```text
1. Orders and payments are recorded in Cloud.
2. Edge syncs robot execution events and stock movements.
3. Cloud stores analytics/audit/monitoring evidence.
4. Organization/store/kiosk reports are generated from Cloud data.
```

## Business State Summary

| Area | Business meaning |
| --- | --- |
| Order pending payment | Customer placed order but has not paid |
| Payment paid | Provider-confirmed payment succeeded |
| Order paid | Order can be dispatched to robot execution |
| Order accepted | Edge accepted execution |
| Order preparing | Robot is making the item |
| Order completed | Item is completed |
| Order failed after paid | Staff refund/support needed |

## Related Backend Docs

- `IceBot-Backend/ARCHITECTURE.md`
- `IceBot-Backend/docs/flows/SYSTEM_FLOWS.md`
- `IceBot-Backend/docs/iot/IOT_CONTRACT.md`
- `IceBot-Backend/docs/architecture/BOUNDARY_CONTEXTS.md`
- `IceBot-Product/product/README.md`
