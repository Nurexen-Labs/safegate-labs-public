# SafeGate V9.1 Backend + Integrity Hardening Plan

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Document Status

**Status:** Backend and integrity hardening plan  
**Scope:** V9.1 planning after controlled Pi Testnet payment-spine pass  
**Current Public Phase:** Pilot Readiness  
**Controlled Pi Testnet Payment Spine:** PASSED  
**Production Claim:** No  
**Security Certification Claim:** No  
**Third-Party Audit Claim:** No  

---

## Important Boundary

This document is a hardening plan.

It is not a production-readiness claim.

It is not a security audit.

It is not a certification.

It does not claim that all items are already implemented.

Its purpose is to define the next engineering closure layer before controlled pilot execution.

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

## Why V9.1 Exists

V9 proved the controlled payment spine.

V9.1 is designed to answer the next reviewer question:

What happens when the payment spine faces duplicate callbacks, replay attempts, timeout states, database failures, stale public verification, and receipt tampering risk?

V9.1 is not about adding more pages.

V9.1 is about hardening backend behavior and evidence integrity.

---

## V9.1 Primary Goal

The primary goal of V9.1 is:

Make backend behavior safer, clearer, and more externally reviewable before controlled pilot execution.

This includes:

- endpoint behavior clarity
- idempotency behavior
- replay protection
- timeout handling
- durable-state failure handling
- public-safe error behavior
- receipt/evidence integrity direction
- public verify freshness policy
- formal state transition documentation

---

## Priority 1 — Backend Behavior Evidence Matrix

Create a public-safe backend behavior matrix.

The matrix should document how SafeGate behaves under normal and abnormal endpoint scenarios.

Required scenarios:

| Scenario | Expected Behavior |
|---|---|
| valid invoice create | invoice created, access locked |
| valid approve | server-approved state recorded |
| duplicate approve | existing state returned, no duplicate success |
| valid complete | verified state recorded |
| duplicate complete | existing verified state returned |
| missing invoiceId | safe rejection |
| missing paymentId | safe rejection |
| missing txid | safe rejection |
| paymentId mismatch | safe rejection |
| receipt before payment verified | no receipt, no evidence, no unlock |
| duplicate receipt/evidence request | existing receipt/evidence returned |
| mismatched receipt/evidence public verify | verified false / not found |
| unknown receipt/evidence public verify | verified false / not found |
| malformed receipt/evidence ID | safe rejection |
| backend internal error | safe public error, no secrets |

Each row should include:

- endpoint
- scenario
- expected HTTP class
- expected state
- receipt/evidence result
- access result
- public-safe response
- current implementation status
- evidence status

---

## Priority 2 — Idempotency Hardening

SafeGate must be safe under repeated requests.

Target behavior:

- duplicate approve must not create inconsistent state
- duplicate complete must not create duplicate receipt/evidence
- duplicate receipt/evidence request must return existing pair
- duplicate public verify must return the same minimum-disclosure result
- duplicate payment identifiers must not bind to another invoice

Recommended controls:

- paymentId unique constraint
- txid unique constraint where available
- invoiceId unique binding
- receiptId unique constraint
- evidenceId unique constraint
- idempotency key support for future API usage
- immutable verified payment record after finalization
- audit log for repeated calls

---

## Priority 3 — Replay Protection

SafeGate must prevent old payment or proof material from being reused for a new trust event.

Replay risks:

- old paymentId used for new invoice
- old txid used for new invoice
- old receipt used for new access event
- receipt/evidence pair mixed across records
- stale verified result reused outside intended context

Target behavior:

- one paymentId binds to one invoice
- one txid binds to one payment record
- one receiptId binds to one evidenceId
- mismatched receipt/evidence pair fails public verify
- old receipt cannot unlock unrelated access
- receipt/evidence pair remains immutable after creation

Future hardening direction:

- nonce
- timestamp
- optional expires_at
- server-side receipt hash
- signed receipt payload or HMAC
- audit trail for replay attempts

---

## Priority 4 — Timeout and Pending-State Handling

Payment flows may become ambiguous.

Required states or policies:

- PAYMENT_PENDING
- PI_APPROVED_BY_SERVER
- COMPLETION_PENDING
- COMPLETION_TIMEOUT
- PAYMENT_VERIFIED_TESTNET
- PAYMENT_FAILED
- PAYMENT_EXPIRED
- PAYMENT_AMBIGUOUS_REVIEW
- RECEIPT_EVIDENCE_CREATED
- ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT

Timeout scenarios:

- user opens payment but does not complete
- Pi API times out
- Pi API returns ambiguous response
- backend approve succeeds but complete fails
- complete succeeds but client disconnects
- backend cannot confirm durable state

Target behavior:

- access remains locked
- no fake success
- no receipt/evidence before verified state
- pending records expire or move to review
- stale pending records can be cleaned
- ambiguous cases are visible for review

---

## Priority 5 — Durable-State Failure Handling

SafeGate depends on durable state.

V9.1 should define what happens when durable state fails.

Failure scenarios:

- database read fails
- database write fails
- connection drops mid-flow
- partial state is written
- payment verified but state write fails
- receipt generation fails after payment verification
- evidence generation fails after receipt generation
- public verify cannot read durable state

Target behavior:

- no access unlock without durable verified state
- no fake verified response
- no duplicate receipt/evidence
- safe retry path
- safe reconciliation path
- failure state recorded if possible
- operator-visible error status

Possible future states:

- DB_WRITE_FAILED
- RECEIPT_CREATE_FAILED
- EVIDENCE_CREATE_FAILED
- RECONCILIATION_REQUIRED
- MANUAL_REVIEW_REQUIRED

---

## Priority 6 — Safe Error Model

SafeGate must fail safely without leaking internals.

Public/API errors must not expose:

- stack traces
- Supabase internals
- service role key details
- raw Pi API response
- access tokens
- environment variables
- filesystem paths
- deployment internals
- private wallet data
- customer data

Preferred response style:

- ok: false
- code: SAFE_ERROR_CODE
- publicSafe: true
- message: short safe explanation
- no raw internal error object
- no sensitive identifiers

Example safe codes:

- MISSING_INVOICE_ID
- MISSING_PAYMENT_ID
- MISSING_TXID
- PAYMENT_NOT_VERIFIED
- PAYMENT_ID_MISMATCH
- RECEIPT_EVIDENCE_NOT_FOUND
- RECEIPT_EVIDENCE_MISMATCH
- DURABLE_STATE_UNAVAILABLE
- PAYMENT_VERIFICATION_AMBIGUOUS
- RATE_LIMITED
- INTERNAL_SAFE_FAILURE

---

## Priority 7 — Receipt / Evidence Integrity Layer

Minimum-disclosure public verify is useful, but V9.1 should define the next integrity layer.

Target integrity features:

- receipt hash
- evidence hash
- server-side HMAC or signature
- timestamp
- immutable receipt/evidence pair
- optional expires_at
- tamper-detection behavior
- public-safe integrity metadata

Receipt integrity should answer:

- Was this receipt generated by SafeGate backend?
- Has the receipt/evidence payload changed?
- Is this receipt bound to the correct invoice/payment record?
- Is this receipt still valid?
- Is this receipt revoked, expired, or disputed?

Public-safe integrity fields may include:

- receiptHash
- evidenceHash
- issuedAt
- integrityVersion
- signaturePresent
- tamperCheckPassed
- minimumDisclosure

Do not expose:

- signing secret
- HMAC secret
- service role key
- raw private payment identifiers

---

## Priority 8 — Pi Verification Depth Documentation

The phrase backend-verified payment state must be technically defined.

V9.1 should document what the backend verifies.

Verification depth should include:

- paymentId exists
- paymentId matches invoice
- amount matches invoice
- payment state is completed/verified in the intended environment
- txid is present when required
- txid is not reused
- invoice/order binding is valid
- environment boundary is clear: Sandbox / Testnet / Mainnet
- replay checks pass
- receipt/evidence is created only after verified state

Important boundary:

Controlled Pi Testnet payment-spine pass does not equal Pi Mainnet settlement.

---

## Priority 9 — Public Verify Freshness Policy

Public verify should clarify whether it is live, cached, static, revocable, or expiry-aware.

V9.1 should define:

- data source
- freshness model
- cache behavior if any
- revoked state behavior
- expired state behavior
- disputed state behavior
- access expired behavior
- public-safe update behavior

Possible public states:

- VERIFIED_ACTIVE
- VERIFIED_EXPIRED
- VERIFIED_REVOKED
- VERIFIED_DISPUTED
- NOT_FOUND
- MISMATCH
- AMBIGUOUS_REVIEW

Target behavior:

- public verify should not show stale trust state as current truth without context
- public verify should remain minimum-disclosure
- public verify should not expose sensitive identifiers

---

## Priority 10 — Rate Limiting / Abuse Resistance

V9.1 should prepare for abuse scenarios.

Abuse risks:

- repeated invoice creation
- repeated public verify requests
- brute-force receipt/evidence guessing
- callback endpoint spam
- AI-agent high-frequency calls
- merchant/API-key abuse in future API scenarios

Recommended controls:

- anti-enumeration IDs
- request rate limits
- IP-based basic throttling
- merchant/API-key limits for future use
- token bucket policy
- safe rate-limit error response
- logging of abuse attempts

---

## Priority 11 — Formal State Machine Table

V9.1 should define legal and illegal transitions.

Example legal transitions:

| From | To | Condition |
|---|---|---|
| INVOICE_CREATED | PAYMENT_PENDING | payment started |
| PAYMENT_PENDING | PI_APPROVED_BY_SERVER | backend approve succeeds |
| PI_APPROVED_BY_SERVER | PAYMENT_VERIFIED_TESTNET | backend complete succeeds |
| PAYMENT_VERIFIED_TESTNET | RECEIPT_EVIDENCE_CREATED | receipt/evidence guard passes |
| RECEIPT_EVIDENCE_CREATED | ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT | public-safe unlock condition passes |
| PAYMENT_PENDING | PAYMENT_EXPIRED | timeout reached |
| PI_APPROVED_BY_SERVER | COMPLETION_TIMEOUT | completion timeout |
| any non-final safe state | MANUAL_REVIEW_REQUIRED | ambiguous state |

Example illegal transitions:

| Illegal Transition | Expected Behavior |
|---|---|
| INVOICE_CREATED → ACCESS_UNLOCKED | reject |
| PAYMENT_PENDING → RECEIPT_EVIDENCE_CREATED | reject |
| PAYMENT_FAILED → ACCESS_UNLOCKED | reject |
| PAYMENT_EXPIRED → PAYMENT_VERIFIED without recheck | reject |
| receipt/evidence mismatch → VERIFIED | reject |
| duplicate paymentId on another invoice → reject |
| duplicate txid on another invoice → reject |

---

## Priority 12 — Controlled External Repeat Test

After hardening evidence is prepared, repeat the controlled payment-spine flow with a trusted external reviewer when feasible.

Target test shape:

- one trusted external reviewer
- one Pi Browser run
- one controlled Testnet payment
- one invoice
- one payment
- one receipt/evidence pair
- one public verify result
- redacted screenshots only
- reviewer notes captured

Do not publish raw sensitive data.

---

## V9.1 Output Documents

Suggested V9.1 public documents:

1. SAFEGATE_BACKEND_BEHAVIOR_EVIDENCE_MATRIX.md
2. SAFEGATE_V9_1_BACKEND_HARDENING_PLAN.md
3. SAFEGATE_RECEIPT_INTEGRITY_PLAN.md
4. SAFEGATE_PI_VERIFICATION_DEPTH_NOTE.md
5. SAFEGATE_PUBLIC_VERIFY_FRESHNESS_POLICY.md

These documents should remain boundary-safe.

They should not claim production readiness.

---

## V9.1 Completion Criteria

V9.1 can be considered complete for Pilot Readiness only when:

- backend behavior matrix is documented
- duplicate callback behavior is tested or clearly planned
- replay protection behavior is documented
- durable-state failure policy is documented
- safe error model is documented
- receipt integrity plan is documented
- Pi verification depth is documented
- public verify freshness policy is documented
- external repeat test plan is ready
- no production/security/audit overclaim is made

---

## What V9.1 Does Not Claim

V9.1 does not claim:

- production readiness
- secure system
- tamper-proof infrastructure
- independently verifiable proof
- formal audit completion
- regulatory approval
- official Pi partnership
- Pi Mainnet settlement
- enterprise-grade security
- high-volume production readiness
- completed commercial pilot
- complete privacy protocol

---

## Relationship to Future AI Agent Readiness

AI Agent Readiness remains a future layer.

Future AI Agent Readiness may include:

- machine-readable trust states
- OpenAPI specification
- llms.txt
- agent integration guide
- privacy-first receipt verification

However, AI Agent Readiness should not outrun backend hardening.

AI agents increase:

- request volume
- replay risk
- automation abuse
- callback pressure
- API misuse risk
- rate-limit requirements

Therefore, V9.1 Backend + Integrity Hardening should come first.

---

## Final V9.1 Direction

V9 proved the controlled payment spine.

V9.1 should prove that the payment spine behaves safely under stress, duplication, failure, replay, and public verification edge cases.

This is the next engineering frontier before controlled pilot execution.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
