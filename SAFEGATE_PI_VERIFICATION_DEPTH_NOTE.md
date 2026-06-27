# SafeGate Pi Verification Depth Note

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This note clarifies what SafeGate should mean when it says backend-verified payment state.

The phrase backend verified must not be vague.

It should map to concrete checks, state transitions, and public-safe boundaries.

This is a technical clarification note.

This is not a production-readiness claim.

This is not Pi Mainnet settlement.

This is not a formal audit.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Pi Verification Depth | Documented direction |
| Pilot Readiness | Open |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |

---

## Completed Evidence Context

SafeGate has passed two important controlled evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

Together, these show:

- controlled Pi Browser payment-spine execution
- backend-verified payment-state direction
- receipt/evidence generated after verified state
- access unlocked after backend-verified receipt
- selected safe negative backend behavior passed
- missing/unknown inputs rejected safely
- no obvious secret/internal leak terms detected in selected validation responses

They do not prove Pi Mainnet settlement.

They do not prove full production-grade payment verification.

---

## Controlled Pi Testnet Payment Spine — PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

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

## Why Verification Depth Matters

If backend verified is not clearly defined, the trust layer becomes ambiguous.

SafeGate should avoid vague claims like:

- payment verified
- backend confirmed
- payment completed
- access unlocked safely

Unless those phrases map to concrete checks.

Backend verification must mean more than receiving a frontend callback.

Frontend callback alone must never unlock access.

---

## Core Rule

No receipt, no evidence, and no access unlock before backend-verified payment state.

Backend-verified payment state should be the gate before:

- receipt creation
- evidence creation
- public verify active result
- access unlock
- merchant proof
- pilot evidence claim

---

## Minimum Verification Checks

A stronger SafeGate backend verification model should check:

- invoice exists
- invoice is still eligible
- invoice has not expired
- paymentId exists
- paymentId belongs to the expected invoice
- payment amount matches invoice
- payment environment is correct
- payment state is completed or verified
- txid exists when required
- txid is not reused
- paymentId is not reused across invoices
- receipt/evidence does not already exist in unsafe duplicate form
- access state remains locked before verification
- final state transition is legal

---

## Pi Context Boundary

SafeGate’s current evidence is controlled Pi Testnet / Sandbox-oriented evidence.

SafeGate should distinguish:

- Pi Browser authentication
- Pi payments scope
- Pi Sandbox / Testnet payment flow
- backend approve / complete flow direction
- backend-verified Testnet state
- Pi Mainnet settlement

The current controlled evidence supports:

- Controlled Pi Testnet Payment Spine: PASSED

It does not support:

- Pi Mainnet settlement completed
- production Pi payment processing
- official Pi Core Team partnership
- regulatory approval
- full payment security certification

---

## Payment State Labels

SafeGate should keep payment-state labels explicit.

Useful labels:

- INVOICE_CREATED
- LOCKED_UNTIL_BACKEND_VERIFIED_PAYMENT
- PAYMENT_PENDING
- PAYMENT_VERIFIED_TESTNET
- PAYMENT_FAILED
- PAYMENT_EXPIRED
- PAYMENT_AMBIGUOUS
- PAYMENT_REVIEW_REQUIRED
- RECEIPT_EVIDENCE_CREATED
- PUBLIC_VERIFY_AVAILABLE
- ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT

Avoid vague labels such as:

- paid
- done
- complete
- approved
- success

unless they are tied to exact backend checks.

---

## Verification Depth Matrix

| Verification Layer | Meaning | Current Evidence |
|---|---|---|
| Frontend callback | Client-side signal only | Must not unlock access |
| Pi Browser auth | User authenticated in Pi Browser context | Passed controlled flow |
| Payments scope | App received payments permission scope | Passed controlled flow |
| Wallet payment action | Payment flow completed in Sandbox / Testnet context | Passed controlled flow |
| Backend approve / complete direction | Backend flow executed | Passed controlled flow |
| Backend-verified Testnet state | Controlled payment state reached PAYMENT_VERIFIED_TESTNET | Passed controlled flow |
| Receipt/evidence guard | Receipt/evidence only after verified state | Passed controlled flow and selected V9.1 negative validation |
| Public verify minimum disclosure | Public verify avoids sensitive identifiers | Passed controlled flow |
| Negative input rejection | Missing/unknown selected inputs fail safely | Passed selected V9.1 validation |
| Duplicate/replay proof | Duplicate/replay safety with real identifiers | Open V9.1 target |
| Pi API timeout behavior | Fail-secure behavior under timeout | Open V9.1 target |
| Supabase failure behavior | Fail-secure behavior under durable-state failure | Open V9.1 target |
| Pi Mainnet settlement | Real Mainnet settlement verification | Not claimed |

---

## Verification Must Precede Receipt / Evidence

Receipt/evidence should only be generated after backend-verified payment state.

Invalid behavior:

- invoice created → receipt created
- frontend callback → evidence created
- payment pending → access unlocked
- missing paymentId → receipt created
- missing invoiceId → evidence created
- unknown public verify pair → verified trust

V9.1 selected validation confirmed:

- invoice creation did not create receipt/evidence
- receipt/evidence before verified payment was rejected safely
- missing invoiceId was rejected safely
- missing paymentId was rejected safely
- public verify unknown pair did not return active verified trust

---

## Verification Must Precede Access Unlock

Access unlock should only happen after:

1. backend-verified payment state
2. receipt/evidence generated after verified state
3. public verify path available
4. legal final access transition

Invalid behavior:

- frontend callback unlocks access
- invoice creation unlocks access
- payment pending unlocks access
- failed payment unlocks access
- unknown public verify pair unlocks access
- duplicate replay unlocks access

Current evidence:

Controlled Pi Testnet payment-spine pass reached:

- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT

V9.1 validation confirmed:

- invoice creation kept access locked

---

## Duplicate And Replay Depth

A complete verification model must handle duplicate and replay cases.

Open targets:

- duplicate approve
- duplicate complete
- duplicate receipt/evidence request
- same paymentId reused across invoices
- same txid reused across receipts
- old paymentId replayed after invoice expiry
- paymentId belongs to different invoice
- txid belongs to different payment
- public verify mismatched receipt/evidence pair

Expected behavior:

- reject safely or return stable idempotent state
- no duplicate receipt
- no duplicate evidence
- no invalid access unlock
- no active verified trust from mismatched data

Current status:

Open V9.1 hardening target.

---

## Timeout And Ambiguous State Depth

A complete verification model must fail secure under ambiguous states.

Examples:

- Pi API timeout
- Pi API ambiguous response
- backend approve succeeds but complete fails
- complete succeeds but database write fails
- receipt write succeeds but evidence write fails
- evidence write succeeds but access unlock fails
- public verify reads stale data

Expected behavior:

- no access unlock from ambiguous state
- no active public verified trust from partial state
- safe error response
- review-required or unavailable state if needed

Current status:

Open V9.1 hardening target.

---

## Public Verify Depth

Public verify should be minimum-disclosure.

It may show:

- verified
- publicSafe
- minimumDisclosure
- paymentState label
- accessState label
- receiptStatus
- evidenceStatus
- freshnessStatus if implemented
- integrityStatus if implemented

It should not show:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- service role key
- raw Pi API response
- private customer data
- private wallet data

Current evidence:

Controlled payment-spine public verify showed minimum-disclosure direction.

V9.1 validation confirmed selected missing/unknown public verify inputs failed safely.

---

## Safe Error Depth

Backend verification should not leak sensitive details when failing.

Safe errors should avoid exposing:

- stack traces
- service role keys
- environment variables
- database URLs
- raw Supabase errors
- raw Pi API responses
- access tokens
- bearer tokens
- wallet data
- customer data

Current evidence:

V9.1 selected validation responses did not show obvious secret/internal leak terms.

Deeper safe error testing remains open.

---

## Recommended Verification Language

SafeGate may say:

- backend-verified payment state was reached in controlled Pi Testnet flow
- paymentState reached PAYMENT_VERIFIED_TESTNET
- access unlocked only after backend-verified receipt
- receipt/evidence was generated after verified state
- V9.1 selected negative backend cases failed safely
- production readiness is not claimed
- Pi Mainnet settlement is not claimed

SafeGate should not say:

- Pi Mainnet payment settled
- production payment verification complete
- all payment security risks solved
- all duplicate/replay risks solved
- formal audit passed
- official Pi partnership exists

---

## Reviewer Questions

A reviewer should ask:

1. What exact checks define backend verified?
2. Does paymentId belong to the invoice?
3. Does amount match the invoice?
4. Is the payment environment correct?
5. Is txid required and verified?
6. Can paymentId be reused?
7. Can txid be reused?
8. Can frontend callback unlock access?
9. What happens if Pi API times out?
10. What happens if Supabase write fails?

---

## Final Verification Depth Position

SafeGate has passed a controlled Pi Testnet payment-spine flow.

SafeGate has passed selected V9.1 safe negative backend checks.

SafeGate should continue defining backend verification through concrete checks, not vague labels.

SafeGate does not claim Pi Mainnet settlement, production readiness, or formal audit completion.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
