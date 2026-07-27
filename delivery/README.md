# Delivery Contracts

This folder is the machine-readable bridge between product journeys, current
backend flows, and delivery targets. It answers which repository is responsible
for a capability, what backend flow owns its behavior, and what evidence is
required before an AI or reviewer calls that capability complete.

It does not replace backend API contracts or copy DTO schemas. Exact routes,
request bodies, authorization policies, retry behavior, and tests remain owned
by the linked `IceBot-Backend/docs/` flow and contract documents.

## Read First

| Need | Read |
| --- | --- |
| Find a flow and assigned target capability | [Flow Catalog](catalogs/FLOW_CATALOG.yaml) |
| Look up generated REST route and policy evidence | [REST Operation Catalog](catalogs/OPERATION_CATALOG.json) |
| Look up an Edge/Cloud message family | [Edge Message Catalog](catalogs/MESSAGE_CATALOG.yaml) |
| Audit WebApp, Kiosk, Mobile, or IoT | [Targets](targets/README.md) |
| Prepare work for any target or its AI | [Role Implementation Contract](playbooks/ROLE_IMPLEMENTATION_CONTRACT.md) |
| Execute an assigned implementation packet | [Target Tasks](tasks/README.md) |
| Ask an AI for an evidence-backed backlog | [AI Implementation Request](playbooks/AI_IMPLEMENTATION_REQUEST.md) |
| Record and route a cross-repository contract change | [Contract Change Protocol](playbooks/CONTRACT_CHANGE_PROTOCOL.md) |
| Challenge a proposed API/field/entity before implementing it | [Request Triage](playbooks/REQUEST_TRIAGE.md) |
| Add a new capability safely | [Flow Implementation Template](playbooks/FLOW_IMPLEMENTATION.md) |

## Source Of Truth Split

```text
IceBot-Product/product/
  -> actor, scope, business goal, expected user journey

IceBot-Backend/docs/flows/
  -> current backend lifecycle, API behavior, invariant, failure handling

IceBot-Product/delivery/
  -> responsibility, stable capability/operation/message IDs, audit evidence standard
```

The catalog must not invent product behavior or backend operations. When a
backend flow has no frontend responsibility, mark it `frontend: none`; do not
create a fake client requirement merely because a backend flow exists.

Each assigned client capability should declare `operation_ids` for its required
REST calls. These IDs resolve against the generated operation catalog; the
contract check fails when a referenced endpoint no longer exists.

## Status Vocabulary

| Status | Meaning |
| --- | --- |
| `complete` | UI, integration, required states, and verification evidence exist. |
| `partial` | Some implementation exists but one or more required parts are absent. |
| `missing` | No meaningful implementation evidence exists. |
| `contradictory` | Frontend behavior conflicts with the current contract. |
| `blocked-by-backend` | A required product behavior has no current backend contract. |
| `not-applicable` | This client is not responsible for the capability. |
| `unverified` | Code exists but audit evidence is insufficient. |

## Maintenance Rule

When a backend flow gains or removes a user-facing operation, update the
matching catalog entry and affected target contract in the same change. When a
target implements a capability, do not change its declared status to
`complete` without evidence from route/screen, API integration, required
states, and verification.

Regenerate `catalogs/OPERATION_CATALOG.json` after controller-route changes with
`IceBot-Tools/docs-ops/commands/export_rest_operation_catalog.py`; never edit
that generated file by hand. Update `catalogs/MESSAGE_CATALOG.yaml` deliberately when
an Edge/Cloud message family changes.

When a change affects another repository, add a precise entry to
`changes/CONTRACT_CHANGES.yaml`. The entry must name affected `FLOW-*` and `CAP-*`
identifiers; a commit message alone is not a reliable cross-repository handoff.

## Target Contracts

Each target folder contains its `CONTRACT.yaml`; frontend targets may also have
a shared `STATUS.yaml`. Non-client targets use `related_flows` to name only the
backend flows relevant to their runtime responsibility. Backend, IoT,
Fairino-Studio, and QA use the same target model without being presented as
frontend clients.

## Related Sources

- [Product](../product/README.md)
- [Backend System Flows](../../IceBot-Backend/docs/flows/SYSTEM_FLOWS.md)
- [Backend Documentation Coverage](../../IceBot-Backend/docs/DOCUMENTATION_COVERAGE.md)
