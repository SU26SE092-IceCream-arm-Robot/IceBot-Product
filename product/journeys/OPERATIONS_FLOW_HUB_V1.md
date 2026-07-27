# Operations Flow Hub V1

## Status And Decision Authority

**V1 direction partially approved on 2026-07-27. Not yet approved for implementation.**

This proposal narrows the earlier `Flow Manager` question to a safe V1:
a read-oriented Admin Web workspace composed inside the existing Dashboard and
named **Trung tam van hanh**.

The product stakeholder has approved the display name and Dashboard
composition. Product/BA must still approve the actor/scope rules and remaining
contract choices before an implementation task is created. Backend and frontend
repositories remain the source of truth for exact contracts and authorization
behavior.

## Confirmed Product Choices

| Decision | Confirmed choice | Date |
| --- | --- | --- |
| User-facing name | `Trung tam van hanh` | 2026-07-27 |
| Placement | Compose the V1 surface inside the existing Admin Web Dashboard; do not create a separate route. | 2026-07-27 |

These choices confirm placement and naming only. They do not approve new
backend contracts, mutations, automation, or broader access.

## Problem And Goal

An Admin Web user currently has two useful but separate capabilities:

- Setup Readiness answers whether the selected organization and store have the
  minimum configuration needed to operate.
- Dashboard shows selected operational signals, such as order, inventory, and
  kiosk attention counts.

The `Trung tam van hanh` Dashboard composition gives a permitted user one place
to discover what needs attention and navigate to the owning module. It does
not create a new workflow engine or take ownership of existing workflow
lifecycle.

## V1 Outcome

For one selected organization and store, an authorized user can:

1. see setup readiness separately from live operational attention;
2. see only evidence backed by an existing Admin Web read model;
3. open the module that owns the next action; and
4. understand when a source is unavailable or cannot be checked.

The hub is a navigation and evidence surface. The owning module remains
responsible for every mutation, lifecycle decision, retry, audit record, and
scope check.

## Actors And Scope

| Actor | Proposed V1 access | Scope rule |
| --- | --- | --- |
| `SystemAdmin` | Can view cards supported by the underlying API. | Global, subject to the underlying API. |
| `OrgAdmin` | Can view organization-scoped cards supported by the underlying API. | Assigned organization scope only. |
| `Manager` | Can view operational cards supported by the underlying API. | Assigned organization/store/kiosk scope only. |
| `Staff` | May view only cards whose underlying read API permits the role. | Assigned scope only. |
| `Technician` | May view technical kiosk/inventory cards whose underlying API permits the role. | Assigned scope only. |

This table is not a replacement for backend policy checks. The hub must not
infer authorization from a role name alone. Each source request remains
authorized by the backend, and a `403` is shown as unavailable rather than as
an empty or healthy state.

## Included Cards

| Group | Card | Evidence already available in Admin Web | Owning destination | V1 behavior |
| --- | --- | --- | --- | --- |
| Setup | Basic setup status | Organization/store activity, kiosk, products, variants, menus, menu items, and active payment method checks. | Existing Setup Readiness view. | Reuse the existing checklist and its next links. |
| Orders | Pending payment | Dashboard order overview count. | Transactions. | Read-only count and link only. |
| Orders | Refund required | Dashboard order overview count. | Transactions. | Read-only count and link only. |
| Inventory | Low or empty inventory | Dashboard inventory summary counts. | Inventory. | Read-only count and link only. |
| Kiosk | Connectivity attention | Dashboard connectivity-backed unreachable count. | Kiosks. | Clearly distinguish connectivity from lifecycle. |
| Kiosk | Maintenance lifecycle | Dashboard maintenance kiosk count. | Kiosks. | Read-only count and link only. |
| Kiosk | Recent device evidence | Dashboard latest device event count. | Kiosks. | Do not label this as a generic alert feed. |

## Explicitly Excluded From V1

- A generic workflow record, workflow builder, or user-configurable automation.
- Generic `Start`, `Retry`, `Cancel`, `Approve`, `Rollback`, or `Resolve`
  actions.
- Direct IoT, Edge, MQTT, robot, payment-provider, deployment, package, or
  redispatch commands.
- An inferred global health score, sales readiness percentage, revenue,
  telemetry state, payment success, or recent-activity feed.
- Maintenance-ticket and alert aggregate cards until their exact read contract,
  scope behavior, and meaning are separately confirmed.
- URL filter propagation until each destination module confirms a stable,
  supported filter contract.

## User Journey

1. The user opens the existing Dashboard.
2. The `Trung tam van hanh` composition loads supported operational evidence
   for the account's backend-enforced effective scope.
3. When the user opens setup readiness, the existing readiness selector is
   used to choose an organization and store available to the account.
4. A source with valid zero results appears as a normal empty/healthy state.
5. A source that returns `403`, `404`, network failure, or partial failure is
   labelled unavailable or cannot be checked; it is never converted into a
   zero count.
6. The user selects a card and is navigated to its owning module.
7. Any mutation happens only in that owning module, under its existing
   confirmation, duplicate-submit, refresh recovery, and backend
   authorization rules.

## UI Boundary

The proposed composition is part of the existing Dashboard. It is not a new
route and does not replace the Setup Readiness page.

```text
Dashboard
  Trung tam van hanh
  Setup status: existing readiness summary and next setup actions
  Needs attention: pending payment, refund required, low/empty inventory,
                   unreachable kiosk, maintenance kiosk, device-event evidence
  Availability notes: only for failed or unauthorized sources
  Links: open the owner module; no mutation controls
```

For V1, the composition should remain compact. It should not duplicate long
checklists or dashboard distributions if a link to the existing view
communicates the same information more clearly. It must not present a new
Dashboard-wide organization/store selector until the Dashboard GraphQL contract
supports and defines that filter.

## Data And Contract Boundary

| Need | Current evidence | Contract status | Implementation consequence |
| --- | --- | --- | --- |
| Setup readiness | Existing Admin Web composes organization, store, kiosk, product, menu, and payment-method reads. | Available in frontend; each source remains independently authorized. | Can be reused after Product approval. |
| Order and inventory attention | Existing management Dashboard GraphQL query returns the counts used by the Dashboard. | Available in frontend; current dashboard query is not documented here as a generic scoped contract. | Do not claim organization/store filtering until backend confirms it. |
| Kiosk lifecycle/connectivity | Existing Dashboard GraphQL separates lifecycle and connectivity. | Available in frontend. | Keep the two concepts separate in all copy and UI. |
| Alert aggregate | Current frontend has an Alerts module, but this proposal does not establish an aggregate contract. | Unknown for hub use. | Deferred. |
| Maintenance aggregate | Current frontend has a Maintenance module, but this proposal does not establish an aggregate contract. | Unknown for hub use. | Deferred. |
| Effective permission codes | Current account access includes role and scope information, not a confirmed complete policy-code list. | Insufficient for universal client-side authorization. | Use conservative visibility; backend remains authoritative. |
| Cross-module URL filters | Existing links are module links, not confirmed filter contracts. | Unknown. | V1 uses plain deep links. |

## Failure And Empty-State Rules

| Situation | Required presentation |
| --- | --- |
| No selected organization or store | Ask the user to select scope; do not calculate readiness. |
| Valid response with zero count | Show the card's natural empty or healthy state. |
| `403` from one source | Mark only that source as unavailable due to access; do not grant or infer permission. |
| `404` from one source | Mark only that source as unavailable; do not treat it as zero. |
| Network or backend failure | Preserve other successful cards and show a retry for the failed read. |
| Source data is stale or unknown | Say it cannot be checked; do not infer a lifecycle, connection, or payment state. |

## Acceptance Criteria For A Future Implementation Task

- The Dashboard composition is read-only and does not introduce a new route,
  backend endpoint,
  persistence model, mutation, or automation trigger.
- Every displayed count has an identified existing source; unsupported cards
  are omitted rather than fabricated.
- Organization/store selection never displays raw UUIDs and never widens the
  account's backend-enforced scope.
- Lifecycle and connectivity remain distinct.
- A failed source cannot appear as a zero count or healthy result.
- Each action is a deep link to the owning module; no high-risk command is
  exposed in the hub.
- Light/dark, desktop/tablet/mobile, loading, partial failure, empty, and
  keyboard focus states are verified.
- The backend remains the final authority for policy and resource scope.

## Open Decisions Before Implementation

1. Confirm the actor/scope matrix per included card with the owning backend
   policies and Product expectations.
2. Decide whether the current Dashboard query may receive organization/store
   filters in a future backend contract; V1 must not assume this.
3. Decide whether Alerts and Maintenance belong in a later hub version after
   their aggregate read contracts are audited.
4. Decide whether a backend endpoint should eventually return explicit
   permission codes for client-side feature visibility.

## Implementation Consequence After Approval

After Product/BA approves this proposal, create one read-only frontend planning
task that first audits the exact dashboard and destination-module contracts.
Do not create a generic workflow API, a `FlowManager` entity, or a backend
mutation as part of that task.

## Related Documents

- [Flow Manager Discovery](../decisions/FLOW_MANAGER_DISCOVERY.md)
- [Technical Operations Journey](TECHNICAL_OPERATIONS.md)
- [Role And Scope Model](../actors-and-scope/ROLE_AND_SCOPE_MODEL.md)
- [Workspace And Dashboard Model](../operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md)
- [Open Questions](../decisions/OPEN_QUESTIONS.md)
