# Manager Store Operations Journey

This document defines the confirmed responsibility and complete current
IceBot-WebApp implementation boundary of a `Manager` responsible for assigned
organizations, stores, or kiosks. It separates normal business and operational
work from on-site Staff work and Technician-only diagnostics.

## Status

`Confirmed for full current-backend implementation`.

The delivery is one complete task divided into implementation phases. Dashboard,
commercial operations, fulfillment, inventory, maintenance, packages, and
deployment are not separate V1/V2 product versions.

## Operating Goal

A Manager keeps an assigned location commercially ready, accepting orders when
it is safe to do so, and moving operational exceptions to an accountable next
owner.

The role is not a generic administrator. A Manager does not receive global
access and does not automatically become a robot or infrastructure technician.

## Actor And Scope

| Actor | Normal scope | Primary responsibility |
| --- | --- | --- |
| `Manager` | Assigned organization, store, or kiosk | Commercial readiness, current order work, inventory follow-up, operational decisions, and exception coordination |
| `Staff` | Assigned store or kiosk | Physical refill, cleaning, pickup, manual fulfillment, inspection, and customer support |
| `Technician` | Assigned organization, store, or kiosk | Device, endpoint, deployment, robot, diagnostics, and technical recovery |
| `OrgAdmin` | Assigned organization | Organization ownership, store lifecycle, package/release governance, and exceptional approvals |
| `SystemAdmin` | Global | Platform, global catalogs/templates, accounts, security, and cross-tenant intervention |

Every Manager action uses the active assigned scope. The UI must show that scope
and must not ask the Manager to enter internal organization, store, kiosk,
order, device, or deployment identifiers manually.

## Current Backend Capability

Current backend authorization allows a scoped Manager to:

- view management dashboards, stores, kiosks, devices, orders, reports,
  inventory, operational evidence, alerts, maintenance work, deployments, and
  package state;
- manage organization-owned products, variants, recipes, menus, prices,
  promotions, and availability;
- handle payment intervention and manual refund workflows;
- refill or adjust inventory and configure dispenser topology;
- acknowledge/resolve alerts and manage maintenance tickets;
- install Production Packages and request deployment or rollback;
- manage Kiosks, Devices, RobotPrograms, and notification-delivery requeue
  within assigned scope.

The backend still restricts several boundaries:

- account creation and role assignment remain `SystemAdmin` work;
- organization/store lifecycle remains `SystemAdmin` or `OrgAdmin` work;
- global product, ingredient, category, device, artifact-template, and package
  authoring remains platform-owned;
- artifact upload/publication and release publication do not belong to Manager;
- raw operation diagnostics require `SystemAdmin` or `Technician`;
- robot configuration setup requires `SystemAdmin` or `Technician`.

Permission availability describes what the backend currently permits. It does
not prove that every permitted action belongs in the Manager's normal workspace.

## Recommended Responsibility Boundary

The Manager owns operating decisions and follow-up, not every physical or
technical action needed to carry them out.

| Concern | Manager responsibility | Handoff |
| --- | --- | --- |
| Commercial offer | Maintain scoped products, menu placement, prices, availability, and opening/sales policy | Escalate global catalog/template changes to `SystemAdmin` |
| Sales admission | Decide whether a Store/Kiosk should accept new orders | Staff/Technician verifies physical and technical readiness |
| Current fulfillment | Monitor paid, queued, manual, packaged, machine-produced, and pickup work | Staff performs physical fulfillment; Technician handles runtime blockers |
| Payment exception | Review intervention evidence and decide the supported manual action | Staff performs approved customer-facing refund/support work |
| Production incident | Ensure inspection happens and select an evidence-supported resolution | Staff inspects/delivers/discards; Technician reviews unsafe or unknown machine behavior |
| Inventory | Review readiness and stock warnings; assign refill/restock work | Staff refills; Technician repairs topology/device faults |
| Maintenance and alerts | Acknowledge ownership, assign work, monitor resolution, and decide when sales may resume | Technician resolves technical faults; Staff performs cleaning/restocking |
| Package and deployment | Adopt an approved package and request deployment after readiness review | `OrgAdmin` governs publication; Technician resolves technical blockers |

## Operating Loop

### Start Of Shift Or Review Period

```text
Select assigned scope
  -> review Store sales state and opening schedule
  -> review Kiosk accepting-orders/readiness state
  -> review paid and active fulfillment
  -> review payment interventions and production incidents
  -> review inventory warnings, alerts, and maintenance tickets
  -> assign each blocker to Manager, Staff, Technician, or OrgAdmin
```

The Manager first answers:

1. Can this location accept new orders?
2. Are paid or active orders waiting for human action?
3. Is any customer owed pickup, remake, support, or compensation?
4. Does inventory or maintenance require on-site work?
5. Does a technical blocker require a Technician?

### During Normal Operations

- Keep the scoped menu, prices, availability, opening hours, and explicit sales
  pause aligned with the real offer.
- Monitor the current fulfillment queue and prioritize already-paid work.
- Assign refill, restocking, cleaning, packaged fulfillment, and manual
  fulfillment to Staff.
- Follow alert, maintenance, payment-intervention, and production-incident
  queues until each has an accountable owner.
- Use dashboard invalidation/realtime updates as prompts, then read the
  authoritative current state before mutating anything.

### When Sales Must Stop

```text
Detect commercial, operational, or safety blocker
  -> stop accepting new orders through Store/Kiosk sales state
  -> preserve paid and active fulfillment
  -> classify the blocker
  -> assign physical or technical work
  -> wait for authoritative evidence
  -> explicitly resume sales after verification
```

Stopping new orders is not the same as cancelling the queue or stopping a robot.
Store closing, Store sales pause, and Kiosk operational state are distinct
conditions. Existing accepted or running work retains its own lifecycle.

`EmergencyStopRequested` records a Cloud request to hold new work and seek
immediate safety intervention. It is not proof that the physical robot stopped.

### Payment And Fulfillment Exceptions

- A provider-confirmed payment remains financial truth even if fulfillment
  fails.
- A late or duplicate paid transaction goes to intervention/refund handling; it
  must not be silently discarded.
- A partial, defective, or unknown machine output requires inspection of the
  exact item/unit range.
- The Manager must not offer a whole-order retry when only an exact unit is
  eligible for remake.
- Current compensation remains a manual workflow. Production-incident
  compensation is full-order unless a later partial-refund contract is approved.

### Recovery And Resume

Before reopening sales, the Manager verifies that:

- the commercial offer is active and the Store is open or intentionally open;
- the Kiosk is explicitly returned to `Operational`;
- paid and active fulfillment has an accountable state;
- required inventory/topology is ready for the active production definition;
- technical blockers have authoritative recovery evidence;
- resolving or closing a maintenance ticket alone has not been treated as
  readiness proof.

### Handoff

At shift or responsibility handoff, the Manager records or exposes:

- paid work still queued/running;
- customer pickup or support still pending;
- unresolved payment interventions and production incidents;
- inventory work not completed;
- active alerts and maintenance tickets;
- Stores/Kiosks intentionally not accepting orders;
- technical work awaiting Technician evidence.

The handoff should use durable backend work queues and history, not a private
chat message as the only record.

## Recommended Manager Workspace

The normal Manager workspace should prioritize:

1. Active scope and Store/Kiosk sales state.
2. Paid and active fulfillment requiring attention.
3. Production incidents and payment interventions.
4. Inventory warnings and refill/restock assignments.
5. Alerts and maintenance tickets.
6. Commercial readiness: menu, price, availability, opening hours.
7. Package/deployment state when adoption or recovery needs a decision.

Advanced technical details should be linked from a blocker and shown only when
the Manager has the required capability. Raw telemetry, command payloads,
credentials, and low-level artifact composition do not belong in the normal
Manager view.

Dashboard is the attention and routing entry for this broader operating loop.
It is not the boundary of the feature and does not own mutations.

## Required UI States

- No assigned scope.
- Assigned organization has no Store or Kiosk.
- Store closed by schedule versus explicitly sales-paused.
- Kiosk not accepting orders while paid/active fulfillment continues.
- No work versus work hidden by insufficient permission.
- Customer support required but physical output is unknown.
- Manager can see a technical blocker but must hand it to a Technician.
- Recovery was requested but authoritative evidence has not arrived.
- Package/release exists but is not active on the selected Kiosk.

## Implementation Consequences

- No new backend API is required by this decision.
- IceBot-WebApp must implement the complete set of current Manager-permitted
  workflows as one phased delivery task.
- Current Manager permissions for Device/endpoint management, RobotProgram
  management, topology configuration, deployment, and rollback are included,
  but must appear in clearly labelled advanced operations areas.
- `OrgAdmin`-only artifact/release/package-fork actions and Technician-only raw
  diagnostics remain separated by exact policy.
- Any future narrowing of Manager authority requires a backend authorization and
  Product contract change together; hiding a button is not an authorization
  boundary.

## Related Sources

- [Role And Scope Model](../actors-and-scope/ROLE_AND_SCOPE_MODEL.md)
- [Workspace And Dashboard Model](../operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md)
- [Manager Operations Workspace Decision](../decisions/MANAGER_OPERATIONS_WORKSPACE_DECISION.md)
- [Full IceBot-WebApp Implementation Task](../../delivery/tasks/icebot-webapp/MANAGER_OPERATIONS_FULL_IMPLEMENTATION.md)
- [Customer Order And Fulfillment](CUSTOMER_ORDER_AND_FULFILLMENT.md)
- [Technical Operations](TECHNICAL_OPERATIONS.md)
- [Production Package Lifecycle](PRODUCTION_PACKAGE_LIFECYCLE.md)
- [Backend Authorization Rules](../../../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md)
- [Backend Management Read Flow](../../../IceBot-Backend/docs/flows/MANAGEMENT_READ_FLOW.md)
- [Backend Checkout Execution Flow](../../../IceBot-Backend/docs/flows/CHECKOUT_EXECUTION_FLOW.md)
- [Backend Production Incident Resolution](../../../IceBot-Backend/docs/flows/PRODUCTION_INCIDENT_RESOLUTION_FLOW.md)
