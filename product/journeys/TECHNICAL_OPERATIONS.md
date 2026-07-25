# Technical Operations Journey

Technical Operations is the kiosk-scoped workspace for keeping an installed
location safe and capable of fulfilling orders. It is not a generic
administrator dashboard and it does not grant commercial configuration rights.

## Actor And Scope

| Actor | Scope | Primary responsibility |
| --- | --- | --- |
| `Technician` | Assigned kiosk | Diagnose runtime/device/deployment readiness and perform permitted technical actions. |
| `Staff` | Assigned store/kiosk | Refill, clean, restock, report a fault, and record on-site evidence. |
| `Manager`, `OrgAdmin` | Assigned organization | Decide whether sales should resume and follow up operational incidents. |

## Operating Loop

```text
Select kiosk
  -> read current sales, readiness, inventory, runtime, and deployment state
  -> classify blocker
  -> perform only the permitted action
  -> wait for authoritative evidence
  -> confirm readiness before reopening sales
```

The UI must distinguish these conditions. They are not interchangeable:

| Condition | Meaning | Typical owner |
| --- | --- | --- |
| Store closed | New sales are not accepted because of commercial schedule/policy. Active paid fulfillment may continue. | Manager/OrgAdmin |
| Kiosk sales paused | The kiosk intentionally does not accept new orders. | Manager/Staff/Technician according to policy |
| Inventory not ready | Required ingredient/container/device/topology is missing or inactive. | Staff/Technician |
| Deployment not ready | The selected release is not active on an eligible endpoint. | Technician |
| Runtime unhealthy | Edge/device/storage/connectivity evidence cannot safely support execution. | Technician |
| Production incident | An order has uncertain, defective, or partial physical output. | Staff plus Manager/Technician |

## Required Screens

1. Kiosk readiness overview: sales state, store state, active release/deployment,
   endpoint health, inventory readiness, and current blocker summary.
2. Operational work queue: refill, cleaning, maintenance tickets, alert follow-up,
   pending incidents, and active fulfillment that must continue after sales stop.
3. Technical evidence view: diagnostics, logs, deployment history, and raw
   payloads only for users with diagnostics permission.
4. Recovery confirmation: an action may be requested, but the screen remains
   pending until Cloud receives authoritative health, deployment, or execution
   evidence.

## Safety Rules

- Online does not mean accepting orders.
- Stop accepting new orders does not cancel paid or active fulfillment.
- A controller/Edge restart during robot execution does not prove a safe resume.
- If runtime storage cannot persist state/evidence, execution must be treated as
  unsafe or unknown rather than automatically completed.
- Do not present a generic retry for a partial production outcome; remake is
  evaluated against the exact failed unit range and evidence.

## Related Sources

- [Workspace And Dashboard Model](../operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md)
- [Customer Order And Fulfillment Flow](CUSTOMER_ORDER_AND_FULFILLMENT.md)
- [Backend Alert Lifecycle Flow](../../../IceBot-Backend/docs/flows/ALERT_LIFECYCLE_FLOW.md)
- [Backend Production Incident Resolution Flow](../../../IceBot-Backend/docs/flows/PRODUCTION_INCIDENT_RESOLUTION_FLOW.md)
- [Backend Deployment Configuration](../../../IceBot-Backend/docs/operations/DEPLOYMENT_CONFIG.md)
