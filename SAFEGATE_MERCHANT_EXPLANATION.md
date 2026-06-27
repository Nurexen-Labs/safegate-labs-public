# SafeGate Merchant Explanation

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Simple Merchant Summary

SafeGate helps merchants answer one important question:

What happened after payment?

A payment alone does not always prove:

- access was unlocked correctly
- a receipt was created
- evidence was created
- the backend verified the payment
- the customer received the expected outcome
- the result can be checked later without exposing private payment data

SafeGate is designed to connect payment events to verified outcomes, receipts, evidence, access state, and public-safe proof.

---

## What SafeGate Does For A Merchant

SafeGate helps a merchant create a clearer post-payment record.

It can help show:

- an invoice was created
- access started locked
- payment reached backend-verified state
- receipt/evidence was created after verified payment state
- public verify can confirm the outcome
- access unlocked only after backend-verified receipt
- sensitive payment identifiers are not exposed publicly

SafeGate does not replace the payment system.

SafeGate adds a trust layer after payment.

---

## What SafeGate Does Not Do

SafeGate does not:

- process payments
- custody funds
- act as a wallet
- act as a bank
- act as a regulated financial institution
- claim Pi Mainnet settlement at this stage
- claim official Pi Core Team partnership
- claim production readiness
- claim formal security audit completion

SafeGate focuses on post-payment trust.

---

## Current SafeGate Evidence

SafeGate has passed two important controlled evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

These results strengthen SafeGate’s Pilot Readiness.

They do not create a production-readiness claim.

---

## Controlled Pi Testnet Payment Spine — PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow demonstrated:

- V9 invoice creation
- access initially locked
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend approval / completion flow
- backend-verified payment state
- receipt/evidence generation after verified state
- minimum-disclosure public verification
- access unlock only after backend-verified receipt

Public-safe observed result:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
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

Related document:

SAFEGATE_CONTROLLED_PI_TESTNET_PAYMENT_SPINE_PASSED.md

Live evidence page:

https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html

---

## V9.1 Safe Negative Backend Validation — PASSED

SafeGate completed a live V9.1 safe negative backend behavior validation.

Observed validation result:

- total visible checks: 18
- passed checks: 18
- failed checks: 0
- warning checks: 0

Confirmed selected public-safe behavior:

- invoice creation kept access locked
- invoice creation did not create receipt/evidence
- receipt/evidence before verified payment was rejected safely
- missing invoiceId was rejected safely
- missing paymentId was rejected safely
- public verify missing inputs were rejected safely
- public verify unknown pair did not return active verified trust
- validation responses did not show obvious secret/internal leak terms

Related document:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

Live validation page:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Boundary:

This validation does not prove real duplicate approve/complete behavior, real paymentId/txid replay handling, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## Merchant Problem

A merchant may receive a payment signal but still need to know:

- Was the payment actually verified by the backend?
- Did the user receive access?
- Was a receipt created?
- Was evidence created?
- Can the merchant prove what happened later?
- Can support understand the outcome?
- Can a reviewer check the result without seeing private payment data?
- What happens if payment is missing, incomplete, unknown, or not verified?

SafeGate addresses this post-payment trust gap.

---

## Merchant Benefit

SafeGate can help merchants with:

- clearer payment outcome records
- safer access unlock logic
- receipt/evidence trail
- public-safe verification
- reduced screenshot dependence
- better support context
- better dispute context
- clearer reviewer evidence
- structured post-payment state

The main merchant benefit is not payment processing.

The main merchant benefit is post-payment clarity.

---

## Example Merchant Flow

A simple merchant-style SafeGate flow:

1. Merchant creates an invoice or service access request.
2. Customer starts payment.
3. Access remains locked.
4. Backend verifies payment state.
5. Receipt/evidence is created only after verified state.
6. Public verify becomes available.
7. Access unlocks only after backend-verified receipt.
8. Merchant can later review the structured outcome.

This helps answer:

Did payment lead to the correct verified outcome?

---

## Why Access Should Stay Locked First

A merchant should not unlock access just because the frontend says payment happened.

Frontend callback alone can be incomplete, repeated, spoofed, delayed, or ambiguous.

SafeGate’s rule is:

No receipt, no evidence, and no access unlock before backend-verified payment state.

This rule protects both merchant and customer.

---

## Public Verify In Merchant Terms

Public verify is a safe confirmation page or response.

It should confirm the outcome without exposing private payment information.

Public verify may show:

- verified
- publicSafe
- minimumDisclosure
- accessState
- receiptStatus
- evidenceStatus
- privateDataIncluded: false
- secretsIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false

Public verify should not show:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- service role key
- raw Pi API response
- private customer data
- private wallet data

---

## Safe Merchant Language

A merchant may describe SafeGate as:

SafeGate helps verify what happened after payment.

SafeGate does not process the payment.

SafeGate helps create a receipt/evidence trail after backend verification.

SafeGate helps keep access locked until the payment outcome is verified.

SafeGate helps provide public-safe proof without exposing sensitive payment identifiers.

---

## Unsafe Merchant Language

A merchant should not describe SafeGate as:

- a production payment gateway
- a bank
- a wallet
- an official Pi Core Team product
- a guaranteed payment settlement system
- a fully audited security product
- a tamper-proof receipt system
- a regulatory-approved payment processor

These claims are not currently made.

---

## Merchant Pilot Boundary

A controlled merchant-style pilot should be:

- limited
- supervised
- public-safe
- clearly labeled as controlled
- not production-scale
- not open onboarding
- not Mainnet settlement claim
- not official Pi partnership claim
- not formal audit claim

A merchant-style pilot should use clear stop conditions.

---

## Merchant Stop Conditions

A merchant-style controlled pilot should pause if:

- access unlocks before backend-verified payment state
- receipt/evidence is created before verified payment state
- public verify exposes sensitive payment data
- unknown public verify pair returns active verified trust
- duplicate payment callback creates uncontrolled duplicate records
- customer data appears in public evidence
- raw paymentId or txid appears in public evidence
- merchant misunderstands SafeGate as production-ready
- reviewer cannot understand the claim boundaries

---

## Merchant Evidence Rules

Merchant evidence should be public-safe.

Allowed public-safe evidence:

- test type
- general device/browser context
- paymentState label
- accessState label
- verified status
- minimumDisclosure status
- publicSafe status
- pass/fail result
- short feedback

Do not publish unredacted:

- raw paymentId
- raw txid
- wallet address
- recipient address
- customer identifier
- access token
- internal logs
- raw Pi API response
- service role information
- private customer data

---

## Merchant FAQ

### Does SafeGate process my payments?

No.

SafeGate does not process payments.

SafeGate verifies what happened after payment.

---

### Does SafeGate custody funds?

No.

SafeGate does not custody user funds.

---

### Is SafeGate production-ready?

No.

Production readiness is not currently claimed.

---

### Is SafeGate officially partnered with Pi Core Team?

No.

Official Pi partnership is not currently claimed.

---

### Does SafeGate prove Pi Mainnet settlement?

No.

Current evidence is controlled Pi Testnet / Sandbox-oriented evidence.

Pi Mainnet settlement is not currently claimed.

---

### What does SafeGate prove now?

SafeGate has passed:

- controlled Pi Testnet payment-spine evidence
- V9.1 safe negative backend validation for selected public-safe cases

This supports Pilot Readiness and technical review.

---

### What still needs more work?

Remaining work includes:

- duplicate approve behavior
- duplicate complete behavior
- replay protection
- timeout handling
- durable-state failure behavior
- receipt/evidence integrity
- public verify freshness
- rate limiting
- deeper safe error testing

---

## Merchant Review Question

The best merchant review question is:

Would this help me prove what happened after a customer paid?

The best technical review question is:

Where would this architecture break first if it moved from controlled Testnet evidence to a real pilot environment?

---

## Merchant Summary In One Paragraph

SafeGate is a post-payment trust layer for merchants and apps. It does not process payments. It helps verify what happened after payment by connecting backend-verified payment state to receipt, evidence, public-safe verification, and access state. Its current evidence includes a controlled Pi Testnet payment-spine pass and a V9.1 safe negative backend validation pass. SafeGate is in Pilot Readiness, not production readiness.

---

## Safe Public Merchant Pitch

SafeGate helps merchants prove what happened after payment.

Payment is the trigger.

SafeGate verifies the outcome.

Receipt and evidence are generated only after backend-verified payment state.

Access unlocks only after backend-verified receipt.

Public verify can confirm the result without exposing sensitive payment identifiers.

---

## Final Merchant Boundary

SafeGate is useful for post-payment trust review and controlled pilot planning.

SafeGate is not yet production-ready.

SafeGate is not formally audited.

SafeGate does not claim Pi Mainnet settlement.

SafeGate does not claim official Pi partnership.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
