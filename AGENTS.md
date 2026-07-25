# IceBot Product Documentation Guide

Operational rules for AI agents and contributors working in this repository.

## Repository Role

`IceBot-Product` is the current shared product/business source of truth and the
cross-repository delivery-contract repository. It is used by backend,
WebApp, Kiosk, Mobile, IoT, backend, QA, and operations contributors.

It does not replace technical source of truth in an owning implementation
repository. Exact API schemas, persistence, runtime messages, and code evidence
remain in that repository.

## Reading Rules

- Start at [README.md](README.md) and read the smallest matching document.
- Read `product/` for actor, scope, business goal, user journey, and
  operating decision.
- Read `delivery/` when auditing or assigning cross-repository work.
- Follow links into `IceBot-Backend/docs/` only for the current technical
  contract needed by the task.
- Do not load every flow or repository by default.

## Decision Rules

- Record confirmed product decisions directly and label unresolved choices as
  open questions.
- Do not infer a business decision from an entity name, route name, mock, or
  current frontend behavior.
- When a request proposes a specific API, entity, field, event, or screen, use
  `delivery/playbooks/REQUEST_TRIAGE.md` before recommending or
  implementing that solution.
- A confirmed product choice must state actor, scope, goal, happy path, failure
  states, decision authority, and implementation consequence.


## Frontend Audit Rules

- Use stable `FLOW-*` and `CAP-*` identifiers from
  `delivery/`.
- Inspect the target client repository before reporting status.
- A page, service, type, mock, or generic error state alone is not completion.
- Report file/symbol evidence for every `complete`, `partial`,
  `contradictory`, or `unverified` result.
- Do not create a target task for a flow that assigns no responsibility to that
  target.
- Do not invent a backend endpoint when the owning flow already defines one.

## Change Rules

- Preserve user-facing meaning when reorganizing documentation.
- Update the product journey, flow catalog, and affected target manifests
  together when responsibility changes.
- When a confirmed change affects another repository, add an impact entry to
  `delivery/changes/CONTRACT_CHANGES.yaml` and name affected stable
  `FLOW-*` and `CAP-*` IDs.
- Do not advance a target's acknowledgement or mark a capability complete
  without its repository evidence.
- Do not silently change role/scope ownership or promote a recommendation to a
  confirmed decision.
- Do not stage, commit, push, or configure a remote unless the user explicitly
  asks.
