# Frontend Audit Protocol

This protocol lets an AI produce a reliable implementation backlog from a
frontend repository and the current IceBot contracts. It is not permission to
invent screens, business rules, or backend endpoints.

## Inputs

1. The target contract in `../targets/<target>/CONTRACT.yaml`.
2. The relevant entries in [Flow Catalog](../catalogs/FLOW_CATALOG.yaml).
3. The linked product journey and backend flow document.
4. The current frontend source: routes/screens, services, models/state,
   authorization/scope logic, and tests.

## Audit Procedure

```text
Select frontend
-> load applicable capabilities
-> read only linked product/backend docs
-> inspect current FE route, service, state, and tests
-> compare required behavior with evidence
-> classify each capability
-> create prioritized, evidence-backed tasks
```

For each capability, the audit must prove or mark absent:

- actor and active scope handling;
- entry screen/route;
- correct read model or command integration;
- lifecycle actions and mutation-pending behavior;
- loading, empty, permission-denied, and failure states;
- domain-specific failure states named by the linked flow;
- realtime or polling behavior when the contract requires it;
- test or manually verifiable evidence.

Do not mark a capability `complete` because a page, type, mock, service stub,
or generic error toast exists. Do not mark it `missing` without searching the
repository for route, component, service, and contract identifiers.

## Required Audit Output

```text
Capability ID:
Status: complete | partial | missing | contradictory | blocked-by-backend |
Evidence:
- path and symbol proving the current behavior

Required behavior not covered:
- precise missing/incorrect state, action, scope, or integration

Backend source:
- product journey and backend flow links

Task:
- actor, scope, screen, allowed action, API/read model, failure states,
  acceptance evidence

Priority:
- P0 blocks customer sale/safe operation/security
- P1 blocks a normal operating workflow
- P2 enables advanced or administrative workflow
```

## Cross-Flow Rule

When one finding exposes a repeated pattern, scan all capabilities that use
the same mechanism before creating individual tasks. Examples:

- missing active-scope handling across management screens;
- treating a page as complete while using mock instead of API data;
- ignoring `partial` or `support-required` order state in more than one view;
- treating all operational alerts as the same resolution action.

The audit should name the common pattern and list each affected capability.

## Related Sources

- [Product Task Readiness Template](TASK_READINESS.md)
- [Backend API Surface Rules](../../../IceBot-Backend/docs/api/API_SURFACE_RULES.md)
- [Backend Authorization Rules](../../../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md)
