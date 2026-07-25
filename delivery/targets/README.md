# Delivery Target Registry

This folder contains the delivery contract and shared status record for each
target repository. Status is evidence of what a target has actually proved
against its assigned `CAP-*` contracts; it is not a task board and must not be
updated from a mock, route name, or unreviewed path match.

Each target file records:

- the last contract change it has acknowledged;
- one status per assigned capability;
- file/symbol/test evidence for a reviewed status;
- precise remaining behavior when status is `partial`, `missing`, or
  `contradictory`.

`unverified` is the correct initial state when the repository has not been
audited. It is intentionally different from `missing`.

## Update Rule

1. Run the target evidence audit and change packet.
2. Let an AI or reviewer inspect the linked source and contracts.
3. Update the target status file with evidence and remaining work.
4. Advance `last_acknowledged_change_id` only after the target has reviewed the
   corresponding change packet. Acknowledging a change does not mean the work
   is complete.

## Related Sources

- [Frontend Audit Protocol](../playbooks/IMPLEMENTATION_AUDIT.md)
- [Contract Change Protocol](../playbooks/CONTRACT_CHANGE_PROTOCOL.md)
- [Flow Catalog](../catalogs/FLOW_CATALOG.yaml)
