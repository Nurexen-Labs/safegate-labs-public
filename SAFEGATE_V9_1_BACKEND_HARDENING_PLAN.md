# SafeGate V9.1 Backend + Integrity Hardening Plan

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document defines SafeGate V9.1 backend and integrity hardening direction.

The purpose is to move from controlled evidence toward stronger backend behavior review.

This is not a production-readiness claim.

This is not a formal audit.

This is not a security certification.

This is a hardening plan.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Pilot Readiness | Open |
| Backend + Integrity Hardening | Active direction |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Security Certification | Not claimed |

---

## Completed Evidence Milestone 1 — Controlled Pi Testnet Payment Spine

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

## Completed Evidence Milestone 2 — V9.1 Safe Negative Backend Validation

SafeGate completed a live V9.1 safe negative backend behavior validation.

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

Related document:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

Live validation page:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Boundary:

This validation does not prove real duplicate approve/complete behavior, real paymentId/txid replay handling, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## V9.1 Core Rule

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Access should unlock only after:

1. invoice exists
2. payment belongs to invoice
3. payment is backend verified
4. receipt/evidence is created after verified state
5. public verify path is available
6. access state transitions legally

---

## Why V9.1 Exists

The controlled V9 payment-spine pass proved a useful happy-path flow.

However, production-like trust requires more than a happy path.

V9.1 exists to harden:

- duplicate callbacks
- replay attempts
- invalid inputs
- paymentId mismatch
- txid mismatch
- timeout behavior
- durable-state failure behavior
- safe error responses
- receipt/evidence integrity
- public verify freshness
- rate limiting
- formal state transition clarity

---

## Hardening Area 1 — State Machine Strictness

SafeGate must maintain strict legal transitions.

Expected legal direction:

- INVOICE_CREATED
- PAYMENT_PENDING
- PAYMENT_VERIFIED_TESTNET
- RECEIPT_EVIDENCE_CREATED
- PUBLIC_VERIFY_AVAILABLE
- ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT

Illegal examples:

- INVOICE_CREATED → ACCESS_UNLOCKED
- PAYMENT_PENDING → RECEIPT_CREATED
- FRONTEND_CALLBACK → ACCESS_UNLOCKED
- FAILED_PAYMENT → ACCESS_UNLOCKED
- UNKNOWN_PUBLIC_VERIFY_PAIR → VERIFIED
- MISSING_PAYMENT_ID → RECEIPT_CREATED
- MISSING_INVOICE_ID → EVIDENCE_CREATED

Current status:

Selected safe negative illegal paths passed in V9.1 validation.

Still needed:

- deeper duplicate/replay/mismatch state transition testing

---

## Hardening Area 2 — Idempotency

Payment callbacks and receipt/evidence requests may repeat.

Expected behavior:

- duplicate approve should not create inconsistent state
- duplicate complete should not create inconsistent state
- duplicate receipt/evidence request should not create uncontrolled duplicate records
- repeated public verify should return stable minimum-disclosure state
- repeated valid request should return same safe outcome or safe already-finalized response

Current status:

Documented as hardening target.

Still needed:

- duplicate approve test with real paymentId
- duplicate complete test with real paymentId / txid
- duplicate receipt/evidence test
- stable idempotent response documentation

---

## Hardening Area 3 — Replay Resistance

Old or mismatched payment identifiers must not create new trust outcomes.

Expected behavior:

- paymentId must belong to the invoice
- txid must not be reused
- paymentId must not be reused across invoices
- replayed payment should not create a new receipt
- replayed payment should not create new evidence
- replayed payment should not unlock access for another invoice

Current status:

Documented as hardening target.

Still needed:

- real paymentId replay test
- real txid replay test
- paymentId mismatch test
- txid mismatch test

---

## Hardening Area 4 — Receipt / Evidence Guard

Receipt and evidence must be created only after backend-verified payment state.

Expected behavior:

- no receipt before verified payment
- no evidence before verified payment
- no access unlock before receipt/evidence
- no duplicate uncontrolled receipt/evidence
- receipt/evidence must bind to invoice
- receipt/evidence must bind to verified payment state

Current status:

Selected public-safe validation passed.

V9.1 validation confirmed receipt/evidence before verified payment was rejected safely.

Still needed:

- duplicate receipt/evidence test
- receipt/evidence mismatch test
- receipt/evidence integrity implementation

---

## Hardening Area 5 — Public Verify Safety

Public verify must not expose sensitive payment data.

Expected behavior:

Public verify may show:

- verified status
- publicSafe status
- minimumDisclosure status
- access state
- receipt/evidence status
- freshness status if implemented

Public verify must not expose:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- service role key
- raw Pi API response
- private customer data
- private wallet data

Current status:

Controlled payment-spine public verify showed minimum-disclosure direction.

V9.1 safe negative validation confirmed unknown pair did not return active verified trust.

Still needed:

- mismatched real receipt/evidence public verify test
- freshness implementation
- revocation / expired / disputed state behavior

---

## Hardening Area 6 — Safe Error Model

Backend error responses must not leak sensitive details.

Expected behavior:

Error responses should not expose:

- stack traces
- database internals
- raw Pi API responses
- environment variables
- access tokens
- service role keys
- private identifiers
- wallet details
- customer data

Current status:

Selected V9.1 leak scans passed.

Still needed:

- deeper safe error response validation
- internal error simulation
- timeout simulation
- database failure simulation

---

## Hardening Area 7 — Pi Verification Depth

Backend verification must be concrete.

Expected backend checks:

- invoice exists
- invoice is eligible
- paymentId exists
- paymentId belongs to invoice
- amount matches invoice
- environment is correct
- payment state is completed/verified
- txid exists when required
- txid is not reused
- paymentId is not reused across invoices
- receipt/evidence is not duplicated
- access state remains locked before verification

Current status:

Documented in:

SAFEGATE_PI_VERIFICATION_DEPTH_NOTE.md

Still needed:

- implementation-level verification evidence
- duplicate/replay tests
- mismatch tests

---

## Hardening Area 8 — Durable-State Failure Behavior

Database and storage failures can create ambiguous state.

Expected behavior:

SafeGate should fail secure if:

- payment verification succeeds but receipt write fails
- receipt write succeeds but evidence write fails
- evidence write succeeds but access unlock fails
- database read fails
- database write fails
- public verify reads stale or unavailable state

No access unlock should happen from ambiguous state.

Current status:

Documented as hardening target.

Still needed:

- Supabase write failure simulation
- partial write failure handling
- durable-state recovery documentation

---

## Hardening Area 9 — Receipt / Evidence Integrity

Minimum disclosure is not the same as tamper evidence.

Future integrity direction:

- canonical receipt payload
- canonical evidence payload
- receipt hash
- evidence hash
- server-side HMAC or signature
- issuedAt
- optional expiresAt
- immutable binding
- tamper-detection behavior
- public-safe integrity metadata

Current status:

Documented in:

SAFEGATE_RECEIPT_INTEGRITY_PLAN.md

Boundary:

SafeGate does not currently claim tamper-proof infrastructure.

SafeGate does not currently claim independent cryptographic verification.

---

## Hardening Area 10 — Public Verify Freshness

Public verify should not become stale or misleading.

Future public verify should distinguish:

- active
- expired
- revoked
- disputed
- stale
- unavailable
- review-required

Current status:

Documented in:

SAFEGATE_PUBLIC_VERIFY_FRESHNESS_POLICY.md

Still needed:

- freshness implementation
- stale-state handling
- dispute/revocation modeling

---

## Hardening Area 11 — Rate Limiting / Abuse Resistance

Automated callers and AI agents can create abuse pressure.

Expected future protection:

- rate limiting
- anti-enumeration behavior
- idempotency keys
- abuse monitoring direction
- stable safe errors
- strict state machine rejection
- no information leaks under probing

Current status:

Documented as future hardening target.

Not production-claimed.

---

## Current V9.1 Validation Coverage

The first V9.1 live validation covered selected public-safe negative cases:

- health check
- valid invoice create
- invoice access remains locked
- invoice does not create receipt/evidence
- receipt/evidence before verified payment rejected
- missing invoiceId rejected
- missing paymentId rejected
- public verify missing inputs rejected
- public verify unknown pair rejected / not verified
- selected leak scans

This is meaningful.

It is not complete backend hardening.

---

## Remaining V9.1 Test Queue

Priority test queue:

1. duplicate approve with real paymentId
2. duplicate complete with real paymentId and txid
3. duplicate receipt/evidence request
4. paymentId mismatch
5. txid mismatch
6. public verify mismatched receipt/evidence pair
7. real paymentId replay
8. real txid replay
9. Pi API timeout behavior
10. Supabase write failure behavior
11. partial receipt/evidence write failure
12. access unlock write failure
13. public verify stale-state behavior
14. deeper safe error response validation
15. rate limiting / abuse probe validation

---

## Reviewer Checklist

A technical reviewer should ask:

1. Can access unlock from any path other than backend-verified receipt?
2. Can receipt/evidence be created before verified payment?
3. Are duplicate callbacks idempotent?
4. Are replayed payment identifiers rejected?
5. Can paymentId from one invoice affect another invoice?
6. Can public verify return verified for unknown or mismatched pairs?
7. Can backend errors leak secrets?
8. What happens if database write fails after payment verification?
9. Is receipt/evidence tamper-evident?
10. Can public verify become stale?

---

## Safe Public Language

SafeGate may say:

- V9.1 backend + integrity hardening is active
- controlled Pi Testnet payment-spine passed
- V9.1 safe negative backend validation passed
- selected negative backend cases failed safely
- invoice creation kept access locked
- receipt/evidence before verified payment was rejected safely
- unknown public verify pair did not return active verified trust
- production readiness is not claimed
- formal audit is not claimed

SafeGate should not say:

- backend is fully secure
- production-ready backend completed
- all duplicate/replay risks solved
- all edge cases proven
- tamper-proof proof completed
- independent cryptographic verification completed
- formal audit passed

---

## Final V9.1 Position

V9.1 is the backend and integrity hardening layer after the controlled Pi Testnet payment-spine pass.

SafeGate has passed a first live safe negative backend validation.

The next credibility layer is deeper duplicate, replay, mismatch, timeout, durable-state failure, freshness, integrity, and rate-limit testing.

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
