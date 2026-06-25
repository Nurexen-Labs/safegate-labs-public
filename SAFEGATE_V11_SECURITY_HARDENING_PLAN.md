# SafeGate V11 — Security Hardening Plan

## Status

SafeGate V11 is the security hardening phase.

V8 established the public evidence layer.

V9 established controlled Pi Sandbox / Testnet-oriented payment spine evidence.

V10 prepared SafeGate for hackathon submission, public visibility, and reviewer readiness.

V11 focuses on strengthening SafeGate before broader pilots.

---

## V11 Purpose

The purpose of V11 is to harden SafeGate’s payment-state, receipt/evidence, access-control, and public verification architecture.

V11 does not replace V8, V9, or V10.

V11 builds on them by improving security, correctness, and abuse resistance.

---

## Core Security Principle

Frontend is never the final source of truth.

A frontend callback alone must not create final trust proof.

A frontend callback alone must not unlock access.

Receipt/evidence creation and access state must depend on backend-controlled verified payment state.

No verified backend state means:

- no final receipt/evidence
- no trusted access unlock
- no final post-payment trust record

---

## V11 Security Focus Areas

V11 focuses on:

- replay protection
- idempotency
- duplicate payment handling
- timeout handling
- cancelled payment handling
- failed payment handling
- refund state
- dispute state
- illegal state transition blocking
- double unlock prevention
- public verify rate limiting
- anti-enumeration IDs
- hash and timestamp integrity
- public-safe error handling
- merchant / API-key limits
- audit logs

---

## Replay Protection

### Problem

A malicious user, script, or broken client may try to reuse the same paymentId, invoiceId, receiptId, or verification result more than once.

### V11 Direction

Each payment, invoice, receipt, and evidence record should have replay protection.

### Expected Rule

One verified payment should not create multiple unauthorized unlocks or duplicate final evidence records.

### Short Line

Same payment cannot be reused to fake new trust.

---

## Idempotency

### Problem

Payment callbacks, browser retries, network retries, or user double-clicks may send the same request more than once.

### V11 Direction

Use idempotency keys and locked state transitions so repeated requests return the same safe result instead of creating duplicates.

### Expected Rule

Repeated requests should not create duplicate receipt/evidence/access unlock records.

### Short Line

Retry should be safe, not dangerous.

---

## Duplicate Payment Handling

### Problem

A user may start multiple payment attempts for the same invoice or merchant flow.

### V11 Direction

SafeGate should clearly track invoice/payment relationships and prevent ambiguous final states.

### Expected Rule

One invoice should have one controlled final payment outcome unless explicitly designed otherwise.

### Short Line

Duplicate payment attempts must not confuse the trust state.

---

## Timeout Handling

### Problem

Pi API, browser session, backend request, or network connection may timeout.

### V11 Direction

Timeout should not be treated as success.

### Expected Rule

If verification is timeout, ambiguous, or incomplete, SafeGate remains locked.

### Short Line

Timeout is not proof.

---

## Cancelled Payment Handling

### Problem

A user may cancel the payment flow.

### V11 Direction

Cancelled payment should create a safe non-final state.

### Expected State

PAYMENT_CANCELLED

### Expected Rule

No receipt/evidence and no access unlock after cancelled payment.

### Short Line

Cancelled means locked.

---

## Failed Payment Handling

### Problem

Payment verification may fail.

### V11 Direction

Failed verification should be stored safely and should not unlock anything.

### Expected State

PAYMENT_FAILED

### Expected Rule

Failed payment cannot create final trust proof.

### Short Line

Failed verification means no trusted outcome.

---

## Refund State

### Problem

A payment may later be refunded.

### V11 Direction

SafeGate should support a refund-aware state.

### Expected State

REFUNDED

### Expected Rule

If refunded, the trust state should not keep presenting the payment as active/final in the same way.

### Short Line

Refund must change the trust state.

---

## Dispute State

### Problem

User and merchant may disagree after payment.

### V11 Direction

SafeGate should support a dispute-aware state.

### Expected State

DISPUTED

### Expected Rule

Dispute should preserve evidence and clearly mark the outcome as under dispute.

### Short Line

Dispute does not delete evidence; it changes status.

---

## Illegal State Transition Blocking

### Problem

A bad actor or bug may try to jump from an early state directly to access unlock.

Bad example:

INVOICE_CREATED → ACCESS_UNLOCKED

### V11 Direction

Only legal state transitions should be allowed.

### Expected Flow

INVOICE_CREATED  
→ PAYMENT_PENDING  
→ VERIFYING_PAYMENT  
→ PAYMENT_VERIFIED  
→ RECEIPT_EVIDENCE_CREATED  
→ ACCESS_UNLOCKED

### Expected Rule

Access unlock cannot happen before verified payment and required evidence conditions.

### Short Line

No shortcut to unlock.

---

## Double Unlock Prevention

### Problem

The same payment or invoice may try to unlock access multiple times.

### V11 Direction

Access unlock should be atomic and controlled.

### Expected Rule

If already unlocked, a repeated unlock request should return the existing state, not create a new unlock.

### Short Line

One verified outcome, one controlled unlock.

---

## Public Verify Rate Limiting

### Problem

Public verification endpoints can be abused by bots, scrapers, or brute-force attempts.

### V11 Direction

Add rate limits and anti-enumeration protections.

### Expected Rule

Public verify must remain useful but not easily abusable.

### Short Line

Public does not mean unlimited abuse.

---

## Anti-Enumeration IDs

### Problem

If receipt/evidence IDs are predictable, attackers may try to guess records.

### V11 Direction

Use non-guessable IDs, strong random identifiers, or UUID-style records.

### Expected Rule

Receipt/evidence lookup should require valid non-predictable identifiers.

### Short Line

IDs must not be easy to guess.

---

## Hash / Timestamp Integrity

### Problem

Evidence records need stronger tamper-evident structure.

### V11 Direction

Add stronger hash proof and timestamp integrity.

Possible fields:

- receiptHash
- evidenceHash
- createdAt
- verifiedAt
- stateHash
- serverSignature / secret-based proof later

### Expected Rule

Evidence should be difficult to alter silently.

### Short Line

Evidence should be tamper-evident.

---

## Public-Safe Error Handling

### Problem

Errors may accidentally leak internal details like database names, stack traces, API structure, or secret logic.

### V11 Direction

Return clean public-safe errors.

### Expected Rule

No secret, stack trace, service role, database internals, or private debug info should be exposed publicly.

### Short Line

Errors must explain enough, but leak nothing.

---

## Merchant / API-Key Limits

### Problem

Future merchants or apps may overload endpoints accidentally or maliciously.

### V11 Direction

Add merchant-level or API-key-level rate limits.

### Expected Rule

Abuse should be limited per merchant/app, not only by IP.

### Short Line

Trust infrastructure needs abuse controls.

---

## Audit Logs

### Problem

If something goes wrong, SafeGate needs a clear history of state changes.

### V11 Direction

Record important transitions.

Examples:

- invoice created
- payment pending
- verification requested
- payment verified
- receipt/evidence created
- access unlocked
- public verify requested
- dispute/refund state changed

### Expected Rule

Important state changes should be auditable.

### Short Line

No trust without trace.

---

## V11 Correct Claim

SafeGate V11 focuses on security hardening before broader pilots.

It strengthens replay protection, idempotency, duplicate payment handling, timeout/cancelled/failed/refund/dispute states, state-machine integrity, anti-enumeration, rate limiting, public-safe errors, and audit logs.

---

## V11 Claim Boundary

V11 is a roadmap and hardening phase.

V11 should not claim full external audit completion unless a real external audit is completed.

V11 should not claim production-grade security until broader testing, monitoring, and review are completed.

---

## V11 One-Line Summary

V11 turns SafeGate from working MVP evidence into a more abuse-resistant trust architecture.

---

## Final Principle

Trust is not only a feature.

Trust is a state machine, an evidence trail, and a security boundary.
