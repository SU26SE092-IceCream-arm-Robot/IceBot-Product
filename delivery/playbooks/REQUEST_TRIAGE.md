# Request Triage Protocol

Use this protocol before proposing or implementing a new API, entity, field,
event, dashboard, or integration. A request phrased as a technical solution is
not proof that the proposed boundary is needed.

## Required Response Sequence

```text
Requested solution
-> restate operating goal in plain language
-> identify actor, tenant/resource scope, and trigger
-> read matching FLOW-* and current technical contract
-> compare existing path with goal
-> classify: use existing flow | missing read model | missing command | open product decision
-> explain consequence and only then propose a change
```

Before calling a request a gap, the AI must answer:

| Question | Why it matters |
| --- | --- |
| Who performs this action? | Prevents a customer, OrgAdmin, technician, Edge, and provider from sharing an unsafe surface. |
| What resource owns it? | Establishes organization, store, kiosk, endpoint, order, or release scope. |
| What operational outcome is needed? | Separates a real workflow from a guessed API shape. |
| Does a current flow already produce that outcome? | Avoids duplicate APIs and competing sources of truth. |
| What lifecycle/security/audit rule would a new surface bypass? | Identifies why a direct shortcut may be unsafe. |
| What is actually absent if the current flow is insufficient? | Narrows a genuine gap to read model, command, contract, or decision. |

## Required Answer Shape

```text
Understanding of the goal:
<plain-language outcome, actor, scope>

Current flow:
<FLOW-* and current path, with exact contract references>

Why the requested API is or is not the correct boundary:
<ownership, authorization, lifecycle, integrity, retry/audit effects>

Recommendation:
<use current operation | add read model | add command | decision needed>

If a new contract is needed:
<smallest resource owner, actor, input, output, authorization, lifecycle,
idempotency, and acceptance evidence>
```

The AI must not silently create the requested API after this analysis. A new
public contract still needs an explicit implementation request or confirmed
product decision.

## Worked Example: "Machine Identifier In, Lua ZIP Out"

Requested solution:

```text
POST /machines/{machineId}/lua-bundle
-> ZIP containing Lua files for that machine
```

The AI must first distinguish these possible goals:

| Actual goal | Current/appropriate boundary |
| --- | --- |
| Install or activate a release on a Full Edge machine | Authenticated execution-endpoint deployment: Cloud commits a deployment, Edge receives a wake-up and pulls `DeployConfiguration`; its payload contains a verified Full Edge bundle descriptor and individual artifacts. |
| Recover a Full Edge machine with the current approved release | The same deployment/recovery flow. The bundle is transport data scoped to the accepted command, not a general resource download. |
| Update a low-cost controller | Its deployment uses selected artifact descriptors; it does not require a ZIP bundle. |
| Let a technician inspect approved release files | Potential diagnostics/download read requirement. It needs an explicit actor, release/kiosk scope, permission, expiry, audit policy, and decision whether source files may leave the private object boundary. |
| Choose files merely by an arbitrary machine identifier | Not a sufficient contract. It bypasses endpoint provisioning, kiosk/organization scope, release publication, compatibility, deployment history, checksums, and activation reporting. |

Current technical facts are in [Edge Command Contract](../../../IceBot-Backend/docs/iot/EDGE_COMMAND_CONTRACT.md):

- a `KioskExecutionEndpoint` is provisioned for one kiosk and execution profile;
- supported robot targets include runtime target, machine model, and optional
  same-kiosk device binding;
- Full Edge deployment delivers an immutable bundle descriptor plus individual
  artifact descriptors only through authenticated command pull;
- Edge verifies size/checksum and reports the accepted deployment outcome;
- low-cost deployment does not require the Full Edge ZIP.

Therefore the initial answer is normally: use the existing deployment flow,
not a generic ZIP endpoint. If the technician-inspection use case is real,
record it as a separate read-model/product decision rather than mutating the
deployment transport into a public file API.

## Related Sources

- [Role Implementation Contract](ROLE_IMPLEMENTATION_CONTRACT.md)
- [AI Implementation Request Template](AI_IMPLEMENTATION_REQUEST.md)
- [Flow Catalog](../catalogs/FLOW_CATALOG.yaml)
- [Edge Command Contract](../../../IceBot-Backend/docs/iot/EDGE_COMMAND_CONTRACT.md)
