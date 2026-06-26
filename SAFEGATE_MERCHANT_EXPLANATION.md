# SafeGate Merchant Explanation

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Document Status

**Status:** Merchant-facing explanation  
**Scope:** Simple explanation of SafeGate for merchants and pilot reviewers  
**Current Public Phase:** Pilot Readiness  
**Controlled Pi Testnet Payment Spine:** PASSED  
**Production Claim:** No  
**Commercial Pilot Claim:** No  
**Pi Mainnet Settlement Claim:** No  
**Official Pi Partnership Claim:** No  

---

## Important Boundary

This document explains SafeGate in simple business language.

It does not claim production readiness.

It does not claim Pi Mainnet settlement.

It does not claim official Pi partnership.

It does not claim that SafeGate is ready for broad commercial deployment.

It explains the intended merchant value of SafeGate’s post-payment trust layer.

---

## What Problem Does SafeGate Solve?

A merchant does not only need to know that a payment button was clicked.

A merchant needs to know what happened after the payment event.

Questions a merchant may ask:

- Was an invoice created?
- Did the buyer start payment?
- Was the payment verified by backend?
- Was a receipt created?
- Was evidence created?
- Was access unlocked?
- Can this result be checked later?
- Can the result be shown without exposing private payment data?

SafeGate is designed to answer these questions.

---

## Simple Definition

SafeGate is a post-payment trust layer.

It helps a merchant verify and record what happened after a payment event.

SafeGate does not replace the payment network.

SafeGate does not custody funds.

SafeGate does not process payments.

SafeGate verifies the post-payment outcome.

---

## Simple Flow

A simple SafeGate merchant flow looks like this:

1. Merchant creates or offers a service/product.
2. Buyer starts a payment.
3. SafeGate creates an invoice/payment intent.
4. Access stays locked.
5. Buyer completes payment in the intended environment.
6. SafeGate backend verifies the payment state.
7. SafeGate creates receipt/evidence after backend verification.
8. Public verify can confirm a minimum-disclosure result.
9. Access unlocks only after backend-verified receipt/evidence.

---

## What The Merchant Gets

A merchant may receive:

- invoice reference
- payment state
- receipt reference
- evidence reference
- access state
- public verify result
- minimum-disclosure proof
- merchant-side support record

The goal is to give the merchant a clear post-payment record.

---

## What The Buyer Gets

A buyer may receive:

- clearer payment outcome
- proof that payment led to a verified state
- access only after backend verification
- public-safe confirmation
- no unnecessary exposure of private payment identifiers in public verify

---

## What Public Verify Means

Public verify is a minimum-disclosure confirmation page or endpoint.

It can show that a receipt/evidence pair exists and was verified without exposing private payment data.

Public verify should show safe fields such as:

- verified
- receiptId
- evidenceId
- paymentState
- accessState
- minimumDisclosure
- publicSafe
- accessUnlocked

Public verify should not expose:

- paymentId
- txid
- raw Pi API response
- service role key
- access token
- private wallet data
- customer data
- private merchant data
- internal logs

---

## Example Merchant Scenario

A merchant sells access to a digital service.

The buyer starts payment.

Before payment verification:

- invoice exists
- access is locked
- no receipt is issued
- no evidence is issued
- public verify does not show active verified access

After backend payment verification:

- payment state becomes verified
- receipt is created
- evidence is created
- access unlocks
- public verify can confirm the result with minimum disclosure

This gives the merchant a safer record than relying only on a frontend payment callback.

---

## What Happens If Payment Is Delayed?

If payment is delayed or unclear, SafeGate should fail secure.

That means:

- access stays locked
- no fake success is shown
- no receipt is created before verification
- no evidence is created before verification
- public verify should not show active verified trust
- the record may move to pending, timeout, or review state

This protects both merchant and buyer from false payment outcomes.

---

## What Happens If The Buyer Says “I Paid”?

SafeGate should help the merchant check:

- invoice status
- payment state
- receipt/evidence status
- public verify result
- access state

If backend verification is not complete, SafeGate should not unlock access automatically.

The merchant can use the record for support or review.

---

## What Happens If Something Fails?

SafeGate should fail safely.

Possible failures:

- payment not completed
- Pi API timeout
- backend verification ambiguity
- database read/write failure
- duplicate callback
- replay attempt
- receipt/evidence mismatch
- public verify unavailable

SafeGate should not treat these as automatic success.

---

## Why Not Trust The Frontend Callback?

Frontend callbacks can be unreliable or manipulated.

A buyer browser may refresh.

A network delay may happen.

A callback may be duplicated.

A malicious client may try to fake success.

SafeGate’s rule is:

Frontend callback alone does not unlock access.

Backend verification comes first.

---

## Merchant Value

SafeGate helps merchants by creating:

- clearer post-payment records
- safer access unlock logic
- receipt/evidence trail
- public-safe proof
- support/dispute context
- less reliance on screenshots or manual claims
- structured trust state after payment

---

## What SafeGate Is Not

SafeGate is not:

- a wallet
- a payment processor
- a custodial fund holder
- a bank
- a regulator
- a chargeback system
- a full dispute resolution platform
- a production-ready merchant platform at this stage
- an official Pi Core Team partner
- a Pi Mainnet settlement proof at this stage

---

## What SafeGate Currently Claims

SafeGate currently claims:

- controlled Pi Testnet payment-spine pass
- backend-verified receipt/evidence direction
- access unlock only after backend-verified receipt
- minimum-disclosure public verify direction
- Pilot Readiness preparation
- public-safe evidence discipline
- V9.1 backend hardening planning

---

## What SafeGate Does Not Currently Claim

SafeGate does not currently claim:

- production readiness
- commercial pilot completion
- Pi Mainnet settlement
- official Pi partnership
- formal third-party audit
- regulatory approval
- enterprise-grade security
- tamper-proof infrastructure
- complete dispute automation

---

## Merchant Review Questions

A merchant should ask:

1. What do I see after a buyer pays?
2. How do I know access was unlocked correctly?
3. What happens if the payment is delayed?
4. What happens if the buyer says they paid but access is locked?
5. What proof can I show without exposing private payment data?
6. Can I check a receipt later?
7. What happens if a receipt is expired, revoked, or disputed?
8. What support record do I have?

---

## Controlled Pilot Merchant Shape

A future controlled pilot should start small.

Recommended first pilot shape:

- one merchant-style scenario
- one product or service
- one buyer/tester
- one invoice
- one controlled payment
- one receipt/evidence pair
- one public verify result
- one redacted evidence note
- one review summary

Do not start with broad merchant onboarding.

Do not claim production readiness.

---

## Simple Merchant Summary

SafeGate helps answer the merchant’s post-payment question:

Did this payment lead to a verified outcome, a receipt, evidence, and the correct access state?

That is the merchant value.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
