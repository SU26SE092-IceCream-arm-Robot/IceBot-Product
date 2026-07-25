# Contract Change Protocol

This protocol answers a practical question: when a backend or product change
lands, how does a target repository learn whether it must act?

## When An Entry Is Required

Append one `../changes/CONTRACT_CHANGES.yaml` entry when a confirmed change alters any of:

- actor, tenant scope, allowed action, or visible lifecycle state;
- required REST operation, GraphQL read model, SignalR behavior, or IoT message;
- validation, idempotency, retry, failure/recovery, or permission behavior a
  client must handle;
- capability responsibility or acceptance evidence.

Do not add entries for formatting-only changes, internal refactors with no
contract effect, or regenerated catalog content with identical operations.

The source owner is responsible for adding the entry. A receiving frontend or
Edge contributor must not discover a changed requirement by reading arbitrary
backend commits. CI can enforce this in the Product repository with
`check_contract_change_coverage.py --base <merge-base>`.

## Entry Shape

```yaml
- id: CHANGE-0001
  title: Example only - do not retain this example as a real change
  status: confirmed
  changed_at: 2026-07-26
  source: IceBot-Backend/docs/flows/EXAMPLE.md
  affected_flows: [FLOW-CHECKOUT-EXECUTION]
  affected_targets:
    IceBot-WebApp: [CAP-WEB-ORDER-PAYMENT-OPERATIONS]
    IceBot-Kiosk: [CAP-KIOSK-CHECKOUT-AND-PAYMENT]
  impact: Customer payment state now exposes an explicit expiry outcome.
  required_review:
    - Render expiry separately from generic payment failure.
  compatibility: Breaking client behavior until handled.
```

`id` must be monotonically increasing. `status` is normally `confirmed`; use
`proposed` only when recipients must not implement it yet.

## Recipient Workflow

```text
Confirmed contract change
-> entry names target capability IDs
-> change-packet tool selects entries after target acknowledgement
-> target AI audits source against required review
-> target updates shared status evidence
-> target advances acknowledgement after review
```

Acknowledgement means "reviewed"; it never means "implemented". A `partial` or
`missing` status is valid after acknowledgement, as long as remaining work is
explicit.

## Related Sources

- [Implementation Status Registry](../targets/README.md)
- [AI Implementation Request Template](AI_IMPLEMENTATION_REQUEST.md)
- [Flow Catalog](../catalogs/FLOW_CATALOG.yaml)
