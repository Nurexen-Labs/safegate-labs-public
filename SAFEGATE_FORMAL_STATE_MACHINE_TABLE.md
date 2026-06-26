# SafeGate Formal State Machine Table

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Document Status

**Status:** Formal state machine planning table  
**Scope:** V9.1 backend + integrity hardening  
**Current Public Phase:** Pilot Readiness  
**Controlled Pi Testnet Payment Spine:** PASSED  
**Production Claim:** No  
**Security Certification Claim:** No  
**Third-Party Audit Claim:** No  
**Formal Verification Claim:** No  

---

## Important Boundary

This document defines the intended SafeGate payment/trust state machine.

It is not a production-readiness claim.

It is not a formal mathematical verification.

It is not a security audit.

It does not claim that every transition is already fully implemented, tested, or hardened.

Its purpose is to make legal and illegal payment/trust transitions explicit before controlled pilot execution.

---

## Why This Document Exists

Technical reviewers asked:

- What states can SafeGate enter?
- Which transitions are legal?
- Which transitions are illegal?
- What happens during timeout, duplicate callback, replay, failure, or ambiguity?
- Can access unlock before backend verification?
- Can receipt/evidence be created before verified payment?
- What happens when state is stale, expired, revoked, disputed, or ambiguous?

This document answers those questions in a public-safe planning format.

---

## Core State Machine Rule

SafeGate must follow this rule:

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

If state is missing, ambiguous, mismatched, duplicated, stale, expired, or not durably verified, SafeGate must fail secure.

---

## Current Confirmed Foundation

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

# 1. State Categories

SafeGate state should be separated into several categories.

| Category | Purpose |
|---|---|
| Invoice State | Tracks invoice / payment intent lifecycle |
| Payment State | Tracks Pi payment lifecycle |
| Receipt State | Tracks receipt lifecycle |
| Evidence State | Tracks evidence lifecycle |
| Access State | Tracks whether access is locked, unlocked, expired, revoked, or disputed |
| Public Verify State | Tracks public-safe verification result |
| Failure / Review State | Tracks timeout, ambiguity, mismatch, replay, or failure |

---

# 2. Invoice States

| State | Meaning | Public-Safe Meaning |
|---|---|---|
| INVOICE_CREATED | Invoice/payment intent created | Invoice exists, access still locked |
| INVOICE_CANCELLED | Invoice cancelled before payment verification | No payment trust result |
| INVOICE_EXPIRED | Invoice expired before payment verification | No access unlock |
| INVOICE_BOUND_TO_PAYMENT | Invoice linked to a paymentId | Payment lifecycle has started |
| INVOICE_REVIEW_REQUIRED | Invoice state is ambiguous | Manual review needed |

---

# 3. Payment States

| State | Meaning | Access Allowed? |
|---|---|---|
| PAYMENT_NOT_STARTED | No payment attempt yet | No |
| PAYMENT_PENDING | Payment attempt started but not completed | No |
| PI_APPROVED_BY_SERVER | Server-side approve step succeeded | No |
| COMPLETION_PENDING | Complete step is in progress or awaiting confirmation | No |
| PAYMENT_VERIFIED_TESTNET | Payment completed in controlled Testnet/Sandbox context | Not by itself |
| PAYMENT_VERIFIED_PI | Future Pi verified state if separately implemented | Not by itself |
| PAYMENT_FAILED | Payment failed | No |
| PAYMENT_EXPIRED | Payment expired | No |
| PAYMENT_CANCELLED | Payment was cancelled | No |
| COMPLETION_TIMEOUT | Complete step timed out | No |
| PAYMENT_VERIFICATION_AMBIGUOUS | Verification cannot be safely determined | No |
| PAYMENT_ID_MISMATCH | paymentId does not match invoice | No |
| TXID_REPLAY_REJECTED | txid replay attempt rejected | No |
| PAYMENT_REPLAY_REJECTED | paymentId replay attempt rejected | No |
| MANUAL_REVIEW_REQUIRED | Human/operator review needed | No automatic unlock |

---

# 4. Receipt States

| State | Meaning | Public-Safe Meaning |
|---|---|---|
| RECEIPT_NOT_CREATED | No receipt exists | No receipt proof |
| RECEIPT_CREATED | Receipt created after verified payment | Receipt exists |
| RECEIPT_ACTIVE | Receipt currently active | Active receipt |
| RECEIPT_EXPIRED | Receipt expired if expiry exists | Historical or inactive receipt |
| RECEIPT_REVOKED | Receipt revoked | No active trust |
| RECEIPT_DISPUTED | Receipt under dispute | Review/dispute status |
| RECEIPT_SUPERSEDED | Receipt replaced by newer record | Old record no longer active |
| RECEIPT_INTEGRITY_FAILED | Receipt integrity check failed | Do not trust active status |
| RECEIPT_REVIEW_REQUIRED | Receipt needs review | Not active trust |

---

# 5. Evidence States

| State | Meaning | Public-Safe Meaning |
|---|---|---|
| EVIDENCE_NOT_CREATED | No evidence exists | No evidence proof |
| EVIDENCE_CREATED | Evidence created after verified payment | Evidence exists |
| EVIDENCE_ACTIVE | Evidence currently active | Active evidence |
| EVIDENCE_EXPIRED | Evidence expired if expiry exists | Inactive evidence |
| EVIDENCE_REVOKED | Evidence revoked | No active trust |
| EVIDENCE_DISPUTED | Evidence under dispute | Review/dispute status |
| EVIDENCE_INTEGRITY_FAILED | Evidence integrity check failed | Do not trust active status |
| EVIDENCE_REVIEW_REQUIRED | Evidence needs review | Not active trust |

---

# 6. Access States

| State | Meaning |
|---|---|
| ACCESS_LOCKED | Access is locked |
| LOCKED_UNTIL_BACKEND_VERIFIED_PAYMENT | Access locked until backend payment verification |
| LOCKED_UNTIL_BACKEND_VERIFIED_RECEIPT | Access locked until receipt/evidence guard passes |
| ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Access unlocked after backend-verified receipt/evidence |
| ACCESS_EXPIRED | Access was active but expired |
| ACCESS_REVOKED | Access was revoked |
| ACCESS_DISPUTED | Access blocked or flagged due to dispute |
| ACCESS_REVIEW_REQUIRED | Access status requires review |
| ACCESS_UNKNOWN | Access status cannot be safely determined |

---

# 7. Public Verify States

| State | Meaning |
|---|---|
| PUBLIC_VERIFY_NOT_FOUND | Receipt/evidence pair not found |
| PUBLIC_VERIFY_MISMATCH | Receipt/evidence pair mismatch |
| PUBLIC_VERIFY_ACTIVE | Public verify confirms active public-safe trust state |
| PUBLIC_VERIFY_EXPIRED | Public verify confirms expired status |
| PUBLIC_VERIFY_REVOKED | Public verify confirms revoked status |
| PUBLIC_VERIFY_DISPUTED | Public verify confirms disputed status |
| PUBLIC_VERIFY_INTEGRITY_FAILED | Public verify detects integrity failure |
| PUBLIC_VERIFY_UNAVAILABLE | Public verify cannot read durable state |
| PUBLIC_VERIFY_REVIEW_REQUIRED | Public verify cannot safely assert active trust |

---

# 8. Legal Transition Table

| From | To | Required Condition | Current Status |
|---|---|---|---|
| none | INVOICE_CREATED | Valid invoice create request | PASSED_CONTROLLED |
| INVOICE_CREATED | PAYMENT_PENDING | Payment flow starts | PARTIAL |
| PAYMENT_PENDING | PI_APPROVED_BY_SERVER | Backend approve succeeds | PASSED_CONTROLLED |
| PI_APPROVED_BY_SERVER | COMPLETION_PENDING | Backend complete starts | PARTIAL |
| COMPLETION_PENDING | PAYMENT_VERIFIED_TESTNET | Backend complete succeeds in controlled Testnet/Sandbox context | PASSED_CONTROLLED |
| PAYMENT_VERIFIED_TESTNET | RECEIPT_CREATED | Receipt guard confirms verified payment state | PASSED_CONTROLLED |
| RECEIPT_CREATED | EVIDENCE_CREATED | Evidence generated for same verified record | PASSED_CONTROLLED |
| EVIDENCE_CREATED | ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Receipt/evidence pair complete and valid | PASSED_CONTROLLED |
| ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | PUBLIC_VERIFY_ACTIVE | Public verify confirms minimum-disclosure record | PASSED_CONTROLLED |
| PAYMENT_PENDING | PAYMENT_EXPIRED | Timeout reached | PLANNED_HARDENING |
| PI_APPROVED_BY_SERVER | COMPLETION_TIMEOUT | Complete step times out | PLANNED_HARDENING |
| COMPLETION_TIMEOUT | MANUAL_REVIEW_REQUIRED | Ambiguity cannot be resolved automatically | PLANNED_HARDENING |
| PAYMENT_VERIFIED_TESTNET | RECEIPT_REVIEW_REQUIRED | Receipt creation fails after verified payment | PLANNED_HARDENING |
| RECEIPT_CREATED | EVIDENCE_REVIEW_REQUIRED | Evidence creation fails after receipt | PLANNED_HARDENING |
| RECEIPT_ACTIVE | RECEIPT_EXPIRED | Expiry policy reached | PLANNED_HARDENING |
| RECEIPT_ACTIVE | RECEIPT_REVOKED | Revocation policy triggered | PLANNED_HARDENING |
| RECEIPT_ACTIVE | RECEIPT_DISPUTED | Dispute policy triggered | PLANNED_HARDENING |
| ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | ACCESS_EXPIRED | Access expiry reached | PLANNED_HARDENING |
| ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | ACCESS_REVOKED | Revocation policy triggered | PLANNED_HARDENING |
| ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | ACCESS_DISPUTED | Dispute policy triggered | PLANNED_HARDENING |

---

# 9. Illegal Transition Table

| Illegal Transition | Expected Behavior | Current Status |
|---|---|---|
| INVOICE_CREATED → ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Reject | NEEDS_BACKEND_EVIDENCE |
| FRONTEND_CALLBACK_ONLY → ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Reject | PASSED_CONTROLLED_BY_FLOW |
| PAYMENT_PENDING → RECEIPT_CREATED | Reject | PASSED_LOCAL_API |
| PAYMENT_PENDING → EVIDENCE_CREATED | Reject | PASSED_LOCAL_API |
| PAYMENT_PENDING → PUBLIC_VERIFY_ACTIVE | Reject | NEEDS_LIVE_REPEAT |
| PI_APPROVED_BY_SERVER → ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Reject | NEEDS_LIVE_REPEAT |
| PAYMENT_FAILED → RECEIPT_CREATED | Reject | PLANNED_HARDENING |
| PAYMENT_FAILED → ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Reject | PLANNED_HARDENING |
| PAYMENT_EXPIRED → ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Reject | PLANNED_HARDENING |
| PAYMENT_ID_MISMATCH → PAYMENT_VERIFIED_TESTNET | Reject | NEEDS_LIVE_REPEAT |
| TXID_REPLAY_REJECTED → PAYMENT_VERIFIED_TESTNET | Reject | PLANNED_HARDENING |
| PAYMENT_REPLAY_REJECTED → RECEIPT_CREATED | Reject | PLANNED_HARDENING |
| RECEIPT_INTEGRITY_FAILED → PUBLIC_VERIFY_ACTIVE | Reject | PLANNED_HARDENING |
| RECEIPT_REVOKED → ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Reject | PLANNED_HARDENING |
| RECEIPT_DISPUTED → ACTIVE_ACCESS_WITHOUT_REVIEW | Reject | PLANNED_HARDENING |
| PUBLIC_VERIFY_MISMATCH → verified true | Reject | NEEDS_LIVE_REPEAT |
| PUBLIC_VERIFY_NOT_FOUND → verified true | Reject | NEEDS_LIVE_REPEAT |

---

# 10. Terminal and Non-Terminal States

## Terminal Safe States

These states should not automatically transition to active access:

- PAYMENT_FAILED
- PAYMENT_CANCELLED
- PAYMENT_EXPIRED
- PAYMENT_REPLAY_REJECTED
- TXID_REPLAY_REJECTED
- RECEIPT_REVOKED
- RECEIPT_INTEGRITY_FAILED
- ACCESS_REVOKED

## Non-Terminal Review States

These states may require retry, reconciliation, or manual review:

- COMPLETION_TIMEOUT
- PAYMENT_VERIFICATION_AMBIGUOUS
- MANUAL_REVIEW_REQUIRED
- RECEIPT_REVIEW_REQUIRED
- EVIDENCE_REVIEW_REQUIRED
- PUBLIC_VERIFY_REVIEW_REQUIRED
- ACCESS_REVIEW_REQUIRED

---

# 11. Duplicate Callback Behavior

Duplicate callback handling should follow idempotency rules.

| Duplicate Scenario | Expected Behavior |
|---|---|
| duplicate approve | Return existing approve/verified state; no corruption |
| duplicate complete | Return existing verified state; no duplicate receipt/evidence |
| duplicate receipt/evidence request | Return existing receipt/evidence pair |
| duplicate public verify | Return same minimum-disclosure result |
| duplicate paymentId on same invoice | Idempotent safe response |
| duplicate paymentId on another invoice | Reject as replay/mismatch |
| duplicate txid on another invoice | Reject as replay/mismatch |

---

# 12. Replay Behavior

Replay attempts must not create new trust states.

| Replay Scenario | Expected Behavior |
|---|---|
| old paymentId used for new invoice | Reject |
| old txid used for new invoice | Reject |
| old receipt used for unrelated access | Reject |
| receipt/evidence pair mixed with another record | Public verify mismatch |
| expired receipt reused | Expired status or reject |
| revoked receipt reused | Revoked status or reject |
| disputed receipt reused | Disputed/review status |

---

# 13. Timeout Behavior

Timeouts should fail secure.

| Timeout Scenario | Expected Behavior |
|---|---|
| user starts payment but does not complete | PAYMENT_EXPIRED or PAYMENT_PENDING until expiry |
| Pi API approve timeout | No approve success; safe ambiguity/failure |
| Pi API complete timeout | COMPLETION_TIMEOUT or PAYMENT_VERIFICATION_AMBIGUOUS |
| backend complete succeeds but client disconnects | durable state determines truth |
| pending state too old | expire or manual review |
| public verify read timeout | PUBLIC_VERIFY_UNAVAILABLE, no fake verified true |

---

# 14. Durable State Failure Behavior

Durable state is the source of truth for SafeGate trust records.

| Failure Scenario | Expected Behavior |
|---|---|
| DB read fails before verification | Reject safely |
| DB write fails during approve | No fake server approval |
| DB write fails during complete | No fake verified state unless persisted |
| payment verified but durable write fails | Reconciliation required; no automatic unlock |
| receipt creation fails | No active access unless receipt/evidence pair is complete |
| evidence creation fails | No active public verify if pair incomplete |
| public verify DB read fails | No fake verified true |

---

# 15. Access Unlock Conditions

Access may unlock only if all required conditions are true:

- invoice exists
- payment state is backend verified
- paymentId matches invoice
- replay/mismatch checks pass
- receipt exists
- evidence exists
- receipt/evidence pair is correctly bound
- durable state is available
- public-safe unlock condition passes
- receipt/access is not expired, revoked, disputed, or integrity-failed

If any condition fails, access must remain locked or move to review state.

---

# 16. Public Verify Conditions

Public verify may show active trust only if:

- receipt/evidence pair exists
- receipt/evidence pair matches
- payment state is verified
- access state is valid
- receipt/evidence is not revoked
- receipt/evidence is not expired if expiry applies
- receipt/evidence is not disputed
- integrity check passes if implemented
- freshness status is acceptable
- minimum-disclosure rules are preserved

---

# 17. Safe Error Rules

Errors must be public-safe.

Do not expose:

- stack traces
- Supabase internals
- service role key details
- raw Pi API response
- access tokens
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
- RECEIPT_EVIDENCE_MISMATCH
- PUBLIC_VERIFY_UNAVAILABLE
- DURABLE_STATE_UNAVAILABLE
- INTERNAL_SAFE_FAILURE

---

# 18. State Machine Test Targets

Recommended V9.1 tests:

1. legal happy-path transition test
2. invoice created cannot unlock access
3. frontend callback alone cannot unlock access
4. receipt before verified payment is rejected
5. duplicate approve is idempotent
6. duplicate complete is idempotent
7. duplicate receipt/evidence returns existing pair
8. paymentId mismatch is rejected
9. txid replay is rejected
10. public verify mismatch returns false
11. unknown receipt/evidence returns not found
12. timeout moves to safe state
13. durable write failure does not unlock access
14. receipt integrity failure blocks active verification
15. revoked/disputed receipt does not show active access

---

# 19. Current Safe Language

SafeGate may say:

- formal state machine planning table
- legal/illegal transition model
- backend-verified unlock rule
- fail-secure state direction
- controlled Pi Testnet payment-spine pass
- V9.1 hardening target

SafeGate should not say:

- formally verified state machine
- production-ready state machine
- complete fail-secure proof
- fully secure payment state engine
- audit-passed state machine
- enterprise-grade state infrastructure

---

# 20. Completion Criteria

This formal state machine table can be considered ready for V9.1 planning when:

- invoice states are defined
- payment states are defined
- receipt/evidence states are defined
- access states are defined
- public verify states are defined
- legal transitions are documented
- illegal transitions are documented
- timeout behavior is documented
- duplicate/replay behavior is documented
- durable-state failure behavior is documented
- access unlock conditions are explicit
- public verify active conditions are explicit
- safe language boundaries are preserved

Implementation completion requires separate testing and evidence.

---

## Final State Machine Rule

SafeGate must not treat payment as trust until the backend verifies the payment event, durably records the verified state, creates guarded receipt/evidence, and only then allows access unlock.

That is the SafeGate state machine target.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
