# Flow Manager Discovery

## Status

**Read-only Dashboard direction partially confirmed. Not an implementation contract.**

This document records the current backend evidence and the product decisions
needed before IceBot introduces a feature called `Flow Manager`. It does not
authorize a generic workflow engine, new API, automation trigger, or client
screen.

## Operating Problem To Decide

IceBot already has several named, stateful workflows. A person may need one
place to understand which setup or operational workflow needs attention for an
organization, store, or kiosk.

The unresolved product question is whether `Flow Manager` means:

1. a read-oriented **Operations Flow Hub** that shows and deep-links to
   existing workflow read models; or
2. a new generic engine that creates, runs, retries, cancels, or composes
   arbitrary workflows.

These are different products. The first may reuse existing bounded workflows.
The second would introduce new ownership, lifecycle, authorization, audit, and
physical-side-effect risks.

## Confirmed Direction

On 2026-07-27, the product stakeholder confirmed two choices for V1:

- the user-facing name is `Trung tam van hanh`; and
- the read-oriented surface is composed inside the existing Admin Web
  Dashboard rather than introduced as a separate route.

The remaining actor/scope and contract questions stay open. These two choices
do not authorize a generic orchestration engine, new mutation, or automation.

## Confirmed Backend Evidence

The backend currently has no `FlowManager`, `WorkflowManager`, generic workflow
engine, or generic workflow orchestration API.

It instead owns distinct workflows with their own state and rules:

| Existing backend-owned workflow | Scope and boundary | Relevant evidence |
| --- | --- | --- |
| Franchise onboarding | One organization; creates or reuses Store and Kiosk and can install one selected package. It is idempotent, resumable, cancellable before it is ready, and ends `ReadyForActivation`. | `Application/Tenants/Onboarding/FranchiseOnboardingService.cs`; `WebAPI/Controllers/Tenants/ManagementFranchiseOnboardingsController.cs`; `docs/flows/BACK_OFFICE_SETUP_FLOW.md` |
| Production Package installation | One organization installation; materializes package resources and returns an installation workspace with typed action codes. Resource-specific commands remain authoritative. | `docs/flows/PRODUCTION_PACKAGE_INSTALLATION_FLOW.md` |
| Production Package upgrade | One installed package; preview, materialization, review, cutover, abandon, rollback, and reconciliation remain package-specific. | `docs/flows/PRODUCTION_PACKAGE_UPGRADE_FLOW.md` |
| Configuration deployment | One kiosk and execution endpoint; deployment/rollback creates durable deployment state and Edge work. It is not a generic task. | `docs/flows/ROBOT_LUA_DEPLOYMENT_AND_ACTIVATION_FLOW.md` |
| Production incident resolution | One order/item/unit range; may lead to remake, compensation, or manual operational follow-up. | `docs/flows/PRODUCTION_INCIDENT_RESOLUTION_FLOW.md` |
| Operations support | Kiosk-scoped telemetry, alerts, maintenance, inventory, and operational evidence. | `docs/flows/OPERATIONS_SUPPORT_FLOW.md` |

Backend reconciliation and alert automation are implementation-owned workers.
They are not evidence of a user-configurable Flow Manager and must not be
exposed as arbitrary client automation.

## Recommended V1 Boundary

Until Product confirms a different outcome, the safe interpretation is an
**Operations Flow Hub**:

- It is a scoped, read-oriented workspace that surfaces existing workflow
  status, blockers, and permitted next links.
- It does not create a universal workflow record or execute arbitrary steps.
- It does not replace the owning workflow's lifecycle, authorization,
  idempotency, retry, audit, or physical-side-effect safeguards.
- It may show only workflow cards for which an existing read model and a
  permitted target screen already exist.

This is a recommendation, not a confirmed product decision.

## Proposed V1 Specification

[Operations Flow Hub V1](../journeys/OPERATIONS_FLOW_HUB_V1.md) turns the
recommended read-only boundary into a reviewable product specification. It
uses current Admin Web evidence as a starting point, labels unsupported
aggregation and authorization behavior as unresolved, and remains proposed
until Product/BA approves the remaining decisions.

## Explicit Non-Goals For V1

- No generic `Start`, `Retry`, `Cancel`, `Rollback`, or `Approve` command.
- No direct IoT, Edge command, robot, payment-provider, or deployment command
  transport.
- No inferred readiness, progress, failure, or completion from unrelated
  records.
- No cross-organization aggregation for an account without explicit effective
  scope authorization.
- No new persistence model merely to mirror existing workflow state.

## Product Decisions Required

Before a `FLOW-*`, `CAP-*`, backend contract, or implementation task is added,
Product/BA must answer:

1. What exact outcome does the user need: discovery/navigation, approval queue,
   or a new orchestration capability?
2. Which existing workflows belong in V1: onboarding, package installation,
   package upgrade, deployment, incidents, maintenance, alerts, or another
   named flow?
3. Which actor and exact scope can see each flow: `SystemAdmin`, `OrgAdmin`,
   `Manager`, `Staff`, or `Technician`; global, organization, store, kiosk, or
   order?
4. Is V1 read-only, or which existing resource-specific actions are allowed as
   deep links? A deep link is not permission to invoke a mutation.
5. Does any workflow require human approval before its existing action is
   exposed? Who is the decision authority?
6. What should the user see for unavailable scope, no flows, stale/read failure,
   blocked technical prerequisites, and an in-progress action owned elsewhere?
7. What evidence makes a flow complete, especially where Edge activation,
   payment settlement, or physical output is involved?

## Delivery Consequence After Decision

Only after the answers above are confirmed:

```text
Confirmed product outcome
-> update owning journey and role/scope model
-> add one FLOW-* entry and only applicable CAP-* assignments
-> identify existing read model(s) and allowed deep links
-> create target-specific implementation tasks
-> add a contract-change entry if responsibility or required client state changes
```

If Product chooses a new orchestration engine instead of a read-only hub, it
requires a separate backend design task before any frontend work. That design
must establish resource ownership, lifecycle, tenancy, authorization,
idempotency, retry, audit history, cancellation, compensation, and recovery.

## Evidence Limits

This document is based on backend source and flow documentation at backend
`main` `a02f314`. It does not claim that every listed backend workflow has a
current Admin Web screen, that a given role has a particular permission, or
that any physical/deployment action is appropriate for a new UI.
