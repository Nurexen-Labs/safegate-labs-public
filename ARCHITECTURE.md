# Architecture

SafeGate is a payment-triggered trust layer.

The payment itself is not the final product. The trust event after payment is the product.

## Core Flow

1. Buyer starts a payment flow
2. Pi Testnet payment succeeds
3. Backend verifies finalized payment state
4. Receipt proof is generated
5. Protected access is unlocked
6. Merchant-side evidence record is created

## Trust Objects

- Order ID
- Payment ID
- Transaction ID
- Receipt ID
- Receipt proof
- Access status
- Merchant record
- Invoice ID
- Invoice status

## Invoice Lite

Invoice Lite is currently UX + data model only.
No backend payment binding change.
No receipt logic change.
No merchant record logic change.
