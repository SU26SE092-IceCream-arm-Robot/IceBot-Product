# Product

This area owns the current shared answer to: who uses IceBot, what operating
outcome they need, and which scope/lifecycle rules apply. It does not own API
schemas, persistence, or Edge transport details.

## Read First

| Need | Read |
| --- | --- |
| Project business summary | [Overview](OVERVIEW.md) |
| Roles and scope | [Role And Scope Model](actors-and-scope/ROLE_AND_SCOPE_MODEL.md) |
| Organization, store, kiosk ownership | [Operating Model](actors-and-scope/ORGANIZATION_STORE_KIOSK_OPERATING_MODEL.md) |
| Customer order, fulfillment, and support | [Customer Journey](journeys/CUSTOMER_ORDER_AND_FULFILLMENT.md) |
| Manager's scoped commercial and store-operations work | [Manager Store Operations](journeys/MANAGER_STORE_OPERATIONS.md) |
| Fairino authoring | [Robot Authoring Journey](journeys/ROBOT_AUTHORING_WORKSPACE.md) |
| Package installation, repair, and upgrade | [Production Package Lifecycle](journeys/PRODUCTION_PACKAGE_LIFECYCLE.md) |
| Kiosk readiness, maintenance, deployment, and incident work | [Technical Operations Journey](journeys/TECHNICAL_OPERATIONS.md) |
| Dashboard/workspace ownership | [Workspace And Dashboard Model](operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md) |
| Flow Manager scope and decision boundary | [Flow Manager Discovery](decisions/FLOW_MANAGER_DISCOVERY.md) |
| Proposed read-only Flow Manager V1 | [Operations Flow Hub V1](journeys/OPERATIONS_FLOW_HUB_V1.md) |
| Unresolved product choices | [Open Questions](decisions/OPEN_QUESTIONS.md) |

## Authority Rules

- Use exact role names: `SystemAdmin`, `OrgAdmin`, `Manager`, `Staff`, and
  `Technician`; never infer a generic "Admin" scope.
- A role alone does not grant data access. Organization, store, kiosk, order,
  or endpoint scope is part of every requirement.
- Put an unresolved behavior in [Open Questions](decisions/OPEN_QUESTIONS.md),
  not in a mock, endpoint request, or frontend task.
- For delivery responsibility, capability status, API/message lookup, or AI
  task handoff, continue in [Delivery](../delivery/README.md).

## Related Sources

- [Delivery Contracts](../delivery/README.md)
- [Backend API Surface Rules](../../IceBot-Backend/docs/api/API_SURFACE_RULES.md)
- [Backend Authorization Rules](../../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md)
