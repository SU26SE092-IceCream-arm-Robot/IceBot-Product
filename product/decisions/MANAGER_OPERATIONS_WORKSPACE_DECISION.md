# Manager Operations Workspace Decision

## Status

Confirmed for IceBot-WebApp implementation on 2026-07-27.

## Problem

The backend already exposes a broad set of scoped commercial and operational
capabilities for `Manager`, with related organization-governance capabilities
for `OrgAdmin`. Implementing only a small dashboard would leave the operating
journey fragmented across incomplete screens and force users to understand
backend modules instead of completing store work.

The implementation needs one complete delivery program, not separate V1/V2
product versions.

## Chosen Boundary

IceBot-WebApp will implement one **Manager Operations Workspace** program:

- `Manager` is the primary day-to-day actor;
- `OrgAdmin` participates where the current backend policy permits
  organization governance;
- every operation uses the current backend role policy and effective assigned
  organization/Store/Kiosk scope;
- Dashboard is the attention and routing entry, not the entire feature;
- all current backend-supported Manager workflows are included in the same
  delivery task and divided into implementation phases;
- mutations remain in their owning modules;
- advanced technical functions remain visibly separate from normal operations,
  even when the backend currently permits Manager access;
- no future backend capability is invented to make a phase appear complete.

The task is complete only when the included backend capability matrix has been
audited and every applicable capability is implemented or explicitly reported
as blocked by a missing current contract.

## Why

The user needs to operate a Store, not merely view attention counts. A useful
workspace must connect commercial setup, active fulfillment, payment
intervention, inventory, Kiosk readiness, maintenance, package adoption, and
deployment evidence.

Phases are used to control implementation risk and verification order. They are
not product versions and do not permit shipping a permanently partial role
model as the final result.

## Included Current Backend Areas

- identity, current account, effective role/scope, and scoped navigation;
- Store/Kiosk setup and readiness;
- Dashboard and reports;
- organization-owned Product, Variant, Option, Recipe, Menu, pricing,
  promotion, and availability;
- orders, payments, manual refund intervention, fulfillment, and production
  incident resolution;
- Kiosk sales state, Device management, execution endpoints, operational
  evidence, inventory, and topology;
- alerts, maintenance tickets, and failed notification requeue where permitted;
- organization RobotPrograms and guided authoring where permitted;
- Production Package installation/repair/upgrade;
- release review, deployment, activation monitoring, and rollback where
  permitted;
- consistent failure routing, authorization, loading, empty, partial, retry,
  concurrency, and stale-state behavior.

## Excluded

- customer Kiosk checkout UI;
- IoT ingest, provider webhook, MQTT transport, and direct Edge command APIs;
- SystemAdmin-only global catalog/template/security work;
- Technician-only raw diagnostics and credentials;
- automatic provider refunds, because the current workflow remains manual;
- new backend endpoints, GraphQL fields, entities, or scope selectors;
- speculative future capability not present in current backend contracts.

## Role And Scope Consequences

- IceBot-WebApp uses exact backend roles:
  `SystemAdmin`, `OrgAdmin`, `Manager`, `Staff`, `Technician`.
- `LocationOwner`, `LOCATION_OWNER`, and generic `ADMIN` are removed as
  authorization aliases.
- Multiple role/scope assignments are preserved. One display-role priority
  cannot decide authorization.
- Frontend visibility is conservative; backend authorization remains final.
- `OrgAdmin` and `Manager` do not receive identical actions. Each screen and
  command follows its exact backend policy.
- Raw internal IDs are never requested from the user when they can be selected
  or inferred from accessible hierarchy.

## Rejected Alternatives

- Dashboard-only V1 followed by undefined later versions: rejected because it
  does not deliver the current operating role.
- Generic workflow engine: rejected because each backend workflow owns distinct
  lifecycle, authorization, idempotency, and recovery rules.
- One universal Admin workspace: rejected because SystemAdmin, OrgAdmin,
  Manager, Staff, and Technician have different scope and responsibility.
- Mirroring every controller as a menu item: rejected because navigation must
  follow user work and capability ownership.

## Implementation Consequences

- One large phased task owns the complete frontend program.
- Existing `FLOW-*` and `CAP-*` contracts remain the traceability units inside
  that task.
- A phase may be verified independently, but the overall task remains
  incomplete until all included phases pass their completion gates.
- Target status is updated capability by capability from repository evidence;
  no capability is marked complete based on page existence alone.

## Related Sources

- [Manager Store Operations](../journeys/MANAGER_STORE_OPERATIONS.md)
- [Workspace And Dashboard Model](../operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md)
- [IceBot-WebApp Manager Operations Task](../../delivery/tasks/icebot-webapp/MANAGER_OPERATIONS_FULL_IMPLEMENTATION.md)
- [Backend Authorization Rules](../../../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md)
