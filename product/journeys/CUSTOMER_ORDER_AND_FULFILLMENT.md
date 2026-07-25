# Customer Order And Fulfillment Flow

This is the product-level customer and staff flow for a kiosk sale. It defines
what users need to understand and act on; it does not replace the Cloud/Edge
message contract.

## Confirmed Current Flow

```text
Customer at kiosk
  -> sees the kiosk runtime menu
  -> chooses one or more sellable items
  -> places an order
  -> receives a payment session / QR instruction
  -> provider confirms payment
  -> each order item follows its fulfillment type
  -> customer receives available completed output
  -> Cloud retains payment, fulfillment, and operational evidence
```

Customer ordering does not require a customer account. The order-access token
is the customer capability for that one order; it is not an internal login.

## Fulfillment Types

| Item type | Normal completion | Primary human action when work cannot finish |
| --- | --- | --- |
| Machine-produced | Edge accepts a production command and reports unit outcomes | Inspect the incident; choose delivery, remake, or compensation according to evidence |
| Packaged | Staff marks the exact item fulfilled or failed | Hand over the packaged item or record the failure |
| Manual | Staff records the exact fulfillment event | Perform and record the manual work |

An order may contain more than one item and more than one fulfillment type.
One completed item must not make the whole order appear completed while another
item is pending, failed, or requires inspection.

## Customer Experience States

| Customer-facing state | Meaning | Expected next action |
| --- | --- | --- |
| Payment pending | Order exists but payment is not provider-confirmed | Complete payment before expiry or start a new order later |
| Preparing | Paid item is queued or executing | Wait for progress; this is not proof that output exists |
| Ready for pickup | The relevant item/order output is completed | Collect the product |
| Support required | Cloud cannot safely confirm outcome or a production issue needs staff action | Contact staff; do not promise an automatic refund |
| Fulfillment issue | At least one paid item needs operational resolution | Staff inspects the affected item/range; completed output and evidence remain visible |

## Paid Production Issue

```text
Provider confirms payment
  -> kiosk/robot cannot accept, complete, or safely report a production unit
  -> Cloud preserves the paid transaction and execution evidence
  -> staff receives an incident/support workflow
  -> staff inspects the exact affected output or unit range
  -> staff may deliver confirmed-good output, request an exact remake,
     or request compensation
```

Do not reduce this to "payment succeeded, therefore refund immediately." A
machine may have produced some units successfully, may have produced uncertain
physical output, or may have failed before output. The staff workflow must
show the affected item and unit range rather than asking a person to guess.

## Compensation Limits In The Current Phase

- Provider-confirmed payment remains financial truth even when fulfillment
  fails.
- Refund handling is a manual staff workflow; automatic provider refund/payout
  is not promised.
- Current production-incident compensation is full-order, not partial-money
  refund. Partial output still remains visible as operational evidence.
- A remake is only allowed for the exact failed unit range when evidence proves
  that replay is safe.

## UI Responsibilities

- Tablet/customer UI must distinguish payment state from fulfillment state.
- Back-office UI must show the item, fulfillment type, current outcome, and
  allowed next action.
- Staff UI must not offer a generic "retry order" when only one unit/item is
  eligible for remake.
- A compensation action requires a reason and an explicit staff decision.
- Do not expose execution checksums, command identifiers, or raw Edge payloads
  in ordinary operations UI.

## Related Sources

- [Workspace And Dashboard Model](../operating-model/WORKSPACE_AND_DASHBOARD_MODEL.md)
- [Backend Checkout Execution Flow](../../../IceBot-Backend/docs/flows/CHECKOUT_EXECUTION_FLOW.md)
- [Backend Production Incident Resolution Flow](../../../IceBot-Backend/docs/flows/PRODUCTION_INCIDENT_RESOLUTION_FLOW.md)
- [Backend Tablet And Cloud Contract](../../../IceBot-Backend/docs/iot/TABLET_CLOUD_CONTRACT.md)
