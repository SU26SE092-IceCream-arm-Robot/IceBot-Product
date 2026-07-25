# Organization, Store, And Kiosk Operating Model

This document describes the business hierarchy through which IceBot is sold,
configured, and operated. It intentionally separates commercial ownership from
physical operating location.

## Confirmed Current Model

```text
Platform
  -> Organization
      -> Store
          -> Kiosk
              -> local runtime, devices, inventory topology, and production work
```

- `SystemAdmin` creates, activates, or disables organizations.
- An organization can be created before a store exists; Store creation is a
  separate organization-scoped action.
- A store belongs to one organization and provides local sales rules such as
  opening hours and sales pause.
- A kiosk belongs to one store and is the physical sales and fulfillment point.
- Orders are placed for a kiosk. The order is therefore attributable to its
  kiosk, store, and organization.
- Kiosk devices, execution endpoints, inventory topology, telemetry, and
  deployments are kiosk-owned operational resources.
- Organization-owned commercial and production configuration may be selected
  for a kiosk, but deployment and readiness are evaluated at the kiosk.

## Operating Ownership

| Concern | Business owner / scope | Product meaning |
| --- | --- | --- |
| Organization profile and internal access | Organization | The franchise/company boundary for users and commercial configuration |
| Store hours and sales pause | Store | Whether the location accepts new sales; not a command to stop running production |
| Kiosk operational state | Kiosk | Whether that physical sales point accepts new work |
| Devices, inventory topology, runtime readiness | Kiosk | Whether the physical setup can execute and support a sale |
| Product, menu, price, and package selection | Organization with scoped placement | What can be offered at a location |
| Order, payment, fulfillment, and incident | Kiosk-originated, retained in tenant history | What happened for a particular customer sale |

## Onboarding Flow

```text
SystemAdmin creates Organization
  -> Organization receives an OrgAdmin account invitation
  -> OrgAdmin or authorized platform user creates Store
  -> authorized user creates Kiosk under Store
  -> technician/configurator prepares kiosk devices and topology
  -> organization configures commercial offer and compatible production package
  -> readiness and operating state permit sales
```

The system currently supports individual actions for this flow. A single
guided franchise onboarding path also exists for the use case where a platform
user provisions the initial store and kiosk set together. UI must not assume
that every organization is ready to sell immediately after creation.

## Sales Admission Versus Active Fulfillment

```text
Store closing or sales pause
  -> stop accepting new orders
  != cancel paid, queued, accepted, or running fulfillment

Kiosk not Operational
  -> stop accepting new work at that kiosk
  != assert that a running robot job failed
```

This distinction is essential for operational UI. A user needs separate views
for "cannot take a new order" and "requires intervention for an existing
order."

## Open Product Decisions

- What customer segment is the launch default: independent owner/operator,
  multi-unit franchise, or centrally managed network?
- What constitutes business onboarding completion: first Store, first ready
  Kiosk, first sellable menu, or first successful sale?
- Which commercial settings are shared across all Stores by default, and which
  need an explicit Store/Kiosk override workflow?
- Is the first Store normally created by the platform during onboarding or by
  the assigned `OrgAdmin` after invitation acceptance?

These questions must be answered in [Open Questions](../decisions/OPEN_QUESTIONS.md) or a
confirmed product decision before a task assumes a single onboarding UX.

## Related Sources

- [Role And Scope Model](ROLE_AND_SCOPE_MODEL.md)
- [Customer Order And Fulfillment Flow](../journeys/CUSTOMER_ORDER_AND_FULFILLMENT.md)
- [Backend API Surface Rules](../../../IceBot-Backend/docs/api/API_SURFACE_RULES.md)
- [Backend Multi-Tenancy Rules](../../../IceBot-Backend/docs/architecture/MULTI_TENANCY_RULES.md)
