# Production Package Lifecycle

Production Packages are the normal way for a franchise organization to adopt a
prepared product-and-robot offering. They hide the Artifact, Program, Recipe,
and release graph behind a business workflow while retaining technical
provenance and lifecycle gates.

## Actor And Scope

| Actor | Scope | Goal |
| --- | --- | --- |
| `SystemAdmin` | Global | Author and publish reusable packages and versions. |
| `OrgAdmin`, `Manager` | Assigned organization | Preview, install, review, repair, or upgrade an organization package installation. |
| `Technician` | Assigned kiosk | Resolve technical readiness or deployment blockers; does not silently change commercial/package ownership. |

## Installation Journey

```text
Browse available package versions
  -> preview organization impact
  -> install once
  -> read installation workspace
  -> resolve commercial or technical blockers
  -> publish release through normal release lifecycle
  -> deploy to each eligible kiosk
```

| Step | User decision | What the workspace must make clear |
| --- | --- | --- |
| Browse | Which published package/version fits the organization? | Package purpose, compatible products, required technical capability, and target scope. |
| Preview | Is installation safe and meaningful for this organization? | Created/reused resources, menu impact, readiness blockers, warnings, and no-write nature of preview. |
| Install | Commit the selected package version. | Installation progress, idempotent/retry state, materialized resources, and any failure that needs repair. |
| Review | Is the commercial offer and technical graph ready? | Separate commercial, technical, release, and deployment blockers. |
| Activate | Publish/deploy the resulting release. | Kiosk/endpoint eligibility and deployment state; package installation itself does not imply activation. |

The installation workspace is the screen-level convergence point. Resource
specific commands remain authoritative for mutations and audit.

## Upgrade Journey

```text
Preview newer version
  -> materialize a successor installation
  -> independently review/publish/deploy successor release
  -> cut over commercial bindings
  -> monitor
  -> rollback only with retained evidence and successful deployment reversal
```

An upgrade must never silently overwrite organization-owned commercial changes.
The UI must explicitly separate:

- package-managed technical resources;
- organization-owned prices, names, availability, and menu choices;
- changes that block cutover or rollback;
- abandon versus rollback.

Use **abandon** before cutover when the successor should be discarded. Use
**rollback** after a completed cutover when restoring source behavior and menu
bindings is required. They are not interchangeable.

## Required UI States

- No package is available for the active organization.
- Preview contains blocking incompatibilities.
- Installation is materializing, failed, ready for review, or installed.
- Package-managed resource requires explicit fork before technical mutation.
- Successor is ready but release/deployment has not been activated.
- Cutover is blocked by stale evidence, changed menu bindings, or inactive
  endpoint deployment.
- Rollback remains pending Edge activation; this is not proof that rollback is
  complete.

## Boundaries

Do not expose the package's internal artifact/program/route graph as required
form fields in the normal install UI. Show it as reviewed evidence and provide
links to technical details for authorized users.

See [Backend Production Package Installation Flow](../../../IceBot-Backend/docs/flows/PRODUCTION_PACKAGE_INSTALLATION_FLOW.md)
and [Backend Production Package Upgrade Flow](../../../IceBot-Backend/docs/flows/PRODUCTION_PACKAGE_UPGRADE_FLOW.md)
for exact API contracts and lifecycle constraints.
