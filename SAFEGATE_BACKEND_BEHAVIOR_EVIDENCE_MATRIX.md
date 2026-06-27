# SafeGate Backend Behavior Evidence Matrix

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This matrix defines expected SafeGate backend behavior under normal, negative, duplicate, replay, mismatch, timeout, and failure scenarios.

The purpose is to make backend behavior reviewable.

This is not a production-readiness claim.

This is not a formal audit.

This is not a security certification.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Backend Behavior Matrix | Active |
| Pilot Readiness | Open |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |

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

## Core Backend Rule

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Unknown, missing, ambiguous, duplicate, replayed, mismatched, stale, or failed states should fail secure.

---

## Behavior Matrix

| Scenario | Expected Backend Behavior | Evidence Status |
|---|---|---|
| Backend health check | Return safe ok status without exposing secrets | PASSED in V9.1 validation |
| Valid invoice create | Create invoice and keep access locked | PASSED in V9.1 validation |
| Invoice create access state | Access remains locked until backend-verified payment | PASSED in V9.1 validation |
| Invoice create receipt/evidence | No receipt/evidence at invoice stage | PASSED in V9.1 validation |
| Receipt/evidence before verified payment | Reject safely; no receipt, no evidence, no access unlock | PASSED in V9.1 validation |
| Missing invoiceId | Reject safely | PASSED in V9.1 validation |
| Missing paymentId | Reject safely | PASSED in V9.1 validation |
| Public verify missing receiptId | Reject safely | PASSED in V9.1 validation |
| Public verify missing evidenceId | Reject safely | PASSED in V9.1 validation |
| Public verify unknown pair | Do not return active verified trust | PASSED in V9.1 validation |
| Selected response leak scan | No obvious secret/internal leak terms | PASSED in V9.1 validation |
| Controlled payment happy path | Backend-verified payment state, receipt/evidence, public verify, access unlock | PASSED in controlled Pi Testnet flow |
| Duplicate approve | Idempotent response; no duplicate state corruption | Open V9.1 target |
| Duplicate complete | Idempotent response; no duplicate receipt/evidence | Open V9.1 target |
| Duplicate receipt/evidence request | Return stable existing state or reject safely | Open V9.1 target |
| paymentId replay | Reject or return safe already-finalized state | Open V9.1 target |
| txid replay | Reject or return safe already-finalized state | Open V9.1 target |
| paymentId mismatch | Reject; no receipt/evidence/access unlock | Open V9.1 target |
| txid mismatch | Reject; no receipt/evidence/access unlock | Open V9.1 target |
| Public verify mismatched pair | Reject or return not verified | Open V9.1 target |
| Pi API timeout | Fail secure; no verified state | Open V9.1 target |
| Supabase write failure | Fail secure; no ambiguous access unlock | Open V9.1 target |
| Partial receipt/evidence write failure | Fail secure; no active public trust from partial state | Open V9.1 target |
| Access unlock write failure | Keep state safe; do not misreport active trust | Open V9.1 target |
| Internal backend error | Safe error; no secrets or stack trace | Open V9.1 target |
| Rate-limit abuse | Throttle or reject safely | Future target |

---

## Endpoint Behavior Expectations

### `/api/health`

Expected behavior:

- return basic ok status
- expose no secrets
- expose no service role information
- expose no raw environment data
- expose no internal stack trace

Current evidence:

- Passed selected V9.1 validation
- Selected leak scan passed

---

### `/api/v9-invoice-create`

Expected behavior:

- create invoice
- return invoice identifier
- set payment state to invoice-created / not verified
- keep access locked
- do not create receipt
- do not create evidence
- expose no sensitive data

Current evidence:

- Passed selected V9.1 validation
- Access locked after invoice create
- No receipt/evidence at invoice stage
- Selected leak scan passed

---

### `/api/v9-receipt-evidence`

Expected behavior:

- reject if invoiceId is missing
- reject if paymentId is missing
- reject if payment is not backend verified
- reject if paymentId does not belong to invoice
- reject if paymentId is replayed
- reject if txid is reused
- prevent duplicate receipt/evidence creation
- create receipt/evidence only after verified payment state
- expose no sensitive data

Current evidence:

- Missing invoiceId rejected in V9.1 validation
- Missing paymentId rejected in V9.1 validation
- Receipt/evidence before verified payment rejected in V9.1 validation

Still open:

- real duplicate receipt/evidence test
- real paymentId replay test
- real txid replay test
- mismatch tests

---

### `/api/v9-public-verify`

Expected behavior:

- reject missing receiptId
- reject missing evidenceId
- reject unknown pair
- reject mismatched pair
- avoid raw payment identifiers
- avoid wallet identifiers
- avoid secrets
- return minimum-disclosure status
- eventually support freshness / stale-state indicators

Current evidence:

- Missing receiptId rejected in V9.1 validation
- Missing evidenceId rejected in V9.1 validation
- Unknown pair did not return active verified trust in V9.1 validation
- Selected leak scan passed

Still open:

- mismatched real receipt/evidence pair test
- freshness / expired / revoked / disputed state behavior

---

## State Transition Matrix

| From State | Event | Expected Result | Status |
|---|---|---|---|
| none | create invoice | INVOICE_CREATED + access locked | PASSED selected validation |
| INVOICE_CREATED | frontend callback only | no access unlock | Must preserve |
| INVOICE_CREATED | receipt/evidence request before verified payment | reject safely | PASSED selected validation |
| INVOICE_CREATED | missing paymentId | reject safely | PASSED selected validation |
| INVOICE_CREATED | backend verified payment | PAYMENT_VERIFIED_TESTNET | PASSED controlled flow |
| PAYMENT_VERIFIED_TESTNET | receipt/evidence request | receipt/evidence created | PASSED controlled flow |
| RECEIPT_EVIDENCE_CREATED | public verify | minimum-disclosure verified result | PASSED controlled flow |
| PUBLIC_VERIFY_AVAILABLE | access unlock | ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | PASSED controlled flow |
| PAYMENT_PENDING | timeout | fail secure / pending / expired | Open target |
| PAYMENT_FAILED | access unlock attempt | reject safely | Open target |
| EXPIRED_INVOICE | payment completion | reject or review-required | Open target |
| VERIFIED_PAYMENT | duplicate complete | idempotent stable response | Open target |
| FINALIZED_PAYMENT | replay paymentId | reject or stable already-finalized state | Open target |
| FINALIZED_PAYMENT | replay txid | reject or stable already-finalized state | Open target |
| RECEIPT_EVIDENCE_CREATED | duplicate receipt/evidence request | no uncontrolled duplicate | Open target |
| PUBLIC_VERIFY_AVAILABLE | mismatched pair | not verified / reject | Open target |
| ANY | backend internal error | safe error, no secret leak | Open target |

---

## Safe Negative Cases Already Passed

The V9.1 safe negative backend validation confirmed:

- invoice creation does not unlock access
- invoice creation does not create receipt/evidence
- receipt/evidence before verified payment is rejected safely
- missing invoiceId is rejected safely
- missing paymentId is rejected safely
- public verify missing inputs are rejected safely
- public verify unknown pair does not return active verified trust
- selected responses do not show obvious secret/internal leak terms

This is meaningful evidence.

It is not complete backend hardening.

---

## Open Duplicate / Replay Cases

The following remain open V9.1 targets:

- duplicate approve with real paymentId
- duplicate complete with real paymentId and txid
- duplicate receipt/evidence request
- same paymentId reused across invoices
- same txid reused across receipts
- stale paymentId used after invoice expiry
- public verify using mismatched real receipt/evidence pair
- replay attempt after access already unlocked

Expected behavior:

- no duplicate receipt
- no duplicate evidence
- no invalid access unlock
- no state corruption
- no raw sensitive data exposure
- stable idempotent response or safe rejection

---

## Open Failure Cases

The following remain open V9.1 targets:

- Pi API timeout
- Pi API ambiguous response
- Supabase read failure
- Supabase write failure
- receipt write succeeds but evidence write fails
- evidence write succeeds but access unlock fails
- public verify reads stale state
- internal backend exception
- rate-limit pressure
- automated abuse probing

Expected behavior:

- fail secure
- do not unlock access from ambiguous state
- do not create active public trust from partial state
- do not leak internal error details
- preserve auditability where possible

---

## Safe Error Response Rules

SafeGate backend responses should not expose:

- stack traces
- service role key
- environment variables
- database URL
- raw Supabase error internals
- raw Pi API response
- access token
- bearer token
- wallet address
- recipient address
- private customer data
- internal table details

V9.1 selected leak scans passed.

Deeper safe error testing remains open.

---

## Public Verify Minimum Disclosure Rules

Public verify should not expose:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- service role key
- raw Pi API response
- private customer data
- private wallet data

Public verify may expose safe outcome fields such as:

- verified
- publicSafe
- minimumDisclosure
- accessState
- receiptStatus
- evidenceStatus
- privateDataIncluded
- secretsIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded

---

## Review Priority

Highest-priority backend review questions:

1. Can access unlock without backend-verified payment?
2. Can receipt/evidence be created before verified payment?
3. Are duplicate approve and complete idempotent?
4. Can paymentId be replayed?
5. Can txid be replayed?
6. Can a paymentId from one invoice affect another invoice?
7. Can public verify return verified for mismatched pair?
8. What happens if Pi API times out?
9. What happens if Supabase write fails?
10. Can error responses leak secrets?

---

## Safe Public Language

SafeGate may say:

- backend behavior matrix is available
- selected safe negative backend behavior passed V9.1 validation
- invoice creation kept access locked
- receipt/evidence before verified payment was rejected safely
- public verify unknown pair did not return active verified trust
- selected responses did not show obvious secret/internal leak terms
- duplicate/replay/failure cases remain V9.1 targets

SafeGate should not say:

- all backend behavior is proven
- backend is production-ready
- all duplicate/replay risks are solved
- all failure cases are solved
- formal audit passed
- tamper-proof backend confirmed

---

## Final Matrix Statement

SafeGate has passed selected public-safe backend behavior checks.

SafeGate still needs deeper duplicate, replay, mismatch, timeout, durable-state failure, freshness, integrity, and rate-limit validation.

This matrix exists to keep the next engineering tests clear and reviewable.

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
