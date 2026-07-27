# IceBot-WebApp Full Task: Manager Operations

## Assignment

| Field | Value |
| --- | --- |
| Target | `IceBot-WebApp` |
| Primary actor | `Manager` |
| Related actor | `OrgAdmin`, only where current backend policy permits |
| Scope | Backend-enforced effective assigned organization/Store/Kiosk scope |
| Delivery shape | One complete task divided into implementation phases |
| Product versions | None; phases are not V1/V2 |
| Backend changes | None by default |
| Current status | All affected target capabilities remain evidence-based and currently `unverified` |

This task implements the complete Manager operating surface supported by the
current backend. It does not stop after Dashboard.

## Required Reading

1. [Role Implementation Contract](../../playbooks/ROLE_IMPLEMENTATION_CONTRACT.md)
2. [Manager Workspace Decision](../../../product/decisions/MANAGER_OPERATIONS_WORKSPACE_DECISION.md)
3. [Manager Store Operations](../../../product/journeys/MANAGER_STORE_OPERATIONS.md)
4. [Target Contract](../../targets/icebot-webapp/CONTRACT.yaml)
5. [Flow Catalog](../../catalogs/FLOW_CATALOG.yaml)
6. [Backend Authorization Rules](../../../../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md)
7. The linked backend flow for each phase.
8. Current IceBot-WebApp source and its local `AGENTS.md`.

Read only the backend flow currently being implemented. Do not recursively load
every linked document.

## Overall Completion Rule

The task is complete only when:

- exact backend roles and effective scopes drive navigation and actions;
- every included current backend capability has repository evidence;
- every mutation is integrated through its owning current backend contract;
- lifecycle, authorization, loading, empty, failure, stale, concurrency, retry,
  and duplicate-submit states are handled where applicable;
- all phase test gates, full lint, full tests, production build, and final diff
  review pass;
- remaining gaps are genuine backend-contract blockers, not unfinished UI.

Page existence, mocked data, a happy-path request, or build success alone is not
completion.

## Frozen Scope

Included capability IDs:

```text
CAP-WEB-IDENTITY-AND-SCOPE
CAP-WEB-TENANT-ONBOARDING
CAP-WEB-SETUP-READINESS
CAP-WEB-WORKSPACE-DASHBOARD
CAP-WEB-MANAGEMENT-REPORTING
CAP-WEB-CATALOG-AND-MENU-AUTHORING
CAP-WEB-ORDER-PAYMENT-OPERATIONS
CAP-WEB-PRODUCTION-INCIDENT-RESOLUTION
CAP-WEB-KIOSK-READINESS
CAP-WEB-INVENTORY-OPERATIONS
CAP-WEB-OPERATION-EVIDENCE
CAP-WEB-ALERT-OPERATIONS
CAP-WEB-MAINTENANCE-OPERATIONS
CAP-WEB-GUIDED-ROBOT-AUTHORING
CAP-WEB-RELEASE-AND-DEPLOYMENT
CAP-WEB-PACKAGE-INSTALLATION
CAP-WEB-PACKAGE-UPGRADE
CAP-WEB-FAILURE-ROUTING
```

Excluded:

- customer Kiosk UI;
- direct IoT/Edge/provider integration surfaces;
- SystemAdmin-only global administration;
- Technician-only raw diagnostics, credentials, and unsafe technical commands;
- future automatic refunds;
- invented API/filter/permission contracts;
- unrelated visual redesign.

## Current Role Policy Matrix

This table summarizes the current backend authorization boundary. The AI must
still verify the exact operation and resource scope in the owning backend flow.

| Area | `Manager` | `OrgAdmin` |
| --- | --- | --- |
| Current account, role/permission catalog, scope options | Read within contract | Read within contract |
| Internal accounts | Scoped read only | Scoped read only |
| Organization | No organization profile policy | Read/update assigned organization profile |
| Store | Read/update assigned Store | Read/create/activate/disable/update assigned Store |
| Kiosk | Read/create/update/change state in assigned scope | Same within assigned organization |
| Device and execution endpoint | Read/manage in assigned scope | Read/manage in assigned scope |
| Artifact | No artifact read/upload policy | Read/upload/review/publish/retire organization artifacts |
| RobotProgram | Read/manage in assigned scope | Read/manage in assigned scope |
| Release | Read; deploy and rollback | Read/author/publish/retire; deploy and rollback |
| Production Package | Read/install/upgrade | Read/install/upgrade/fork |
| Product template/category/ingredient | Lookup only | No current tenant-role lookup policy unless separately allowed by the exact endpoint |
| Organization Product/Variant/Recipe/Menu | Manage in assigned scope | No current `products.manage` or `menus.manage` policy |
| Payments and refund intervention | Manage payment and manual refund workflows | No current tenant payment-management policy |
| Inventory | Read/manage/configure | Read only |
| Operations evidence | Curated scoped reads | Curated scoped reads |
| Raw diagnostics | Not allowed | Not allowed |
| Notification delivery | Requeue permanently failed delivery in scope | Same |
| Alerts | Read/acknowledge/resolve | Same |
| Maintenance | Read/create/manage | Same |
| Reports | Scoped read | Scoped read |

Important consequences:

- `Manager` and `OrgAdmin` are not interchangeable.
- A shared page may be visible to both while individual commands differ.
- `OrgAdmin` artifact/release governance does not grant Manager artifact upload.
- Manager Product/Menu/Payment/Inventory authority does not automatically grant
  those mutations to `OrgAdmin`.
- Technician-only raw diagnostics and `robot-config.manage` remain outside this
  task's normal Manager surface.

## Phase 0: Baseline And Backend Capability Matrix

- [ ] Read target Git status and preserve uncommitted work.
- [ ] Run baseline:

```powershell
npm run lint
npm test
npm run build
```

- [ ] Generate candidate target evidence:

```powershell
cd ..\IceBot-Product
python ..\IceBot-Tools\docs-ops\commands\audit_target_capability_evidence.py --target IceBot-WebApp
```

- [ ] Build a matrix for every included capability:

```text
CAP id
actor and exact policy
scope
entry route
backend query/operation
current FE service
current UI
lifecycle states
failure states
tests
status
```

- [ ] Compare target contract with backend authorization policy. Backend is
      authoritative for current technical permission; report product conflicts.
- [ ] Find legacy role assumptions:

```powershell
rg -n "DashboardRole|LOCATION_OWNER|LocationOwner|ADMIN|role ===|hasPermission|canAccessRoute" src
```

Gate: no implementation begins until every current Manager backend area is
assigned to a phase and missing contracts are identified.

## Phase 1: Identity, Exact Roles, Effective Scope, And Navigation

Owning capabilities:

- `CAP-WEB-IDENTITY-AND-SCOPE`
- `CAP-WEB-FAILURE-ROUTING`

Work:

- [ ] Replace `ADMIN`, `LOCATION_OWNER`, and `LocationOwner` aliases with exact
      backend role codes.
- [ ] Preserve all role/scope assignments from current-account access.
- [ ] Separate presentation labels from authorization input.
- [ ] Refactor RBAC, route visibility, sidebar, top bar, hooks, mocks, and tests.
- [ ] Make active/effective scope visible without requesting raw IDs.
- [ ] Fail closed when current-account access is absent or stale.
- [ ] Handle unauthenticated, expired session, no scope, forbidden route, and
      permission change after page load.
- [ ] Centralize user-safe mapping of 401, 403, 404, 409, validation, dependency,
      and unknown failures without hiding owning workflow detail.

Gate:

- exact-role tests;
- multi-role/multi-scope tests;
- route-visibility tests;
- session-expiry tests;
- stale aliases absent from executable source and tests.

## Phase 2: Workspace Entry, Dashboard, Reports, And Setup Readiness

Owning capabilities:

- `CAP-WEB-WORKSPACE-DASHBOARD`
- `CAP-WEB-SETUP-READINESS`
- `CAP-WEB-MANAGEMENT-REPORTING`

Work:

- [ ] Keep `/dashboard` as the Manager attention and routing entry.
- [ ] Reuse current scoped GraphQL dashboard/order/Kiosk/inventory reads.
- [ ] Show pending-payment, refund-required, low/empty inventory, unreachable
      Kiosks, and Kiosks in Maintenance with exact meanings.
- [ ] Do not treat device-event volume as an alert.
- [ ] Keep Setup Readiness as a separate workflow with its existing permitted
      scope selection.
- [ ] Implement reports as the current client-composed projection over scoped
      REST reads for Stores, Kiosks, orders, refunds, Products, Menus,
      inventory, and maintenance.
- [ ] Do not invent a GraphQL Reports root or treat client aggregation as one
      atomic backend snapshot.
- [ ] Preserve per-source success, permission-unavailable, failure, truncation,
      and stale-evidence state in the composed report.
- [ ] Route cards to owning modules only when the destination is permitted.
- [ ] Handle independent GraphQL root failures and partial data.
- [ ] Distinguish zero, no data, unavailable, unauthorized, stale, and healthy.

Gate:

- GraphQL mapping tests;
- partial-data page test;
- setup-readiness source failure tests;
- report scope/empty/failure tests;
- report partial-source, permission-unavailable, and truncation tests;
- responsive and keyboard check for workspace entry.

## Phase 3: Organization, Store, Kiosk Setup, And Sales Admission

Owning capabilities:

- `CAP-WEB-TENANT-ONBOARDING`
- `CAP-WEB-KIOSK-READINESS`

Work:

- [ ] Implement only exact OrgAdmin/Manager operations permitted by backend.
- [ ] Support hierarchy lookup instead of manual organization/Store/Kiosk IDs.
- [ ] Handle Store details, opening hours, sales pause/resume, and Kiosk
      operational state.
- [ ] Keep Store closed, Store sales-paused, Kiosk unavailable, and Device
      connectivity as distinct conditions.
- [ ] Preserve paid/active fulfillment visibility when new sales stop.
- [ ] Support Device catalog lookup and Manager-permitted Device/endpoint
      management in an advanced operations section.
- [ ] Treat `EmergencyStopRequested` as a Cloud request, not proof of physical
      stop.
- [ ] Handle hierarchy mismatch, stale update, conflict, and authorization
      failure.

Gate:

- tenant hierarchy tests;
- opening-hours and sales-state presentation tests;
- active-fulfillment-preservation test;
- Device/endpoint policy visibility tests;
- mutation duplicate-submit and conflict tests.

## Phase 4: Product, Recipe, Options, Menu, Pricing, And Availability

Owning capability:

- `CAP-WEB-CATALOG-AND-MENU-AUTHORING`

Work:

- [ ] Product/template and category lookup under exact current policies.
- [ ] Organization Product and Variant authoring.
- [ ] OptionGroup/ProductOption authoring and lifecycle.
- [ ] Ingredient lookup, Recipe/RecipeItem authoring, lifecycle, and validation.
- [ ] Product/option ingredient execution bindings exposed through guided
      forms, not raw JSON.
- [ ] Menu authoring, MenuItem membership, allowed options, pricing, promotion,
      currency, and availability lifecycle.
- [ ] Clone behavior and source/organization ownership clearly presented.
- [ ] Sellability/readiness blockers shown before activation.
- [ ] No direct lifecycle fields in generic edit forms where dedicated commands
      exist.

Gate:

- create/edit/lifecycle tests;
- Product/Variant/Recipe/Option mismatch tests;
- Menu currency and option-membership tests;
- required-group sellability tests;
- stale/conflicting mutation tests.

## Phase 5: Orders, Payments, Fulfillment, Refund Intervention, And Incidents

Owning capabilities:

- `CAP-WEB-ORDER-PAYMENT-OPERATIONS`
- `CAP-WEB-PRODUCTION-INCIDENT-RESOLUTION`

Work:

- [ ] Scoped order list/detail with current lifecycle evidence.
- [ ] Payment session/transaction states and provider-confirmed truth.
- [ ] Manual refund/intervention workflow; do not imply automatic provider
      refund.
- [ ] Duplicate payment occurrence and refund-required intervention.
- [ ] Manual, Packaged, and MachineProduced fulfillment presentation.
- [ ] Per-item and per-production-unit progress.
- [ ] Partial/defective/unknown output inspection.
- [ ] Exact-unit remake eligibility and action.
- [ ] Delivery/discard/compensation/resolution evidence.
- [ ] Preserve successful inventory/output evidence when another unit fails.
- [ ] Prevent generic whole-order retry when only one unit is eligible.

Gate:

- late/duplicate payment tests;
- partial fulfillment tests;
- exact-unit remake tests;
- unknown outcome tests;
- stale incident and duplicate-submit tests;
- scoped order tenancy tests.

## Phase 6: Inventory, Topology, Refill, Adjustment, And Readiness

Owning capability:

- `CAP-WEB-INVENTORY-OPERATIONS`

Work:

- [ ] Kiosk topology view: Device, container, Ingredient, state.
- [ ] Provision dispenser state using accessible Device/Ingredient choices.
- [ ] Refill and adjustment with actor, reason, before/after quantity.
- [ ] Rebind/replace/retire workflows and history.
- [ ] Device capability validation and inactive-reference warnings.
- [ ] Readiness states: Ready, MissingIngredient, ContainerInactive,
      DeviceUnavailable, CalibrationMissing where current backend supports it.
- [ ] Production configuration consistency warnings/blockers.
- [ ] Keep inventory readiness separate from automatic menu sellability unless
      current backend explicitly decides it.

Gate:

- topology scope tests;
- capability mismatch tests;
- refill/adjust audit tests;
- rebind conflict tests;
- stale inventory/concurrency tests;
- readiness mapping tests.

## Phase 7: Alerts, Maintenance, Notifications, And Operational Evidence

Owning capabilities:

- `CAP-WEB-ALERT-OPERATIONS`
- `CAP-WEB-MAINTENANCE-OPERATIONS`
- `CAP-WEB-OPERATION-EVIDENCE`

Work:

- [ ] Alert list/detail, occurrence count, last occurrence, acknowledge, resolve.
- [ ] Do not confuse Kiosk Maintenance state with ticket count.
- [ ] Maintenance create, assign, start, resolve, close, and cancel according to
      exact policy/lifecycle.
- [ ] Validate eligible assignees and current scope.
- [ ] Show operational evidence needed for coordination without raw diagnostic
      payloads.
- [ ] Requeue permanently failed notifications only under current permission,
      with reason and actor confirmation.
- [ ] Keep physical recovery evidence separate from ticket closure.

Gate:

- alert dedup/lifecycle tests;
- maintenance lifecycle and assignee tests;
- notification requeue tests;
- operation-evidence permission tests;
- partial list/detail failure tests.

## Phase 8: Robot Programs, Packages, Releases, Deployments, And Rollback

Owning capabilities:

- `CAP-WEB-GUIDED-ROBOT-AUTHORING`
- `CAP-WEB-PACKAGE-INSTALLATION`
- `CAP-WEB-PACKAGE-UPGRADE`
- `CAP-WEB-RELEASE-AND-DEPLOYMENT`

Work:

- [ ] Separate normal guided adoption from advanced technical authoring.
- [ ] Implement current Manager-permitted RobotProgram read/manage behavior.
- [ ] Package catalog, preview, install, retry, repair, workspace, and fork.
- [ ] Upgrade preview, materialization, review, cutover, abandon, rollback.
- [ ] Preserve organization-owned commercial changes under current upgrade
      ownership rules.
- [ ] Release review and current permitted publication/deployment actions.
- [ ] Full Edge and Low-cost deployment paths.
- [ ] Endpoint eligibility, readiness blockers, pending activation, active,
      failed, timeout, reconciliation, and rollback evidence.
- [ ] Never expose direct Edge transport or raw credential operations.

Gate:

- package install failure/retry tests;
- upgrade stale-preview/cutover/abandon/rollback tests;
- release readiness tests;
- deployment idempotency/conflict/timeout tests;
- tenant scope tests;
- permission separation between Manager, OrgAdmin, and Technician.

## Phase 9: Cross-Cutting UX, Accessibility, And Consistency

- [ ] Consistent list/detail/filter/pagination patterns.
- [ ] Typed forms instead of raw JSON or backend schema-version fields.
- [ ] Stable loading, empty, no-access, validation, conflict, dependency,
      partial-failure, and retry presentation.
- [ ] Destructive/high-impact action confirmation.
- [ ] Disable duplicate submit without hiding server idempotency behavior.
- [ ] Refresh and navigation recovery after successful mutation.
- [ ] SignalR invalidation triggers authoritative reread; realtime messages do
      not become business truth.
- [ ] Mobile/tablet/desktop layout and no overlapping controls.
- [ ] Keyboard, focus, accessible name, contrast, and non-color status cues.
- [ ] No raw tokens, secrets, provider payloads, internal JSON, or diagnostic
      fields in normal Manager UI.

Gate: shared component tests and representative responsive/accessibility
evidence across every major workspace.

## Phase 10: Independent Full-Slice Review

Freeze implementation and review the final diff without adding architecture:

- [ ] Every included capability has entry, scope, contract, lifecycle, failure,
      and test evidence.
- [ ] Every visible mutation maps to an existing backend operation and policy.
- [ ] No route or role widens tenant scope.
- [ ] No failure is converted into zero/success.
- [ ] No lifecycle is inferred from a related but non-authoritative state.
- [ ] No module exposes raw technical/provider/Edge data to normal Manager UI.
- [ ] No tests were weakened or deleted for a pass.
- [ ] No unrelated redesign or backend work was included.

Run:

```powershell
npm run lint
npm test
npm run build
git diff --check
```

The overall task remains incomplete if any phase gate lacks evidence.

## Phase 11: Evidence And Product Handoff

For every affected `CAP-*`, report:

```text
Capability:
Status: complete | partial | contradictory | blocked-by-backend
Actor/scope evidence:
Route/screen evidence:
Backend integration evidence:
Lifecycle/failure evidence:
Test evidence:
Residual gap:
```

After review:

- update `delivery/targets/icebot-webapp/STATUS.yaml` capability by capability;
- advance `last_acknowledged_change_id` only after reviewing the relevant
  contract change;
- do not call the large task complete while any included capability remains
  `partial`, `contradictory`, `unverified`, or lacks required evidence;
- a genuine backend blocker must name the missing contract and owning backend
  flow.

## Stop Conditions

Stop only the affected phase and report a blocker when:

- the backend contract cannot represent the confirmed operating outcome;
- policy and product actor ownership directly conflict;
- a necessary lifecycle/read model is absent;
- current uncommitted target changes make a safe edit impossible.

Do not invent a backend API, role, scope, status, or transition. Continue other
independent phases when safe.

## Ready-To-Use AI Request

```text
Implement the complete Manager Operations program in IceBot-WebApp.

Use:
IceBot-Product/delivery/tasks/icebot-webapp/
MANAGER_OPERATIONS_FULL_IMPLEMENTATION.md

This is one complete task divided into implementation phases, not separate
V1/V2 releases. Continue through all phases, tests, build, independent final
review, and evidence handoff. Preserve existing uncommitted work. Use current
backend routes, policies, lifecycle, and effective tenant scope exactly. Do not
invent APIs or stop after Dashboard. Report the overall task complete only when
every included capability has implementation and failure-path evidence.
```
