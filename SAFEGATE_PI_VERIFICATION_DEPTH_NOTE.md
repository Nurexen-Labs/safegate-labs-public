# SafeGate Pi Verification Depth Note

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Document Status

**Status:** Pi verification depth planning note  
**Scope:** V9.1 backend + integrity hardening  
**Current Public Phase:** Pilot Readiness  
**Controlled Pi Testnet Payment Spine:** PASSED  
**Production Claim:** No  
**Pi Mainnet Settlement Claim:** No  
**Official Pi Partnership Claim:** No  
**Security Certification Claim:** No  
**Third-Party Audit Claim:** No  

---

## Important Boundary

This document is a technical clarification note.

It does not claim Pi Mainnet settlement.

It does not claim official Pi Core Team partnership.

It does not claim production readiness.

It does not claim formal audit completion.

It defines what “backend-verified payment state” should mean for SafeGate before stronger pilot or production claims.

---

## Why This Note Exists

Technical reviewers asked an important question:

When SafeGate says “backend-verified payment state,” what exactly is being verified?

This note defines the intended verification depth.

SafeGate should avoid vague verification language.

The phrase “backend verified” should map to concrete checks.

---

## Current Foundation

SafeGate has demonstrated a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow showed:

- V9 invoice creation
- initial access state locked
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend approval / completion flow
- backend-verified payment state
- receipt and evidence generation after verified state
- public verify confirmation
- access unlock only after backend-verified receipt

Public-safe confirmed result:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- verified: true
- minimumDisclosure: true
- publicSafe: true
- paymentIdIncluded: false
- txidIncluded: false
- secretsIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

---

## Core Verification Principle

SafeGate should not trust frontend payment callbacks alone.

SafeGate should only move toward receipt/evidence/access unlock after server-side verification.

The server must verify that the payment event matches the intended SafeGate invoice/trust event.

---

## Minimum Verification Checks

A backend-verified SafeGate payment state should include these checks.

| Check | Purpose | Status |
|---|---|---|
| invoice exists | Prevent unknown invoice verification | Required |
| invoice is still eligible | Prevent expired/cancelled invoice verification | Required |
| paymentId exists | Identify Pi payment object | Required |
| paymentId is bound to invoice | Prevent cross-invoice payment reuse | Required |
| amount matches invoice | Prevent underpayment/overpayment mismatch | Required |
| payment environment is correct | Keep Sandbox/Testnet/Mainnet boundaries clear | Required |
| payment state is completed/verified | Prevent pending or failed payment from unlocking access | Required |
| txid exists when required | Confirm completion payload | Required |
| txid not reused | Prevent replay across invoices | Required |
| receipt/evidence not already duplicated | Preserve idempotency | Required |
| access state remains locked before verification | Prevent premature unlock | Required |

---

## Invoice Binding

The payment must be bound to the intended invoice.

SafeGate should verify:

- invoiceId exists
- orderId exists where used
- invoice is not expired
- invoice is not cancelled
- invoice is not already bound to a different paymentId
- invoice amount matches expected payment amount
- invoice environment matches the payment environment

Target rule:

One invoice should not accept unrelated payment identifiers.

---

## Payment Identifier Binding

SafeGate should bind payment identifiers carefully.

Rules:

- one paymentId should bind to one invoice
- one txid should bind to one payment record where available
- the same paymentId should not verify another invoice
- the same txid should not verify another invoice
- duplicate calls for the same valid invoice/payment should return existing state idempotently

Target behavior:

Duplicate is safe.

Replay is rejected.

---

## Amount Verification

SafeGate should verify that the payment amount matches the invoice amount.

Checks:

- expected amount
- received amount
- currency / Pi unit context
- test amount boundary if used
- no receipt/evidence if amount does not match

Boundary:

The current controlled flow used a small Testnet/Sandbox payment amount.

This does not imply Mainnet pricing, settlement, or commercial fee readiness.

---

## Recipient / App Context Verification

SafeGate should verify that the payment belongs to the intended app/payment context.

Checks may include:

- SafeGate app context
- intended recipient/payment destination where available
- payment metadata/memo if used
- invoice/order reference if used
- environment boundary

Boundary:

Do not publicly expose recipient addresses or sensitive payment identifiers.

---

## Approval Verification

Backend approval should not create receipt/evidence or unlock access.

Approval only means the payment is moving through the server-side payment lifecycle.

Target behavior:

- approve can record server-approved state
- approve does not equal final verification
- approve does not generate receipt/evidence
- approve does not unlock access
- duplicate approve should be idempotent

Possible state:

- PI_APPROVED_BY_SERVER

---

## Completion Verification

Backend completion is the step that may move payment state to verified if all checks pass.

Completion should verify:

- invoice exists
- paymentId matches invoice
- approval was reached when required
- txid is present when required
- Pi payment completion succeeds
- amount/context checks pass
- replay checks pass
- durable state write succeeds

Target behavior:

Only after completion succeeds should SafeGate move to:

- PAYMENT_VERIFIED_TESTNET

Boundary:

Completion in controlled Sandbox/Testnet context does not equal Pi Mainnet settlement.

---

## Receipt/Evidence Guard After Verification

Even after payment completion, receipt/evidence should be created by a separate guard.

The receipt/evidence guard should check:

- payment state is verified
- paymentId matches invoice
- receipt/evidence does not already exist
- record is not failed/expired/cancelled
- durable state is available
- no mismatch or replay condition exists

Target behavior:

- verified payment state first
- receipt/evidence second
- access unlock only after backend-verified receipt/evidence

---

## Environment Boundary

SafeGate must keep environment boundaries explicit.

Possible environments:

- Pi Browser Sandbox
- Pi Testnet-oriented controlled flow
- future Pi Mainnet flow if separately implemented and verified

Current claim:

SafeGate has passed a controlled Pi Testnet / Sandbox-oriented payment-spine flow.

Current non-claim:

SafeGate does not claim Pi Mainnet settlement.

---

## Ambiguous Verification States

Not every payment flow resolves cleanly.

SafeGate should define ambiguous states.

Examples:

- Pi API timeout
- Pi API unavailable
- payment started but not completed
- completion result unclear
- txid missing
- backend write failure
- duplicate callback conflict
- paymentId mismatch
- amount mismatch

Target behavior:

- access remains locked
- no receipt/evidence
- no public verified true
- safe error or manual review state
- no sensitive details exposed

Possible states:

- PAYMENT_PENDING
- COMPLETION_PENDING
- COMPLETION_TIMEOUT
- PAYMENT_VERIFICATION_AMBIGUOUS
- MANUAL_REVIEW_REQUIRED
- PAYMENT_FAILED
- PAYMENT_EXPIRED

---

## Replay Protection Checks

SafeGate should reject replay attempts.

Replay scenarios:

- same paymentId on a different invoice
- same txid on a different invoice
- old completed payment submitted again
- old receipt used for unrelated access
- receipt/evidence pair mixed with another record

Target behavior:

- reject mismatches
- no new receipt/evidence
- no access unlock
- safe public error
- audit event internally

---

## Safe Error Behavior

Verification errors must be public-safe.

Do not expose:

- raw Pi API response
- stack trace
- service role key
- access token
- private wallet data
- customer data
- deployment internals
- environment variables
- internal logs

Use safe error codes such as:

- MISSING_INVOICE_ID
- MISSING_PAYMENT_ID
- MISSING_TXID
- INVOICE_NOT_FOUND
- PAYMENT_NOT_VERIFIED
- PAYMENT_ID_MISMATCH
- AMOUNT_MISMATCH
- APPROVAL_REQUIRED
- COMPLETION_TIMEOUT
- PAYMENT_VERIFICATION_AMBIGUOUS
- REPLAY_REJECTED
- DURABLE_STATE_UNAVAILABLE
- INTERNAL_SAFE_FAILURE

---

## Public Documentation Rule

SafeGate should document verification depth without exposing sensitive implementation secrets.

Safe to document:

- what categories are checked
- what states are possible
- what fails secure
- what public verify excludes
- what is still planned

Not safe to expose:

- API secrets
- signing secrets
- service role key
- raw tokens
- sensitive wallet identifiers
- private deployment details

---

## V9.1 Verification Depth Test Targets

Recommended tests:

1. valid payment verification
2. missing invoiceId rejection
3. missing paymentId rejection
4. missing txid rejection
5. unknown invoice rejection
6. paymentId mismatch rejection
7. duplicate approve idempotency
8. duplicate complete idempotency
9. same paymentId on another invoice rejection
10. same txid on another invoice rejection
11. amount mismatch rejection
12. receipt before verified payment rejection
13. Pi API timeout safe failure
14. durable state write failure safe failure
15. public verify minimum-disclosure confirmation

---

## Current Safe Language

SafeGate may say:

- controlled Pi Testnet payment-spine pass
- backend-verified payment state direction
- Pi verification depth planning
- receipt/evidence after verified payment state
- access unlock only after backend-verified receipt
- no Pi Mainnet settlement claim
- no production readiness claim

SafeGate should not say:

- Pi Mainnet settlement verified
- production-ready Pi payment verification
- official Pi partnership
- fully secure Pi payment infrastructure
- complete anti-replay system
- independently audited payment verification
- enterprise-grade payment verification

---

## Relationship to Receipt Integrity

Payment verification and receipt integrity are related but separate.

Payment verification answers:

Did the payment event match the intended invoice/trust event?

Receipt integrity answers:

Was the resulting receipt/evidence generated by SafeGate backend and preserved without tampering?

Both are needed for stronger future trust claims.

---

## Relationship to AI Agent Readiness

AI agents will need machine-readable trust states.

Before AI agents rely on SafeGate, verification depth must be clear.

AI-facing integrations should not consume vague `verified: true` responses without understanding:

- environment
- payment state
- receipt status
- integrity status
- freshness status
- access state
- error state
- expiry/revocation/dispute state

Therefore, Pi verification depth comes before broad AI Agent Readiness.

---

## Final Verification Rule

Backend verification should mean more than “a callback happened.”

Backend verification should mean:

The server confirmed that the payment event matches the intended SafeGate invoice/trust event, passed replay and mismatch checks, reached the correct payment lifecycle state, and was durably recorded before receipt/evidence/access unlock.

That is the SafeGate verification target.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
