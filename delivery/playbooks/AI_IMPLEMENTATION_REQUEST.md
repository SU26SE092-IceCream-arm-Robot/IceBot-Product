# AI Implementation Request Template

```text
You are implementing IceBot contract work in [TARGET REPOSITORY].
Target: [repository ID or role manifest]
Requested flows/capabilities: [FLOW-* and/or CAP-* IDs]

Read in order:
1. IceBot-Product/delivery/playbooks/ROLE_IMPLEMENTATION_CONTRACT.md
2. Target manifest under delivery/targets/<target>/CONTRACT.yaml
3. Relevant FLOW_CATALOG entries and linked product journeys
4. Linked backend flow documents
5. OPERATION_CATALOG.json and/or MESSAGE_CATALOG.yaml for exact lookup
6. Current target repository source

Do not invent routes, messages, product behavior, roles, or tenant scope.
If the request begins by prescribing an API, field, entity, event, or screen,
perform `REQUEST_TRIAGE.md` first and wait for a confirmed direction
before treating it as an implementation task.
Return before implementation:
- capability-by-capability status with file/symbol evidence;
- missing contract, state, integration, or test evidence;
- prioritized tasks with actor, scope, entry, action, failure states, contract,
  and acceptance evidence;
- blocking questions where contracts are genuinely undecided.
```

## Required Output Shape

```text
Target: <repository/role>
Capability: <CAP-* or role responsibility>
Status: complete | partial | missing | contradictory | blocked-by-contract | unverified
Evidence: <path + symbol>
Contract: <FLOW-*>, <OP-* or MSG-* if applicable>
Missing: <precise behavior/state>
Task: <implementable next action>
Acceptance evidence: <test/manual check>
```

## Guardrails

- `OPERATION_CATALOG.json` is generated route evidence, not DTO or lifecycle documentation.
- `MESSAGE_CATALOG.yaml` is an index, not permission to ignore the linked version,
  authentication, idempotency, and retry rules.
- Product docs decide who may act and what outcome matters; backend docs decide
  exact current behavior.
- A requested technical solution is not an API requirement until request triage
  has identified the actor, outcome, owner, and gap in the current flow.
