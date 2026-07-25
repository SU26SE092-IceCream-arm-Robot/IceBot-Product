# Robot Authoring Workspace Journey

This is the normal technical-authoring journey for bringing a Fairino project
into one organization. It is not a general franchise setup flow; use a
Production Package when the organization is adopting a prepared offering.

## Actor And Scope

| Actor | Scope | Goal |
| --- | --- | --- |
| `Technician` | Assigned organization, optionally one Store/Kiosk/Device | Turn a reviewed Fairino export into an organization-owned program and release draft. |
| `OrgAdmin` or `Manager` | Assigned organization, only when granted technical permissions | Review progress and approve business/release decisions; they do not acquire technical rights merely from organization ownership. |

The UI must show the active organization and any optional physical target
before upload. A user must never supply identifiers by hand to establish scope.

## Use This Journey When

Use this workflow when an organization is authoring or importing its own robot
behavior. Do not start with individual Artifact, Technical Contract, or Program
CRUD screens unless the user is deliberately repairing or authoring an advanced
technical graph.

```text
Fairino-Studio export
  -> authoring workspace
  -> reviewed Draft technical resources
  -> semantic composition against Recipe/options
  -> published resources
  -> release Draft
  -> normal release approval and kiosk deployment
```

## Guided Workspace Steps

| Step | User action and expected screen | Workspace outcome | Backend integration owner |
| --- | --- | --- | --- |
| 1. Export | Technician exports the normal IceBot bundle from Fairino-Studio. | A ZIP contains ordered Lua artifacts and their sidecars. | Fairino-Studio export contract. |
| 2. Stage | Upload the bundle once, with the selected technical scope. | A durable authoring session exists; no runtime resource is created. | Create authoring import. |
| 3. Inspect | Open the authoring workspace immediately after every command. | UI receives status, blockers, created IDs, release/deployment context, and allowed next actions. | Authoring workspace read model. |
| 4. Validate | Ask Cloud to validate archive, sidecars, revisions, identities, and order. | Errors stop the workflow; warnings remain visible for review. | Validate import. |
| 5. Materialize | Create Draft contracts, artifacts, and one ordered Draft program. | The technical graph is reviewable but cannot execute. | Materialize import resources. |
| 6. Compose | Select the Recipe and production-affecting option codes, then preview and confirm. | Cloud proves the selected recipe/options can be represented by the imported effects and sets program membership/order. | Composition preview and confirmation. |
| 7. Publish resources | Explicitly publish the imported contracts, artifacts, and program. | The technical resources become immutable inputs for a release. | Publish import resources. |
| 8. Create release draft | Select the Recipe and any required workcell capability. | Cloud derives the program binding and creates one organization release Draft. | Create release draft from import. |
| 9. Activate normally | Review/publish the release and deploy it to an eligible kiosk. | The authoring journey ends; deployment and Edge activation have their own lifecycle. | Release/deployment workflows. |

The UI must reload the workspace after steps 2 and 4 through 9. It must show
only actions returned as allowed by the workspace; it must not infer lifecycle
steps from a local status enum.

## Required UI States

| Condition | Required UI behavior |
| --- | --- |
| Uploaded but not validated | Offer validation and discard only. |
| Validation has errors | Show each issue with artifact identity; do not offer materialization. |
| Validation has warnings | Require a clear review acknowledgement before the user continues. |
| Draft resources created | Show program/artifact/contract review links and composition as the next technical decision. |
| Composition blocked | Show the exact unmatched Recipe ingredient or production option; do not offer publication. |
| Release Draft exists | Hand off to release review, not an artificial second robot-authoring wizard. |
| No eligible deployment endpoint | Preserve the published release and show readiness/deployment blockers. |

## Advanced Technical Authoring

Individual technical-contract, artifact, and program screens are appropriate
only for these cases:

- repairing an existing Draft;
- creating a global template as `SystemAdmin`;
- cloning a reviewed template into an organization;
- deliberately building a technical graph without a Fairino export bundle;
- inspecting, retiring, or replacing a resource under its lifecycle rules.

They must be labelled **Advanced technical authoring**. They are not an
alternative set of mandatory steps for a normal bundle import.

## API Guidance

The backend flow is the API source of truth. FE should treat the import
workspace as the convergence read model and invoke its typed commands in the
order above. Do not create a generic client-side action router or assume that
an action code grants permission.

See [Backend Robot Lua Authoring And Import Flow](../../../IceBot-Backend/docs/flows/ROBOT_LUA_AUTHORING_AND_IMPORT_FLOW.md),
especially its Primary FE Integration Journey and API lookup table.

## Completion

This journey is complete when the organization has a Published release and an
eligible deployment has been initiated or its remaining deployment blocker is
visible. It is not complete merely because Lua bytes were uploaded.
