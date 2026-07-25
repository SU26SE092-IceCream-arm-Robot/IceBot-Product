# Workspace And Dashboard Model

Dashboards are task-and-scope workspaces. They are not a generic screen for a
person called "admin".

## Required Naming Rule

Do not use these requirement names:

```text
Admin dashboard
Admin panel
Admin can manage everything
```

Use an actor and scope instead:

```text
Platform Control Dashboard for SystemAdmin
Organization Operations Dashboard for OrgAdmin and Manager
Store Operations Workspace for Manager and Staff
Kiosk Technician Diagnostics for Technician
```

## Workspace Model

| Workspace | Actors | Scope | Primary questions |
| --- | --- | --- | --- |
| Platform Control | `SystemAdmin` | Global | Are organizations active? Are global catalogs/packages/technical templates healthy? Are platform-level exceptions blocking many tenants? |
| Organization Operations | `OrgAdmin`, `Manager` | Assigned organization | Which stores and kiosks need attention? Is the commercial offer ready? Which orders/incidents require a decision? |
| Store Operations | `Manager`, `Staff` | Assigned store/kiosk | Can this location take new orders? What needs refill, cleaning, pickup, support, or manual fulfillment now? |
| Technical Operations | `Technician`, permitted `OrgAdmin`/`Manager` | Assigned kiosk | Is the kiosk/runtime/device/deployment healthy enough to operate? Which maintenance and diagnostics actions are allowed? |
| Customer Kiosk | Customer | One kiosk and one order capability | What can be bought now? What is the payment/progress/pickup state of this order? |

The same account can open more than one workspace when it has the required
role and scope. The frontend must derive available workspaces from effective
access and current scope, not from one hard-coded "admin" boolean.

`IceBot-Mobile` is a delivery channel for Store Operations and Technical
Operations work performed beside a kiosk or at an assigned store. It is not a
new role and it must use the same effective role and scope rules as other
clients. Cash collection or payment confirmation is not yet a Mobile contract;
it requires a separately defined payment-settlement workflow before a screen or
backend API is added.

## Dashboard Content Rules

| Workspace | Include | Exclude by default |
| --- | --- | --- |
| Platform Control | organization lifecycle, global catalog/package maintenance, platform health, global intervention queues | tenant commercial detail not needed for platform work |
| Organization Operations | scoped order health, store/kiosk readiness, business incidents, menu/package state, scoped notification follow-up | another organization's data and raw technical payloads |
| Store Operations | current orders, pickup/fulfillment work, inventory warnings, store sales state, local maintenance tickets | global catalog authoring and cross-store reporting unless assigned |
| Technical Operations | kiosk connectivity, device/runtime readiness, deployments, diagnostics, technical maintenance | prices, business reporting, and other tenant data unless separately authorized |

Existing GraphQL management reads provide scoped dashboard/order/kiosk/inventory
read models. REST remains the mutation and integration surface. A dashboard
requirement must name the query/read model it needs and the command routes it
is allowed to invoke.

## Empty And Failure States

Every workspace task must design these states before implementation:

- no assigned organization/store/kiosk scope;
- scope exists but has no Stores or Kiosks yet;
- Store is closed or sales-paused;
- Kiosk cannot accept new orders but has active fulfillment to monitor;
- no data, no permission, and loading/error are distinct states;
- an order has partial completion or requires inspection;
- the user can view a condition but is not allowed to mutate it.

## Acceptance Rule For Dashboard Tasks

A dashboard task is ready only when it states:

```text
Actor and exact role(s)
Active scope and scope selector behavior
Questions the workspace answers
Read model/query source
Allowed actions and their authorization
Empty, unavailable, and failure states
Out-of-scope data/actions
```

## Related Sources

- [Role And Scope Model](../actors-and-scope/ROLE_AND_SCOPE_MODEL.md)
- [Customer Order And Fulfillment Flow](../journeys/CUSTOMER_ORDER_AND_FULFILLMENT.md)
- [Task Readiness Template](../../delivery/playbooks/TASK_READINESS.md)
- [Backend Management Read Flow](../../../IceBot-Backend/docs/flows/MANAGEMENT_READ_FLOW.md)
- [Backend Authorization Rules](../../../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md)
