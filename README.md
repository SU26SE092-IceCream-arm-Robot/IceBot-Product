# Project Documentation Index

This file routes readers and AI agents to the smallest useful set of documents.
Do not read every document by default.

## Source Of Truth Levels

| Location | Authority | Use it for |
| --- | --- | --- |
| `IceBot-Product/` | Current shared product/business truth | Business rules and cross-repository specifications |
| `IceBot-Product/product/` | Current shared product truth | Actors, scope, operating model, user-facing journeys, and open product decisions |
| `IceBot-Product/delivery/` | Cross-repository delivery truth | Target responsibility, capability status, contract change handoff, generated route/message indexes, and AI playbooks |
| `IceBot-Backend/ARCHITECTURE.md`, `IceBot-Backend/docs/` | Current backend truth | Implemented backend contracts, schema, API, and accepted current design |
| `IceBot-Tools/` | Tooling truth | Local development tooling, RAG/MCP helpers, generated data, and utility scripts |

When documents conflict, prefer current source-of-truth documents over
unconfirmed proposals or external notes.

## Read First

Start with one of these depending on the task:

- Business flow or user journey: [Product](product/README.md)
- Product actors, operating model, workspace ownership, or open decisions: [Product](product/README.md)
- Robot authoring, package lifecycle, or kiosk technical operation: [Product](product/README.md)
- Audit what a target repository must implement or compare it with backend flows: [Delivery Contracts](delivery/README.md)
- Prepare an AI-assisted task for any repository or target: [Role Implementation Contract](delivery/playbooks/ROLE_IMPLEMENTATION_CONTRACT.md)
- Backend architecture: [../IceBot-Backend/ARCHITECTURE.md](../IceBot-Backend/ARCHITECTURE.md)
- Backend working rules: [../IceBot-Backend/docs/process/WORKING_PROTOCOL.md](../IceBot-Backend/docs/process/WORKING_PROTOCOL.md)

## Routing

| Topic | Read |
| --- | --- |
| Business flow, payment/refund/customer journey | [Product](product/README.md) |
| Product roles, organization/store/kiosk model, workspaces, robot authoring, package lifecycle, kiosk operations, or product open questions | [Product](product/README.md) |
| Target responsibility, capability IDs, implementation status, contract changes, or AI implementation audit | [Delivery Contracts](delivery/README.md) |
| Backend architecture | [../IceBot-Backend/ARCHITECTURE.md](../IceBot-Backend/ARCHITECTURE.md) |
| Backend working protocol | [../IceBot-Backend/docs/process/WORKING_PROTOCOL.md](../IceBot-Backend/docs/process/WORKING_PROTOCOL.md) |
| Domain ownership / bounded contexts | [../IceBot-Backend/docs/architecture/BOUNDARY_CONTEXTS.md](../IceBot-Backend/docs/architecture/BOUNDARY_CONTEXTS.md) |
| Dependency and layer boundaries | [../IceBot-Backend/docs/architecture/DEPENDENCY_RULES.md](../IceBot-Backend/docs/architecture/DEPENDENCY_RULES.md) |
| Naming conventions | [../IceBot-Backend/docs/process/NAMING_RULES.md](../IceBot-Backend/docs/process/NAMING_RULES.md) |
| Internal roles and authorization policies | [../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md](../IceBot-Backend/docs/api/AUTHORIZATION_RULES.md) |
| Data modeling, indexes, soft delete, ERD checks | [../IceBot-Backend/docs/data/DATA_MODELING_RULES.md](../IceBot-Backend/docs/data/DATA_MODELING_RULES.md) |
| JSON field ownership and snapshots | [../IceBot-Backend/docs/data/JSON_FIELD_RULES.md](../IceBot-Backend/docs/data/JSON_FIELD_RULES.md) |
| Idempotency and retry | [../IceBot-Backend/docs/data/IDEMPOTENCY_RETRY_RULES.md](../IceBot-Backend/docs/data/IDEMPOTENCY_RETRY_RULES.md) |
| Tablet, edge, cloud, IoT contract | [../IceBot-Backend/docs/iot/IOT_CONTRACT.md](../IceBot-Backend/docs/iot/IOT_CONTRACT.md) |
| Backend system flows | [../IceBot-Backend/docs/flows/SYSTEM_FLOWS.md](../IceBot-Backend/docs/flows/SYSTEM_FLOWS.md) |
| Multi-tenancy | [../IceBot-Backend/docs/architecture/MULTI_TENANCY_RULES.md](../IceBot-Backend/docs/architecture/MULTI_TENANCY_RULES.md) |
| Local RAG/MCP/tooling | [../IceBot-Tools/README.md](../IceBot-Tools/README.md) |

## AI Reading Rules

- Read this index first when the task involves documentation.
- Read only the 1-3 documents that match the task.
- Treat links as routing hints, not mandatory recursive reads.
- Do not reread linked files already read in the current task unless the user asks, the file may have changed, or a specific section is needed.
- Use search before opening many files.
- Do not depend on documents outside this repository for implementation
  instructions or product decisions.
- For code changes, read the relevant code and only the docs needed to understand ownership or contracts.
- For a user-facing or operational task, read the matching Product & Operations
  document before inferring a role, tenant scope, dashboard, or workflow from
  a route name or entity name.
