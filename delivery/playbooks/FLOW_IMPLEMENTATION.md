# Flow Implementation Template

Use this template when a current backend flow gains a frontend responsibility
or when a new product journey is accepted.

```yaml
id: FLOW-EXAMPLE
title: Short user-facing flow name
backend_flow: ../../IceBot-Backend/docs/flows/OWNING_FLOW.md
product_journey: ../../product/journeys/OWNING_JOURNEY.md
frontend:
  IceBot-Kiosk: [CAP-EXAMPLE-KIOSK]
  IceBot-WebApp: [CAP-EXAMPLE-WEB]
  none: false
```

Each capability must declare:

```yaml
id: CAP-EXAMPLE-WEB
actor: [OrgAdmin, Manager]
scope: organization | store | kiosk | order | global
entry: route or screen intent
backend_operations:
  - stable operation name from the owning backend flow
required_states:
  - loading
  - empty
  - permission-denied
  - flow-specific failure state
evidence_targets:
  - route/screen
  - service/query/command
  - state model
  - tests
```

Keep request/response schemas in the backend contract. Keep user decision and
screen behavior in the product journey. This template records only the bridge
needed to audit delivery.
