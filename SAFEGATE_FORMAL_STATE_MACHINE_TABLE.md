# SafeGate Formal State Machine Table

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document defines SafeGate’s formal state machine direction.

The purpose is to make legal and illegal transitions clear across invoice, payment, receipt, evidence, public verify, and access states.

This is not a production-readiness claim.

This is not a formal audit.

This is not a completed production state-machine certification.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Formal State Machine | Documented direction |
| Pilot Readiness | Open |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Security Certification | Not claimed |

---

## Completed Evidence Context

SafeGate has passed two important controlled evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

These milestones show:

- controlled backend-verified payment-state direction
- receipt/evidence generation after verified payment state
- access unlock after backend-verified receipt
- invoice creation keeps access locked
- invoice creation does not create receipt/evidence
- receipt/evidence before verified payment is rejected safely
- missing invoiceId and paymentId are rejected safely
- public verify unknown pair does not return active verified trust

They do not prove every state transition.

They do not prove production readiness.

---

## Core State Machine Rule

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Unknown, missing, ambiguous, duplicate, replayed, mismatched, expired, failed, or unavailable states should fail secure.

---

## Controlled Pi Testnet Payment Spine — PASSED

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

This validation does not prove every duplicate, replay, mismatch, timeout, durable-state failure, freshness, or integrity transition.

---

## State Domains

SafeGate state should be separated into domains:

- invoice state
- payment state
- receipt state
- evidence state
- public verify state
- access state
- freshness state
- integrity state
- error / review state

This separation helps prevent one weak signal from incorrectly unlocking another state.

---

## Invoice States

| State | Meaning | Notes |
|---|---|---|
| INVOICE_CREATED | Invoice exists | Access must remain locked |
| INVOICE_PENDING_PAYMENT | Invoice awaits payment | No receipt/evidence yet |
| INVOICE_EXPIRED | Invoice is no longer eligible | Should not unlock access |
| INVOICE_CANCELLED | Invoice cancelled | Should not unlock access |
| INVOICE_REVIEW_REQUIRED | Ambiguous invoice state | Fail secure |

---

## Payment States

| State | Meaning | Notes |
|---|---|---|
| PAYMENT_NOT_VERIFIED | Payment not verified | Default after invoice |
| PAYMENT_PENDING | Payment flow started but not finalized | No access unlock |
| PAYMENT_VERIFIED_TESTNET | Backend-verified Testnet/Sandbox payment state | Controlled V9 pass reached this |
| PAYMENT_FAILED | Payment failed | No receipt/evidence/access unlock |
| PAYMENT_EXPIRED | Payment expired | No access unlock |
| PAYMENT_AMBIGUOUS | Backend cannot confirm | Fail secure |
| PAYMENT_REPLAY_SUSPECTED | Replay suspected | No new receipt/evidence |
| PAYMENT_MISMATCH | Payment does not match invoice | Reject safely |
| PAYMENT_REVIEW_REQUIRED | Manual review needed | No automatic access unlock |

---

## Receipt States

| State | Meaning | Notes |
|---|---|---|
| RECEIPT_NOT_CREATED | No receipt exists | Expected before verified payment |
| RECEIPT_CREATED | Receipt created after verified payment | Must bind to invoice/payment state |
| RECEIPT_DUPLICATE_BLOCKED | Duplicate receipt prevented | Future idempotency target |
| RECEIPT_INVALID | Receipt invalid | Public verify should not show active trust |
| RECEIPT_REVOKED | Receipt revoked | Not active |
| RECEIPT_REVIEW_REQUIRED | Receipt requires review | No clean active trust |

---

## Evidence States

| State | Meaning | Notes |
|---|---|---|
| EVIDENCE_NOT_CREATED | No evidence exists | Expected before verified payment |
| EVIDENCE_CREATED | Evidence created after verified payment | Must bind to receipt |
| EVIDENCE_DUPLICATE_BLOCKED | Duplicate evidence prevented | Future idempotency target |
| EVIDENCE_INVALID | Evidence invalid | Public verify should not show active trust |
| EVIDENCE_REVOKED | Evidence revoked | Not active |
| EVIDENCE_REVIEW_REQUIRED | Evidence requires review | No clean active trust |

---

## Public Verify States

| State | Meaning | Notes |
|---|---|---|
| PUBLIC_VERIFY_NOT_AVAILABLE | No public verify record | Expected before receipt/evidence |
| PUBLIC_VERIFY_AVAILABLE | Public verify exists | Must remain minimum-disclosure |
| PUBLIC_VERIFY_VERIFIED | Public verify confirms valid trust record | Should not expose sensitive identifiers |
| PUBLIC_VERIFY_NOT_FOUND | Pair not found | Must not show active trust |
| PUBLIC_VERIFY_MISSING_INPUT | Missing input | Reject safely |
| PUBLIC_VERIFY_MISMATCH | Receipt/evidence mismatch | Reject or review-required |
| PUBLIC_VERIFY_STALE | Record may be outdated | Future freshness target |
| PUBLIC_VERIFY_REVOKED | Record revoked | Not active |
| PUBLIC_VERIFY_DISPUTED | Record disputed | Review-required |
| PUBLIC_VERIFY_UNAVAILABLE | Backend unavailable | Fail secure |
| PUBLIC_VERIFY_REVIEW_REQUIRED | Ambiguous public verify state | No clean active trust |

---

## Access States

| State | Meaning | Notes |
|---|---|---|
| LOCKED_UNTIL_BACKEND_VERIFIED_PAYMENT | Access locked until backend verification | Passed V9.1 invoice validation |
| ACCESS_LOCKED | Access locked | Default safe state |
| ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Access unlocked after verified receipt | Passed controlled V9 flow |
| ACCESS_EXPIRED | Access expired | Not active |
| ACCESS_REVOKED | Access revoked | Not active |
| ACCESS_DISPUTED | Access disputed | Review-required |
| ACCESS_REVIEW_REQUIRED | Ambiguous state | No automatic active trust |
| ACCESS_UNAVAILABLE | Cannot determine access state | Fail secure |

---

## Freshness States

| State | Meaning | Notes |
|---|---|---|
| FRESHNESS_NOT_IMPLEMENTED | Freshness not fully implemented | Current boundary |
| ACTIVE | Record currently active | Future target |
| EXPIRED | Record expired | Future target |
| REVOKED | Record revoked | Future target |
| DISPUTED | Record disputed | Future target |
| STALE | Record may be outdated | Future target |
| UNAVAILABLE | Freshness cannot be determined | Fail secure |
| REVIEW_REQUIRED | Ambiguous freshness | No clean active trust |

---

## Integrity States

| State | Meaning | Notes |
|---|---|---|
| INTEGRITY_NOT_IMPLEMENTED | Integrity not fully implemented | Current boundary |
| INTEGRITY_VALID | Hash/signature valid | Future target |
| INTEGRITY_MISMATCH | Payload mismatch | No active trust |
| INTEGRITY_UNAVAILABLE | Cannot verify integrity | Fail secure |
| INTEGRITY_REVIEW_REQUIRED | Ambiguous integrity | No clean active trust |

---

## Legal Transition Table

| From | Event | To | Status |
|---|---|---|---|
| none | create invoice | INVOICE_CREATED + LOCKED_UNTIL_BACKEND_VERIFIED_PAYMENT | Passed selected V9.1 validation |
| INVOICE_CREATED | start payment | PAYMENT_PENDING | Expected |
| PAYMENT_PENDING | backend verifies payment | PAYMENT_VERIFIED_TESTNET | Passed controlled V9 flow |
| PAYMENT_VERIFIED_TESTNET | create receipt/evidence | RECEIPT_CREATED + EVIDENCE_CREATED | Passed controlled V9 flow |
| RECEIPT_CREATED + EVIDENCE_CREATED | enable public verify | PUBLIC_VERIFY_AVAILABLE | Passed controlled V9 flow |
| PUBLIC_VERIFY_AVAILABLE | verified minimum-disclosure check | PUBLIC_VERIFY_VERIFIED | Passed controlled V9 flow |
| PUBLIC_VERIFY_VERIFIED | unlock access | ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Passed controlled V9 flow |
| ANY ambiguous state | review needed | REVIEW_REQUIRED | Expected |
| ANY unavailable state | backend unavailable | UNAVAILABLE / fail secure | Future target |

---

## Illegal Transition Table

| Illegal Transition | Expected Behavior | Evidence Status |
|---|---|---|
| INVOICE_CREATED → ACCESS_UNLOCKED | Reject / keep locked | Passed selected V9.1 validation |
| INVOICE_CREATED → RECEIPT_CREATED | Reject | Passed selected V9.1 validation |
| INVOICE_CREATED → EVIDENCE_CREATED | Reject | Passed selected V9.1 validation |
| FRONTEND_CALLBACK → ACCESS_UNLOCKED | Reject / require backend verification | Must preserve |
| PAYMENT_PENDING → ACCESS_UNLOCKED | Reject | Open target |
| PAYMENT_PENDING → RECEIPT_CREATED | Reject | V9.1 selected validation supports direction |
| PAYMENT_FAILED → ACCESS_UNLOCKED | Reject | Open target |
| PAYMENT_EXPIRED → ACCESS_UNLOCKED | Reject | Open target |
| MISSING_INVOICE_ID → RECEIPT_CREATED | Reject | Passed selected V9.1 validation |
| MISSING_PAYMENT_ID → RECEIPT_CREATED | Reject | Passed selected V9.1 validation |
| UNKNOWN_PUBLIC_VERIFY_PAIR → PUBLIC_VERIFY_VERIFIED | Reject / not verified | Passed selected V9.1 validation |
| MISMATCHED_PUBLIC_VERIFY_PAIR → PUBLIC_VERIFY_VERIFIED | Reject / review-required | Open target |
| REPLAYED_PAYMENT_ID → NEW_RECEIPT | Reject or idempotent stable response | Open target |
| REPLAYED_TXID → NEW_EVIDENCE | Reject or idempotent stable response | Open target |
| DUPLICATE_COMPLETE → DUPLICATE_RECEIPT | Block / idempotent | Open target |
| BACKEND_UNAVAILABLE → ACCESS_UNLOCKED | Reject / fail secure | Open target |

---

## V9.1 Validated Transitions

The first V9.1 live validation confirmed selected transitions:

- invoice creation creates invoice-like state
- invoice creation keeps access locked
- invoice creation does not create receipt/evidence
- receipt/evidence request before verified payment is rejected
- missing invoiceId is rejected
- missing paymentId is rejected
- missing public verify inputs are rejected
- unknown public verify pair does not return active verified trust
- selected responses do not show obvious secret/internal leak terms

This is meaningful state-machine evidence.

It is not complete state-machine proof.

---

## Open State-Machine Targets

Open targets:

- duplicate approve idempotency
- duplicate complete idempotency
- duplicate receipt/evidence request behavior
- real paymentId replay behavior
- real txid replay behavior
- paymentId mismatch behavior
- txid mismatch behavior
- public verify mismatched pair behavior
- expired invoice behavior
- failed payment behavior
- Pi API timeout behavior
- Supabase failure behavior
- partial receipt/evidence write failure
- access unlock write failure
- public verify stale-state behavior
- freshness state implementation
- integrity state implementation

---

## State-Machine Safety Rules

SafeGate should preserve these safety rules:

1. Default state is locked.
2. Missing input fails safe.
3. Unknown pair fails safe.
4. Ambiguous backend state fails safe.
5. Frontend callback alone never unlocks access.
6. Receipt/evidence requires backend-verified payment.
7. Access unlock requires backend-verified receipt.
8. Public verify remains minimum-disclosure.
9. Duplicate callbacks should be idempotent.
10. Replay attempts should not create new trust outcomes.
11. Stale public verify should not imply active trust.
12. Integrity mismatch should not imply active trust.

---

## Reviewer Questions

A reviewer should ask:

1. What state unlocks access?
2. Can any frontend-only state unlock access?
3. Can receipt/evidence exist before backend verification?
4. Can unknown public verify pair return verified?
5. Are duplicate callbacks idempotent?
6. Are replayed identifiers rejected?
7. What happens during timeout?
8. What happens during database failure?
9. What happens when public verify is stale?
10. What happens when integrity fails?

---

## Safe Public Language

SafeGate may say:

- formal state-machine direction is documented
- selected V9.1 negative transitions passed validation
- invoice creation kept access locked
- receipt/evidence before verified payment was rejected safely
- public verify unknown pair did not return active verified trust
- production readiness is not claimed
- formal audit is not claimed

SafeGate should not say:

- complete state machine is formally verified
- all transitions are production-proven
- all duplicate/replay risks are solved
- formal audit passed
- tamper-proof system is complete
- production-ready state machine is complete

---

## Final State Machine Position

SafeGate has a documented formal state-machine direction.

SafeGate has passed selected state-machine-related negative checks in V9.1.

SafeGate still needs deeper duplicate, replay, mismatch, timeout, durable-state failure, freshness, and integrity transition validation.

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
