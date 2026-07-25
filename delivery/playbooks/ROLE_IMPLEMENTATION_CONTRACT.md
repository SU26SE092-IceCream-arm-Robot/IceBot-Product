# Role Implementation Contract

This contract gives any contributor or AI one consistent entry point. It does
not require complete system knowledge before starting; it requires correct
evidence before implementation.

## Three Reading Levels

| Level | Read | Outcome |
| --- | --- | --- |
| Product | Product journey and `FLOW-*` entry | Actor, scope, goal, visible state, allowed action. |
| Integration | Operation/message catalog and owning backend flow | Current route/message family, authority, idempotency, failure rules. |
| Repository | Target manifest and local source | Files to inspect, evidence, and missing work. |

Do not infer product rules from a route, mock, or existing screen. When the
product journey and backend contract do not settle a rule, report a blocking
question instead of inventing behavior.

## Requests That Propose A Solution

When a user asks for a specific API, field, entity, event, or screen, first use
[Request Triage Protocol](REQUEST_TRIAGE.md). The user may be naming a
technical shortcut rather than the operating need. Explain the current flow
and its boundary before proposing implementation.

## Universal Capability Evidence

| Evidence | UI client | Edge/IoT | Backend | QA |
| --- | --- | --- | --- | --- |
| Entry | route/screen | command/event entrypoint | controller/handler | scenario setup |
| Scope | user/tenant/order access | endpoint/kiosk identity | authorization/tenancy | actor fixture |
| Input | typed request | versioned message | DTO/command | request fixture |
| State | loading/empty/error/lifecycle | durable local state/retry | aggregate/projection | expected transition |
| Failure | user-safe recovery | retry/reject/unknown | error/compensation | failure assertion |
| Verification | test/manual evidence | replay/restart evidence | test evidence | executable acceptance |

## AI Implementation Request

Give an AI the target repository plus `CAP-*` and/or `FLOW-*` IDs. It must
follow [AI Implementation Request Template](AI_IMPLEMENTATION_REQUEST.md)
and return an evidence-backed task list before expanding scope.

## Related Sources

- [Flow Catalog](../catalogs/FLOW_CATALOG.yaml)
- [REST Operation Catalog](../catalogs/OPERATION_CATALOG.json)
- [Edge Message Catalog](../catalogs/MESSAGE_CATALOG.yaml)
- [Frontend Audit Protocol](IMPLEMENTATION_AUDIT.md)
- [Request Triage Protocol](REQUEST_TRIAGE.md)
