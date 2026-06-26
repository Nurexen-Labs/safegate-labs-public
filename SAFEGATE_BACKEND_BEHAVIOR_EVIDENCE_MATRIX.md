# SafeGate Backend Behavior Evidence Matrix

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Document Status

**Status:** Backend behavior evidence matrix  
**Scope:** V9 / V9.1 payment-spine backend behavior  
**Current Public Phase:** Pilot Readiness  
**Controlled Pi Testnet Payment Spine:** PASSED  
**Production Claim:** No  
**Security Certification Claim:** No  
**Third-Party Audit Claim:** No  

---

## Important Boundary

This document is not a production-readiness claim.

This document is not a formal audit.

This document is not a security certification.

This document does not claim that every edge case is already fully hardened.

Its purpose is to make backend behavior clearer, safer, and more reviewable before controlled pilot execution.

---

## Why This Matrix Exists

Technical reviewers identified one major next question:

Can SafeGate’s backend behavior be externally reasoned about under duplicate callbacks, replay attempts, missing parameters, timeout cases, database failures, and public verify edge cases?

This matrix answers that question in a public-safe format.

It separates:

- what has already been demonstrated
- what is implemented directionally
- what needs deeper live repeat evidence
- what remains planned hardening

---

## Status Labels

| Label | Meaning |
|---|---|
| PASSED_CONTROLLED | Demonstrated in controlled Pi Browser / Testnet-oriented flow |
| PASSED_LOCAL_API | Demonstrated in local or controlled API skeleton test |
| PARTIAL | Direction exists, but deeper backend evidence is needed |
| NEEDS_LIVE_REPEAT | Should be repeated against live backend behavior |
| PLANNED_HARDENING | Not claimed complete; planned for V9.1 hardening |
| NOT_CLAIMED | Not currently claimed |

---

## Core Backend Principle

SafeGate backend behavior must follow this principle:

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

---

## Current Confirmed Payment Spine

SafeGate has demonstrated the following controlled flow:

1. V9 invoice creation
2. initial access state locked
3. Pi Browser authentication
4. username + payments scope
5. Pi wallet payment in Sandbox / Testnet context
6. backend approval / completion flow
7. backend-verified payment state
8. receipt and evidence generation after verified state
9. public verify confirmation
10. access unlock only after backend-verified receipt

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

# Endpoint Behavior Matrix

---

## 1. Invoice Create Endpoint

**Endpoint:** `POST /api/v9-invoice-create`

Purpose:

Create a controlled invoice/payment intent and keep access locked until backend payment verification.

---

### Matrix

| Scenario | Expected Result | Access Result | Receipt/Evidence Result | Public-Safe Response | Current Status |
|---|---|---|---|---|---|
| Valid invoice create | Invoice created | LOCKED_UNTIL_BACKEND_VERIFIED_PAYMENT | No receipt/evidence | Public-safe invoice summary | PASSED_CONTROLLED |
| Invoice create without durable storage | Reject safely | No unlock | No receipt/evidence | DURABLE_STATE_UNAVAILABLE or safe error | PLANNED_HARDENING |
| Duplicate invoice create with new request | New invoice may be created if intentionally requested | Locked | No receipt/evidence | Public-safe invoice summary | PARTIAL |
| Missing required server configuration | Reject safely | No unlock | No receipt/evidence | Safe configuration error, no secrets | PARTIAL |
| Unexpected backend error | Reject safely | No unlock | No receipt/evidence | INTERNAL_SAFE_FAILURE, no stack trace | PLANNED_HARDENING |

---

### Evidence Notes

Confirmed:

- Controlled invoice creation reached `INVOICE_CREATED`.
- Initial access remained locked.
- No receipt/evidence was created at invoice creation.

Needs deeper evidence:

- safe behavior when durable storage is unavailable
- safe behavior when required environment variables are missing
- no internal error leakage under failure

---

## 2. Pi Approve Endpoint

**Endpoint:** `POST /api/v9-pi-approve`

Purpose:

Approve a Pi payment server-side after invoice/payment binding checks.

---

### Matrix

| Scenario | Expected Result | Access Result | Receipt/Evidence Result | Public-Safe Response | Current Status |
|---|---|---|---|---|---|
| Valid approve | Server-approved state recorded | Still locked | No receipt/evidence | PI_APPROVED_BY_SERVER | PASSED_CONTROLLED |
| Missing invoiceId | Reject | No unlock | No receipt/evidence | MISSING_INVOICE_ID | NEEDS_LIVE_REPEAT |
| Missing paymentId | Reject | No unlock | No receipt/evidence | MISSING_PAYMENT_ID | NEEDS_LIVE_REPEAT |
| Unknown invoiceId | Reject | No unlock | No receipt/evidence | INVOICE_NOT_FOUND | NEEDS_LIVE_REPEAT |
| paymentId already bound to same invoice | Return existing state idempotently | Still locked unless already complete | No duplicate receipt/evidence | Existing approve state | NEEDS_LIVE_REPEAT |
| paymentId already bound to another invoice | Reject as mismatch/replay | No unlock | No receipt/evidence | PAYMENT_ID_MISMATCH or REPLAY_REJECTED | PLANNED_HARDENING |
| Pi API approve timeout | Do not fake success | No unlock | No receipt/evidence | PAYMENT_APPROVAL_UNAVAILABLE or AMBIGUOUS | PLANNED_HARDENING |
| Pi API approve error | Do not fake success | No unlock | No receipt/evidence | Safe payment approval error | PLANNED_HARDENING |
| Duplicate approve callback | No duplicate state corruption | No premature unlock | No duplicate receipt/evidence | Existing state returned | NEEDS_LIVE_REPEAT |
| Malformed request body | Reject | No unlock | No receipt/evidence | MALFORMED_REQUEST | PLANNED_HARDENING |

---

### Evidence Notes

Confirmed:

- Controlled payment flow reached server approval direction.
- Receipt/evidence was not created at approval stage alone.

Needs deeper evidence:

- duplicate approve behavior
- paymentId mismatch behavior
- safe Pi API timeout behavior
- safe public error model

---

## 3. Pi Complete Endpoint

**Endpoint:** `POST /api/v9-pi-complete`

Purpose:

Complete the Pi payment server-side and move the record to backend-verified payment state only after completion succeeds.

---

### Matrix

| Scenario | Expected Result | Access Result | Receipt/Evidence Result | Public-Safe Response | Current Status |
|---|---|---|---|---|---|
| Valid complete after approve | PAYMENT_VERIFIED_TESTNET | Still locked until receipt/evidence guard completes | No duplicate receipt/evidence at complete alone | Verified payment state | PASSED_CONTROLLED |
| Missing invoiceId | Reject | No unlock | No receipt/evidence | MISSING_INVOICE_ID | NEEDS_LIVE_REPEAT |
| Missing paymentId | Reject | No unlock | No receipt/evidence | MISSING_PAYMENT_ID | NEEDS_LIVE_REPEAT |
| Missing txid | Reject | No unlock | No receipt/evidence | MISSING_TXID | NEEDS_LIVE_REPEAT |
| Complete before approve | Reject or require approval | No unlock | No receipt/evidence | APPROVAL_REQUIRED | NEEDS_LIVE_REPEAT |
| Unknown invoiceId | Reject | No unlock | No receipt/evidence | INVOICE_NOT_FOUND | NEEDS_LIVE_REPEAT |
| paymentId mismatch | Reject | No unlock | No receipt/evidence | PAYMENT_ID_MISMATCH | NEEDS_LIVE_REPEAT |
| Duplicate complete after verified state | Return existing verified state idempotently | No duplicate unlock | No duplicate receipt/evidence | Existing verified state | NEEDS_LIVE_REPEAT |
| Same txid submitted for another invoice | Reject as replay | No unlock | No receipt/evidence | TXID_REPLAY_REJECTED | PLANNED_HARDENING |
| Pi API complete timeout | Do not fake verification | No unlock | No receipt/evidence | COMPLETION_TIMEOUT or AMBIGUOUS | PLANNED_HARDENING |
| Pi API complete error | Do not fake verification | No unlock | No receipt/evidence | PAYMENT_COMPLETION_FAILED | PLANNED_HARDENING |
| Backend crash during complete | No fake success; reconciliation required | No unlock unless durable verified state exists | No receipt/evidence unless verified state persisted | RECONCILIATION_REQUIRED | PLANNED_HARDENING |

---

### Evidence Notes

Confirmed:

- Controlled Pi Browser flow reached `PAYMENT_VERIFIED_TESTNET`.
- Access unlock happened after receipt/evidence guard, not from complete callback alone.

Needs deeper evidence:

- duplicate complete behavior
- missing txid behavior
- same txid replay behavior
- completion timeout behavior
- durable write failure behavior

---

## 4. Receipt / Evidence Endpoint

**Endpoint:** `POST /api/v9-receipt-evidence`

Purpose:

Create receipt/evidence only after backend-verified payment state.

---

### Matrix

| Scenario | Expected Result | Access Result | Receipt/Evidence Result | Public-Safe Response | Current Status |
|---|---|---|---|---|---|
| Receipt requested before verified payment | Reject | No unlock | No receipt/evidence | PAYMENT_NOT_VERIFIED | PASSED_LOCAL_API |
| Receipt requested after PAYMENT_VERIFIED_TESTNET | Create receipt/evidence | ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Receipt/evidence generated | Public-safe receipt/evidence summary | PASSED_CONTROLLED |
| Missing invoiceId | Reject | No unlock | No receipt/evidence | MISSING_INVOICE_ID | NEEDS_LIVE_REPEAT |
| Missing paymentId | Reject | No unlock | No receipt/evidence | MISSING_PAYMENT_ID | NEEDS_LIVE_REPEAT |
| paymentId mismatch | Reject | No unlock | No receipt/evidence | PAYMENT_ID_MISMATCH | NEEDS_LIVE_REPEAT |
| Duplicate receipt/evidence request | Return existing pair | No duplicate unlock | Existing receipt/evidence returned | Existing public-safe pair | NEEDS_LIVE_REPEAT |
| Receipt generation fails after verified payment | No fake public verify; mark failure/retry | No unlock unless completed | Failure state or retry required | RECEIPT_CREATE_FAILED | PLANNED_HARDENING |
| Evidence generation fails after receipt | No inconsistent public verify | No unlock unless pair is complete | Failure state or retry required | EVIDENCE_CREATE_FAILED | PLANNED_HARDENING |
| Attempt receipt for failed payment | Reject | No unlock | No receipt/evidence | PAYMENT_NOT_VERIFIED | PLANNED_HARDENING |
| Attempt receipt for expired payment | Reject or manual review | No unlock | No receipt/evidence | PAYMENT_EXPIRED | PLANNED_HARDENING |

---

### Evidence Notes

Confirmed:

- Local/skeleton test showed receipt/evidence rejects before verification.
- Controlled Pi Browser flow created receipt/evidence after `PAYMENT_VERIFIED_TESTNET`.
- Access unlock occurred after backend-verified receipt/evidence.

Needs deeper evidence:

- duplicate receipt/evidence request behavior
- receipt create failure behavior
- evidence create failure behavior
- expired/failed state behavior

---

## 5. Public Verify Endpoint

**Endpoint:** `GET /api/v9-public-verify`

Purpose:

Confirm receipt/evidence status publicly with minimum disclosure.

---

### Matrix

| Scenario | Expected Result | Access Result | Receipt/Evidence Result | Public-Safe Response | Current Status |
|---|---|---|---|---|---|
| Valid receipt/evidence pair | verified true | accessUnlocked true if backend-verified unlock exists | Existing pair confirmed | minimumDisclosure true, publicSafe true | PASSED_CONTROLLED |
| Missing receiptId | Reject safely | No change | No new receipt/evidence | MISSING_RECEIPT_ID | PASSED_LOCAL_API |
| Missing evidenceId | Reject safely | No change | No new receipt/evidence | MISSING_EVIDENCE_ID | PASSED_LOCAL_API |
| Unknown receipt/evidence | Not found / verified false | No change | No new receipt/evidence | RECEIPT_EVIDENCE_NOT_FOUND | NEEDS_LIVE_REPEAT |
| Mismatched receipt/evidence pair | verified false | No change | No new receipt/evidence | RECEIPT_EVIDENCE_MISMATCH | NEEDS_LIVE_REPEAT |
| Malformed receipt/evidence ID | Reject safely | No change | No new receipt/evidence | MALFORMED_ID | PLANNED_HARDENING |
| Expired receipt | verified false or expired state | No active unlock | Existing pair marked expired | VERIFIED_EXPIRED | PLANNED_HARDENING |
| Revoked receipt | verified false or revoked state | No active unlock | Existing pair marked revoked | VERIFIED_REVOKED | PLANNED_HARDENING |
| Disputed receipt | disputed public-safe state | Context-dependent | Existing pair marked disputed | VERIFIED_DISPUTED | PLANNED_HARDENING |
| Backend read failure | No fake verified response | No change | No new receipt/evidence | DURABLE_STATE_UNAVAILABLE | PLANNED_HARDENING |

---

### Evidence Notes

Confirmed:

- Public verify confirmed the controlled V9 payment-spine result.
- Minimum-disclosure output did not expose paymentId or txid.
- Public verify public-safe fields were visible.

Needs deeper evidence:

- mismatched pair behavior
- unknown pair behavior
- stale/expired/revoked/disputed state behavior
- backend read failure behavior
- public verify freshness policy

---

## 6. Safe Error Behavior Matrix

SafeGate should return public-safe errors without exposing internals.

---

### Matrix

| Scenario | Expected Result | Sensitive Data Exposure | Current Status |
|---|---|---|---|
| Missing required parameter | 400 safe rejection | No secrets | NEEDS_LIVE_REPEAT |
| Invalid payment state | 409 safe rejection | No secrets | NEEDS_LIVE_REPEAT |
| Payment not verified | 409 safe rejection | No secrets | PASSED_LOCAL_API |
| Unknown receipt/evidence | 404 or verified false | No secrets | NEEDS_LIVE_REPEAT |
| Durable state unavailable | 503 safe rejection | No secrets | PLANNED_HARDENING |
| Pi API timeout | 502/503 safe rejection or ambiguous state | No raw Pi response | PLANNED_HARDENING |
| Internal exception | INTERNAL_SAFE_FAILURE | No stack trace | PLANNED_HARDENING |
| Rate limit exceeded | RATE_LIMITED | No internals | PLANNED_HARDENING |

---

## 7. State Transition Behavior Matrix

SafeGate state transitions must remain strict.

---

### Legal Transition Targets

| From | To | Required Condition | Current Status |
|---|---|---|---|
| INVOICE_CREATED | PAYMENT_PENDING | Payment attempt starts | PARTIAL |
| PAYMENT_PENDING | PI_APPROVED_BY_SERVER | Backend approve succeeds | PASSED_CONTROLLED |
| PI_APPROVED_BY_SERVER | PAYMENT_VERIFIED_TESTNET | Backend complete succeeds | PASSED_CONTROLLED |
| PAYMENT_VERIFIED_TESTNET | RECEIPT_EVIDENCE_CREATED | Receipt/evidence guard passes | PASSED_CONTROLLED |
| RECEIPT_EVIDENCE_CREATED | ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | Backend-verified receipt unlock condition passes | PASSED_CONTROLLED |
| PAYMENT_PENDING | PAYMENT_EXPIRED | Timeout policy reached | PLANNED_HARDENING |
| PI_APPROVED_BY_SERVER | COMPLETION_TIMEOUT | Complete timeout reached | PLANNED_HARDENING |
| Any ambiguous state | MANUAL_REVIEW_REQUIRED | Ambiguity cannot be resolved safely | PLANNED_HARDENING |

---

### Illegal Transition Targets

| Illegal Transition | Expected Behavior | Current Status |
|---|---|---|
| INVOICE_CREATED → ACCESS_UNLOCKED | Reject | PASSED_BY_DESIGN / NEEDS_BACKEND_EVIDENCE |
| FRONTEND_CALLBACK_ONLY → ACCESS_UNLOCKED | Reject | PASSED_CONTROLLED_BY_FLOW |
| PAYMENT_PENDING → RECEIPT_EVIDENCE_CREATED | Reject | PASSED_LOCAL_API |
| PAYMENT_FAILED → ACCESS_UNLOCKED | Reject | PLANNED_HARDENING |
| PAYMENT_EXPIRED → ACCESS_UNLOCKED | Reject | PLANNED_HARDENING |
| PAYMENT_ID_MISMATCH → RECEIPT_EVIDENCE_CREATED | Reject | NEEDS_LIVE_REPEAT |
| MISMATCHED_RECEIPT_EVIDENCE → VERIFIED_TRUE | Reject | NEEDS_LIVE_REPEAT |
| DUPLICATE_PAYMENT_ID_ON_NEW_INVOICE → VERIFIED | Reject | PLANNED_HARDENING |
| DUPLICATE_TXID_ON_NEW_INVOICE → VERIFIED | Reject | PLANNED_HARDENING |

---

## 8. Idempotency Evidence Matrix

Idempotency means repeated valid calls do not corrupt state.

---

| Scenario | Target Behavior | Current Status | Required Next Evidence |
|---|---|---|---|
| duplicate approve, same invoice/paymentId | Return existing approve/verified state | NEEDS_LIVE_REPEAT | Repeat approve call and capture safe result |
| duplicate complete, same invoice/paymentId/txid | Return existing verified state | NEEDS_LIVE_REPEAT | Repeat complete call and capture safe result |
| duplicate receipt/evidence request | Return existing receipt/evidence pair | NEEDS_LIVE_REPEAT | Repeat endpoint call and confirm no duplicate pair |
| duplicate public verify | Return same minimum-disclosure result | PASSED_CONTROLLED / NEEDS_REPEAT | Repeat public verify call |
| duplicate invoice create | Create separate invoice only if intentionally requested | PARTIAL | Define idempotency key policy |
| browser refresh during checkout | No duplicate receipt/evidence | NEEDS_LIVE_REPEAT | Simulate refresh/retry |
| network retry after payment | No duplicate trust record | NEEDS_LIVE_REPEAT | Simulate retry behavior |

---

## 9. Replay Protection Evidence Matrix

Replay protection prevents old proof material from being reused.

---

| Scenario | Target Behavior | Current Status | Required Next Evidence |
|---|---|---|---|
| same paymentId used on another invoice | Reject | PLANNED_HARDENING | DB-level unique/binding evidence |
| same txid used on another invoice | Reject | PLANNED_HARDENING | DB-level unique/binding evidence |
| old receipt used for unrelated access | Reject | PLANNED_HARDENING | Receipt context binding |
| receipt/evidence pair mixed with another record | Reject | NEEDS_LIVE_REPEAT | Mismatched public verify test |
| malformed receipt/evidence IDs | Reject | PLANNED_HARDENING | Safe parser test |
| stale receipt after expiry | Reject or expired state | PLANNED_HARDENING | Expiry policy |
| revoked receipt | Revoked state | PLANNED_HARDENING | Revocation policy |
| disputed receipt | Disputed state | PLANNED_HARDENING | Dispute state policy |

---

## 10. Durable State Failure Matrix

Durable state failure must not create fake success.

---

| Failure Scenario | Target Behavior | Access Result | Receipt/Evidence Result | Current Status |
|---|---|---|---|---|
| database read fails before approve | Reject safely | No unlock | No receipt/evidence | PLANNED_HARDENING |
| database write fails during approve | No fake approval | No unlock | No receipt/evidence | PLANNED_HARDENING |
| database write fails during complete | No fake verified state unless persisted | No unlock | No receipt/evidence | PLANNED_HARDENING |
| payment verified but state write fails | Reconciliation required | No unlock unless durable verified state exists | No receipt/evidence | PLANNED_HARDENING |
| receipt create fails after verified state | Retry or failure state | No unlock unless pair is complete | No complete pair | PLANNED_HARDENING |
| evidence create fails after receipt | Retry or failure state | No unlock unless pair is complete | Incomplete pair not public verified | PLANNED_HARDENING |
| public verify DB read fails | No fake verified response | No change | No new record | PLANNED_HARDENING |
| partial state exists | Manual review/reconciliation | No fake unlock | No duplicate pair | PLANNED_HARDENING |

---

## 11. Public Verify Minimum Disclosure Matrix

Public verify must remain minimum-disclosure.

---

| Field / Data Type | Public Verify Behavior | Current Status |
|---|---|---|
| verified | Allowed | PASSED_CONTROLLED |
| receiptId | Allowed if public-safe | PASSED_CONTROLLED |
| evidenceId | Allowed if public-safe | PASSED_CONTROLLED |
| paymentState | Allowed in public-safe form | PASSED_CONTROLLED |
| accessState | Allowed in public-safe form | PASSED_CONTROLLED |
| accessUnlocked | Allowed | PASSED_CONTROLLED |
| minimumDisclosure | Allowed | PASSED_CONTROLLED |
| publicSafe | Allowed | PASSED_CONTROLLED |
| paymentId | Must not expose | PASSED_CONTROLLED |
| txid | Must not expose | PASSED_CONTROLLED |
| raw Pi API response | Must not expose | PASSED_CONTROLLED |
| service role key | Must not expose | PASSED_CONTROLLED |
| access token | Must not expose | PASSED_CONTROLLED |
| private wallet data | Must not expose | PASSED_CONTROLLED |
| customer data | Must not expose | PASSED_CONTROLLED |
| stack trace | Must not expose | PLANNED_HARDENING |
| internal deployment path | Must not expose | PLANNED_HARDENING |

---

## 12. Receipt Integrity Matrix

Receipt/evidence integrity is a V9.1 hardening target.

---

| Integrity Feature | Purpose | Current Status |
|---|---|---|
| receiptId | Public-safe receipt reference | PASSED_CONTROLLED |
| evidenceId | Public-safe evidence reference | PASSED_CONTROLLED |
| issuedAt / createdAt | Time context | PARTIAL |
| receiptHash | Tamper-evidence | PLANNED_HARDENING |
| evidenceHash | Tamper-evidence | PLANNED_HARDENING |
| server-side HMAC/signature | Integrity proof | PLANNED_HARDENING |
| signaturePresent field | Public-safe integrity indicator | PLANNED_HARDENING |
| tamperCheckPassed field | Public-safe integrity result | PLANNED_HARDENING |
| expires_at | Optional expiry control | PLANNED_HARDENING |
| revoked/disputed state | Lifecycle control | PLANNED_HARDENING |
| immutable receipt/evidence binding | Replay protection | PLANNED_HARDENING |

---

## 13. Pi Verification Depth Matrix

Backend-verified payment state should be technically specific.

---

| Verification Check | Purpose | Current Status |
|---|---|---|
| paymentId exists | Identify Pi payment object | PASSED_CONTROLLED |
| paymentId bound to invoice | Prevent mismatch | PARTIAL / NEEDS_LIVE_REPEAT |
| amount matches invoice | Prevent under/over mismatch | NEEDS_DOCUMENTATION |
| txid present at completion | Confirm completion payload | PASSED_CONTROLLED |
| txid not reused | Replay prevention | PLANNED_HARDENING |
| payment state completed/verified | Confirm payment status | PASSED_CONTROLLED |
| invoice/order binding | Prevent cross-invoice binding | PARTIAL |
| environment boundary | Keep Sandbox/Testnet separate from Mainnet claims | PASSED_PUBLIC_BOUNDARY |
| replay checks | Prevent reuse | PLANNED_HARDENING |
| Pi API timeout behavior | Fail secure | PLANNED_HARDENING |

---

## 14. Rate Limiting / Abuse Matrix

Abuse resistance is a V9.1 hardening target.

---

| Abuse Scenario | Target Behavior | Current Status |
|---|---|---|
| repeated invoice creation | Throttle or limit | PLANNED_HARDENING |
| repeated approve calls | Idempotent + rate controlled | PLANNED_HARDENING |
| repeated complete calls | Idempotent + rate controlled | PLANNED_HARDENING |
| public verify scraping | Rate limit / anti-enumeration | PLANNED_HARDENING |
| brute-force receipt/evidence IDs | Anti-enumeration + rate limit | PARTIAL |
| AI-agent high-frequency calls | API limits before agent readiness | PLANNED_HARDENING |
| malformed request spam | Safe rejection + throttle | PLANNED_HARDENING |
| callback endpoint abuse | Auth/validation + throttle | PLANNED_HARDENING |

---

## 15. Current Evidence Summary

### Demonstrated Strongly

- controlled Pi Testnet payment-spine pass
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend verified payment state
- receipt/evidence after verified state
- access unlock after backend-verified receipt
- public verify minimum disclosure
- no public exposure of paymentId / txid in public verify

---

### Demonstrated Directionally

- fail-secure receipt/evidence guard
- no frontend-callback-only unlock
- server-side approval/completion direction
- durable state direction
- public-safe evidence direction
- claim boundary discipline

---

### Needs Live Repeat Evidence

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- paymentId mismatch behavior
- receipt/evidence mismatch public verify
- missing parameter behavior
- safe public error behavior
- public verify unknown pair behavior

---

### Planned V9.1 Hardening

- database failure behavior
- timeout/ambiguous payment handling
- cleanup/manual review policy
- receipt hash/signature
- replay immutability
- public verify freshness policy
- rate limiting
- abuse resistance
- formal state table
- merchant-facing simplified behavior explanation

---

## 16. Reviewer Interpretation

This matrix should be read as a technical honesty document.

It does not claim that every backend edge case is already complete.

It shows that SafeGate understands the required backend behaviors and is turning reviewer criticism into a hardening plan.

The current strongest claim remains:

SafeGate passed a controlled Pi Testnet payment-spine flow and unlocks access only after backend-verified receipt/evidence.

The next required claim should be:

SafeGate backend behavior remains safe under duplicate, replay, timeout, mismatch, and failure scenarios.

That next claim is not yet fully made.

---

## 17. Next Test Targets

Recommended next tests:

1. duplicate approve test
2. duplicate complete test
3. duplicate receipt/evidence test
4. receipt before verified payment test repeat
5. paymentId mismatch test
6. receipt/evidence mismatch public verify test
7. missing txid test
8. unknown receipt/evidence public verify test
9. safe error response test
10. durable state failure simulation

---

## 18. What This Matrix Does Not Claim

This matrix does not claim:

- production readiness
- complete backend hardening
- complete replay protection
- complete idempotency proof
- complete rate limiting
- complete cryptographic integrity
- formal security audit
- official Pi partnership
- Pi Mainnet settlement
- enterprise-grade security
- completed commercial pilot

---

## 19. Final Backend Rule

SafeGate backend behavior must remain conservative:

If payment state is missing, ambiguous, duplicated, mismatched, stale, or not durably verified, SafeGate must not unlock access.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
