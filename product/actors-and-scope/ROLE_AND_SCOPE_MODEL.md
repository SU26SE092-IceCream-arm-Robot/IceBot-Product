# Role And Scope Model

This document defines the business meaning of internal actors and the tenant
scope required by their work. It is the starting point for screen ownership,
task assignment, and acceptance criteria.

## Confirmed Current Model

Customer ordering is anonymous. A customer is a business actor, not an
internal account role.

Internal access is the combination of a role and an assigned scope. A role
name alone is never enough to infer what data a person may see or change.

```text
Internal account
  -> one or more role assignments
  -> each assignment may be global, organization-scoped, store-scoped,
     or kiosk-scoped
```

## Actors

| Actor / exact role | Primary goal | Usual operating scope | Must not be described as |
| --- | --- | --- | --- |
| `SystemAdmin` | Operate the platform and global catalogs | Global | Organization administrator or store operator by default |
| `OrgAdmin` | Operate one assigned organization | Assigned organization and its descendants | Platform administrator |
| `Manager` | Run business and operations for assigned locations | Assigned organization, store, or kiosk | Global administrator |
| `Staff` | Perform on-site work and customer support | Assigned store or kiosk | Account/tenant administrator |
| `Technician` | Install, diagnose, and maintain technical equipment | Assigned organization, store, or kiosk | Business pricing or platform owner |
| Customer | Buy and collect products at a kiosk | One order through its order-access capability | Internal user |

The same person may hold more than one role. For example, a franchise owner
may be both `OrgAdmin` and `Technician`; this is an explicit access assignment,
not an implication of organization ownership.

## Scope Model

```text
Platform
  -> Organization
      -> Store
          -> Kiosk
```

- A global action affects platform-owned data or all organizations.
- An organization action applies only to the organization selected or assigned.
- A store action applies only to a store within the selected organization.
- A kiosk action applies only to one physical kiosk and its attached runtime,
  devices, topology, telemetry, and current operations.

An interface must show the active scope before a user takes a scoped mutation.
Do not make a user type or copy an internal identifier to establish scope.

## Workspace Expectations

| Actor | Primary workspace | Examples of decisions |
| --- | --- | --- |
| `SystemAdmin` | Platform control | Organization lifecycle, internal accounts, global templates/catalogs, platform health |
| `OrgAdmin` | Organization operations | Organization profile, stores, commercial configuration, package installation, release/deployment approval in assigned scope |
| `Manager` | Store operations | Menu/pricing operations, current orders, inventory work, operational follow-up |
| `Staff` | On-site operations | Refill, cleaning, support, inspection, manual fulfillment, refund support where assigned |
| `Technician` | Technical operations | Kiosk/device readiness, diagnostics, maintenance, endpoint/deployment visibility |

This table defines product intent, not a guarantee that every listed screen
already exists. Exact permission checks remain owned by backend authorization
rules.

## UI And Task Rules

- Never name a screen simply `Admin Dashboard`.
- Name the actor and scope, for example `Organization Operations Dashboard` or
  `Kiosk Technician Diagnostics`.
- A role selector must explain the scope to be assigned and show only valid
  organization/store/kiosk choices.
- A screen may serve several roles only when its permitted actions and data
  are determined by the active role/scope, not by a frontend assumption.
- If a requirement says only "admin", return it for clarification before
  implementing it.

## Related Sources

- [Workspace And Dashboard Model](../operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md)
- [Organization, Store, And Kiosk Operating Model](ORGANIZATION_STORE_KIOSK_OPERATING_MODEL.md)
- [Task Readiness Template](../../delivery/playbooks/TASK_READINESS.md)
- [Backend Authorization Rules](../../../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md)
