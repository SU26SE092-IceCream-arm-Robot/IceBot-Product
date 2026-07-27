# Open Product Questions

This file records unresolved product choices that must not be silently decided
by a backend handler, frontend screen, or task description.

## Organization Onboarding

- Which launch customer segment is primary: single owner/operator, multi-unit
  franchise, or centrally managed franchise network?
- Who normally creates the first Store: platform onboarding staff or invited
  `OrgAdmin`?
- What event marks onboarding complete: first Store, first ready Kiosk, first
  sellable menu, or first successful sale?
- Does the product need an explicit onboarding progress model visible to the
  organization?

## Commercial Ownership

- Who owns the physical robot and kiosk under each commercial model?
- Are prices and menus organization defaults with optional Store/Kiosk
  overrides, or is each location configured independently?
- Who absorbs the cost of a fulfillment issue: platform, organization, store,
  or a policy determined per incident?
- What business reports/KPIs are necessary for each workspace in the first
  release?

## Customer Fulfillment

- What should the customer see when part of a mixed order is ready and another
  item requires inspection?
- Is a voucher a real V1 compensation option or only a reserved backend
  resolution type?
- What pickup/abandonment behavior is required when completed output is not
  collected?

## Product Governance

- Who owns and reviews the shared Product & Operations documents?
- Which product decisions require a short decision record before implementation?

## Flow Manager

Confirmed on 2026-07-27:

- V1 is a read-oriented composition named `Trung tam van hanh` inside the
  existing Admin Web Dashboard.
- V1 does not introduce a separate route or generic orchestration engine.

Still unresolved:

- Which actor and exact scope may see each included card?
- Can a future Dashboard contract support organization/store filters without
  changing the meaning of current aggregate counts?
- Should Alerts and Maintenance aggregates be included after their contracts
  are audited?
- Should current-account access eventually return explicit policy codes for
  client-side feature visibility?

See [Flow Manager Discovery](FLOW_MANAGER_DISCOVERY.md) before proposing an
endpoint, entity, screen, or automation trigger.

## Resolution Rule

When an answer is chosen, create or update the owning product document and add
the decision date, owner, and implementation consequence. Keep enough rationale
here for contributors to apply the decision without external notes.

## Related Sources

- [Product And Operations Index](../README.md)
- [Organization, Store, And Kiosk Operating Model](../actors-and-scope/ORGANIZATION_STORE_KIOSK_OPERATING_MODEL.md)
- [Customer Order And Fulfillment Flow](../journeys/CUSTOMER_ORDER_AND_FULFILLMENT.md)
