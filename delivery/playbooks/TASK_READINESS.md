# Product Task Readiness Template

Use this template before creating or accepting a frontend, backend, tablet,
Edge, or QA task that affects a user or operations workflow.

## Required Context

```text
Title:

Capability ID:
For example: CAP-KIOSK-ORDER-TRACKING. Use the implementation-contract
catalog when the task changes an existing frontend responsibility.

Actor:
Exact role(s): SystemAdmin | OrgAdmin | Manager | Staff | Technician | Customer

Scope:
Global | Organization | Store | Kiosk | One customer order

Goal:
What decision or action is this actor trying to complete?

Preconditions:
What must already exist or be true?

Happy path:
What does the actor see and do, in order?

Allowed actions:
Which mutations are possible? Which role/scope permits each one?

Read model / contract:
Which existing query, API, or event supplies the UI state?

Empty and failure states:
No scope, no data, unavailable resource, pending state, partial outcome,
permission denied, retry/manual intervention.

Out of scope:
What this task must not infer, add, or change.

Acceptance evidence:
UI behavior, API/contract behavior, and test or manual verification needed.

Implementation evidence:
Route/screen, service/query/command, state model, and test paths that prove
the capability is complete or explain why it remains partial.
```

## Example

```text
Title: Organization Operations Dashboard - kiosk readiness panel

Actor: OrgAdmin, Manager
Scope: Assigned organization; user selects an allowed Store/Kiosk
Goal: Decide whether a kiosk can accept new sales and what follow-up is needed.
Preconditions: User has organization scope and kiosk exists.
Happy path: Select scope -> view sales state, connectivity, readiness, and
open incident count -> navigate to the permitted operational action.
Failure states: No assigned scope; Store has no kiosk; kiosk paused while a
paid job is active; user may view but cannot change kiosk state.
Out of scope: Platform-wide health, raw Edge diagnostics, automatic refunds.
```

## Rejection Rule

Return a task for clarification when it says only "admin", "dashboard",
"manage", "status", or "make an API" without defining actor, scope, business
goal, and failure state.

## Related Sources

- [Role And Scope Model](../../product/actors-and-scope/ROLE_AND_SCOPE_MODEL.md)
- [Workspace And Dashboard Model](../../product/operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md)
- [Open Questions](../../product/decisions/OPEN_QUESTIONS.md)
- [Frontend Implementation Contracts](../README.md)
