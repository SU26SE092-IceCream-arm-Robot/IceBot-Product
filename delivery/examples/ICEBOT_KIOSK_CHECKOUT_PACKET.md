# IceBot Implementation Packet: IceBot-Kiosk

Generated navigation only. Inspect the target repository before declaring any status.

## Target

- Manifest: `IceBot-Product/delivery/targets/icebot-kiosk/CONTRACT.yaml`
- Repository: `../../../../IceBot-Kiosk`
- Audience: Customer at one kiosk

## Read Order

1. `IceBot-Product/delivery/playbooks/ROLE_IMPLEMENTATION_CONTRACT.md`
2. `IceBot-Product/delivery/targets/icebot-kiosk/CONTRACT.yaml`
3. Each selected FLOW entry and its linked product/backend documents
4. `delivery/catalogs/OPERATION_CATALOG.json` or `MESSAGE_CATALOG.yaml` only for exact integration lookup
5. Current target repository code and tests

## Applicable Flows

### FLOW-CHECKOUT-EXECUTION: Customer checkout, payment, fulfillment progress, and pickup
- Product journey: `../../product/journeys/CUSTOMER_ORDER_AND_FULFILLMENT.md`
- Backend flow: `../../../IceBot-Backend/docs/flows/CHECKOUT_EXECUTION_FLOW.md`
- Target capabilities: CAP-KIOSK-CHECKOUT-AND-PAYMENT, CAP-KIOSK-ORDER-TRACKING

## Capability Evidence Targets

### CAP-KIOSK-CHECKOUT-AND-PAYMENT
- Flow: FLOW-CHECKOUT-EXECUTION
- Actor/scope: Customer / kiosk and one customer order
- Required states: empty cart, request in progress, validation failure, idempotent retry-safe submission, payment pending, payment expired or failed, payment confirmed
- Inspect: lib/features/kiosk/presentation/screens/cart_screen.dart, lib/features/kiosk/presentation/screens/checkout_screen.dart, lib/features/kiosk/presentation/screens/payment_screen.dart, lib/features/kiosk/data/repositories/order_repository.dart, lib/features/kiosk/data/repositories/payment_repository.dart

### CAP-KIOSK-ORDER-TRACKING
- Flow: FLOW-CHECKOUT-EXECUTION
- Actor/scope: Customer / one customer order through order-access capability
- Required states: payment pending, preparing, ready for pickup, support required, fulfillment issue, access token expired or invalid, reconnecting or polling fallback
- Inspect: lib/features/kiosk/presentation/screens/order_tracking_screen.dart, lib/features/kiosk/presentation/state/kiosk_controller.dart, lib/features/kiosk/data/models/order_models.dart


## Required Audit Output

Use `AI_IMPLEMENTATION_REQUEST.md`: status with file/symbol evidence, missing behavior, implementable task, and acceptance evidence. Do not infer completion from a screen, mock, type, or service stub.
