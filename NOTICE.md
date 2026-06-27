# SafeGate Notice

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Current Public Notice

SafeGate is currently in Pilot Readiness.

SafeGate has passed:

- Controlled Pi Testnet Payment Spine
- V9.1 Safe Negative Backend Validation

SafeGate has not claimed:

- production readiness
- Pi Mainnet settlement
- official Pi Core Team partnership
- custodial payment processing
- regulatory approval
- formal third-party audit
- security certification
- tamper-proof infrastructure
- independent cryptographic verification
- completed commercial pilot

---

## Controlled Pi Testnet Payment Spine

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow demonstrated:

- V9 invoice creation
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend approval / completion flow
- backend-verified payment state
- receipt/evidence generation after verified state
- minimum-disclosure public verification
- access unlock only after backend-verified receipt

Observed controlled result:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- public verify: confirmed
- verified: true
- minimumDisclosure: true
- publicSafe: true
- privateDataIncluded: false
- secretsIncluded: false
- rawPiApiResponseIncluded: false
- serviceRoleKeyIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

Boundary:

This was a controlled Pi Testnet / Sandbox-oriented flow.

It does not claim Pi Mainnet settlement or production readiness.

---

## V9.1 Safe Negative Backend Validation

SafeGate completed a live V9.1 safe negative backend behavior validation.

Live validation page:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Public-safe pass note:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

Observed validation result:

- total visible checks: 18
- passed checks: 18
- failed checks: 0
- warning checks: 0

Confirmed selected public-safe behavior:

- backend health returned ok
- invoice creation kept access locked
- invoice creation did not create receipt/evidence
- receipt/evidence before verified payment was rejected safely
- missing invoiceId was rejected safely
- missing paymentId was rejected safely
- public verify missing inputs were rejected safely
- public verify unknown pair did not return active verified trust
- validation responses did not show obvious secret/internal leak terms

Boundary:

This validation does not prove real duplicate approve/complete behavior, real paymentId/txid replay handling, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## What SafeGate Is

SafeGate is a post-payment trust layer.

SafeGate is designed to help verify what happened after a payment event.

SafeGate can connect:

- invoice
- payment state
- backend verification
- receipt
- evidence
- access state
- public-safe verification
- merchant/reviewer proof

SafeGate is Pi-first, not Pi-only.

---

## What SafeGate Is Not

SafeGate is not:

- a custodial wallet
- a payment processor
- a bank
- a regulated financial institution
- a production payment gateway
- an official Pi Core Team product
- a formal audit result
- a security certification
- a regulatory approval claim

SafeGate does not custody user funds.

SafeGate does not claim to settle Pi Mainnet payments at this stage.

SafeGate does not claim an official Pi partnership.

---

## Public-Safe Evidence Notice

Raw screenshots may contain sensitive technical details.

Do not publish unredacted screenshots containing:

- payment identifiers
- transaction identifiers
- wallet addresses
- recipient addresses
- access tokens
- user identifiers
- app identifiers
- internal logs
- raw Pi API responses
- service role information
- private wallet data
- customer data

Public-safe evidence should only show minimum-disclosure confirmation such as:

- verified: true
- minimumDisclosure: true
- publicSafe: true
- privateDataIncluded: false
- secretsIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

---

## Claim Boundary Notice

SafeGate may currently say:

- Controlled Pi Testnet Payment Spine: PASSED
- V9.1 Safe Negative Backend Validation: PASSED
- SafeGate verifies what happened after payment
- SafeGate does not process payments
- access unlock happens only after backend-verified payment state
- public verify follows a minimum-disclosure direction
- Pilot Readiness is open
- production readiness is not claimed
- formal audit is not claimed

SafeGate should not say:

- production ready
- fully secure
- tamper-proof
- officially partnered with Pi Core Team
- Pi Mainnet settlement completed
- formal audit passed
- regulatory approval completed
- enterprise-grade security certified
- all duplicate/replay risks solved
- independent cryptographic verification completed

---

## Technical Boundary Notice

Current completed evidence:

- controlled Pi Testnet payment-spine pass
- V9.1 safe negative backend validation pass
- public-safe evidence pack
- external technical review note
- backend behavior evidence matrix
- receipt integrity plan
- Pi verification depth note
- public verify freshness policy
- formal state machine table
- controlled pilot execution checklist

Remaining technical targets include:

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- real paymentId replay behavior
- real txid replay behavior
- paymentId mismatch behavior
- public verify mismatched pair behavior
- Pi API timeout behavior
- Supabase write failure behavior
- durable-state recovery behavior
- receipt/evidence integrity implementation
- rate limiting / abuse resistance
- deeper safe error response validation

---

## Pilot Readiness Notice

Pilot Readiness means preparation and controlled review.

Pilot Readiness does not mean:

- public launch
- production deployment
- completed commercial pilot
- high-volume operation
- formal security approval
- regulatory approval
- official ecosystem partnership

Any controlled pilot should remain limited, supervised, public-safe, and boundary-clear.

---

## Final Notice

SafeGate has passed important controlled evidence milestones.

SafeGate remains not production-ready and not formally audited.

SafeGate’s current value is its post-payment trust architecture, controlled Pi Testnet evidence, public-safe verification direction, and disciplined boundary management.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
