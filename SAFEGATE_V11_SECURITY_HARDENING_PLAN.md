# SafeGate V11 Security Hardening Plan

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document defines SafeGate’s V11 security hardening direction.

V11 is a security logic and hardening plan.

It builds on the current evidence baseline:

- Controlled Pi Testnet Payment Spine: PASSED
- V9.1 Safe Negative Backend Validation: PASSED

This is not a production-readiness claim.

This is not a formal audit.

This is not a security certification.

This is not Pi Mainnet settlement.

This is not an official Pi partnership claim.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| V11 Security Hardening | Plan / direction |
| Pilot Readiness | Open |
| V10 Controlled Pilot Preparation | Planned |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Security Certification | Not claimed |

---

## Current Evidence Baseline

SafeGate has passed two important controlled evidence milestones.

### 1. Controlled Pi Testnet Payment Spine — PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow demonstrated:

- V9 invoice creation
- access initially locked
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend approval / completion flow
- backend-verified payment state
- receipt/evidence generation after verified state
- minimum-disclosure public verification
- access unlock only after backend-verified receipt

Observed public-safe result:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
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

Live evidence:

https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html

Related note:

SAFEGATE_CONTROLLED_PI_TESTNET_PAYMENT_SPINE_PASSED.md

---

### 2. V9.1 Safe Negative Backend Validation — PASSED

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

Live validation:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Related note:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

Boundary:

This validation does not prove full backend security, real duplicate approve/complete behavior, real paymentId/txid replay handling, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## V11 Security Principle

The main V11 security principle is:

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Unknown, missing, ambiguous, duplicate, replayed, mismatched, expired, failed, stale, or unavailable states should fail secure.

---

## V11 Hardening Goals

V11 should harden SafeGate around:

- strict state machine control
- idempotency
- replay resistance
- paymentId / txid mismatch handling
- duplicate callback handling
- receipt/evidence duplication control
- Pi API timeout behavior
- Supabase failure behavior
- safe error responses
- secret-leak prevention
- public verify minimum disclosure
- public verify freshness
- receipt/evidence integrity
- rate limiting / abuse resistance
- reviewer-verifiable negative tests

---

## State Machine Hardening

SafeGate should preserve strict legal transitions.

Legal direction:

1. invoice created
2. access locked
3. payment pending
4. backend verifies payment
5. receipt/evidence created
6. public verify becomes available
7. access unlocks after backend-verified receipt

Illegal transitions:

- invoice created to access unlocked
- frontend callback to access unlocked
- payment pending to receipt created
- missing paymentId to receipt created
- missing invoiceId to evidence created
- unknown public verify pair to verified trust
- failed payment to access unlocked
- expired invoice to access unlocked
- duplicate replay to new receipt/evidence
- ambiguous state to active verified trust

V9.1 validation confirmed selected illegal paths failed safely.

V11 should expand this into deeper repeatable tests.

---

## Idempotency Hardening

V11 should make repeated requests safe.

Target behavior:

- duplicate approve does not create conflicting state
- duplicate complete does not create conflicting state
- duplicate receipt request does not create uncontrolled duplicate receipt
- duplicate evidence request does not create uncontrolled duplicate evidence
- repeated public verify returns stable public-safe output
- repeated frontend callback cannot unlock access without backend-verified state

Recommended direction:

- stable idempotency key
- invoiceId + paymentId binding
- paymentId uniqueness
- txid uniqueness when available
- receipt/evidence uniqueness by verified payment state
- safe duplicate response instead of duplicate mutation

---

## Replay Resistance

V11 should prevent old or foreign payment identifiers from creating new trust outcomes.

Replay risks:

- reused paymentId
- reused txid
- paymentId from another invoice
- txid from another invoice
- stale callback
- old receipt/evidence pair
- copied public verify pair
- manually edited invoiceId

Expected behavior:

- reject mismatched paymentId
- reject mismatched txid
- reject unknown paymentId
- reject stale or already-used payment identifiers
- do not create new receipt/evidence from replayed identifiers
- do not unlock access from replayed identifiers

Boundary:

Full replay resistance is not yet claimed.

---

## Duplicate Callback Hardening

Payment systems may send callbacks more than once.

V11 should handle duplicate callbacks safely.

Target behavior:

- first valid backend-verified completion may finalize state
- duplicate completion should return stable final state
- duplicate callback should not create duplicate receipt/evidence
- duplicate callback should not change receipt/evidence binding
- duplicate callback should not expose internal state
- duplicate callback should not unlock access twice in a conflicting way

Boundary:

Real duplicate approve / complete behavior remains a hardening target.

---

## PaymentId / Txid Mismatch Hardening

V11 should test mismatch cases.

Examples:

- correct invoiceId + wrong paymentId
- wrong invoiceId + correct paymentId
- correct receiptId + wrong evidenceId
- correct public verify pair but stale state
- txid belongs to another invoice
- paymentId belongs to another user/session
- amount or memo mismatch if available

Expected behavior:

- reject mismatch safely
- keep access locked
- do not create receipt/evidence
- do not return active verified trust
- return public-safe error
- do not leak raw payment identifiers

---

## Timeout / Ambiguous State Hardening

If Pi API, backend, or database state is unavailable, SafeGate should fail secure.

Timeout cases:

- Pi API timeout
- Pi API 5xx
- Pi API ambiguous response
- backend completion timeout
- Supabase read timeout
- Supabase write timeout
- public verify timeout
- receipt write succeeds but evidence write fails
- evidence write succeeds but access unlock fails

Expected behavior:

- do not unlock access
- do not create verified receipt/evidence from ambiguous state
- do not return active verified trust
- return review-required or unavailable state when appropriate
- do not leak stack traces or secrets

---

## Supabase / Durable-State Hardening

V11 should treat durable-state behavior as security-critical.

Target checks:

- invoice write failure
- payment state write failure
- receipt write failure
- evidence write failure
- read-after-write failure
- partial receipt/evidence write
- duplicate row conflict
- unique constraint behavior
- stale read behavior
- public verify record missing
- public verify record mismatched

Expected behavior:

- fail secure
- keep access locked if final state is not durable
- do not expose internal database errors
- record safe review-required state if needed

---

## Safe Error Response Hardening

SafeGate should avoid leaking internal implementation details.

Public responses should not expose:

- service role key
- API secret
- raw Pi API response
- raw Supabase error
- stack trace
- environment variables
- database URL
- access token
- bearer token
- private customer data
- wallet address
- raw paymentId
- raw txid

V9.1 selected leak scans passed.

V11 should expand safe error validation across more failure cases.

---

## Public Verify Hardening

Public verify should confirm outcome without exposing sensitive payment data.

Public verify should support:

- verified status
- publicSafe status
- minimumDisclosure status
- paymentState label
- accessState label
- receiptStatus
- evidenceStatus
- freshnessStatus when implemented
- integrityStatus when implemented

Public verify should reject safely:

- missing receiptId
- missing evidenceId
- unknown pair
- mismatched pair
- stale pair
- revoked pair
- disputed pair

Boundary:

Full public verify freshness is not yet claimed.

---

## Receipt / Evidence Integrity Hardening

V11 should prepare receipt/evidence integrity.

Future integrity fields may include:

- canonical receipt payload
- canonical evidence payload
- receipt hash
- evidence hash
- combined trust hash
- server-side HMAC or signature
- key version
- issuedAt
- verifiedAt
- expiresAt if needed
- integrityStatus
- tamper-detection behavior
- public-safe integrity metadata

Boundary:

SafeGate does not currently claim tamper-proof infrastructure.

SafeGate does not currently claim independent cryptographic verification.

---

## Rate Limiting / Abuse Resistance

V11 should prepare for automated abuse.

Potential abuse cases:

- repeated invoice creation
- repeated verify attempts
- repeated public verify requests
- brute-force receipt/evidence IDs
- duplicate callback pressure
- AI-agent endpoint probing
- competitor/database-abuse testing
- high-volume missing-input requests

Recommended direction:

- per-endpoint rate limits
- merchant/API-key-aware limits when applicable
- anti-enumeration IDs
- safe 4xx responses
- no expensive backend work for invalid requests
- monitoring / logging without leaking private data

---

## AI-Agent Security Boundary

AI agents can make SafeGate more useful, but also riskier.

AI agents may increase:

- request volume
- replay attempts
- callback pressure
- endpoint probing
- automated misuse
- rate-limit pressure

SafeGate should not claim AI-agent readiness until:

- idempotency is stronger
- replay resistance is stronger
- public verify is stable
- safe error responses are broader
- receipt/evidence integrity is implemented
- freshness is implemented
- rate limiting is tested

---

## V11 Test Matrix

V11 should define repeatable tests for:

| Test Area | Expected Safe Behavior |
|---|---|
| duplicate approve | stable final state, no duplicate mutation |
| duplicate complete | stable final state, no duplicate mutation |
| duplicate receipt/evidence | no uncontrolled duplicate records |
| paymentId replay | reject safely |
| txid replay | reject safely |
| paymentId mismatch | reject safely |
| txid mismatch | reject safely |
| public verify missing input | reject safely |
| public verify unknown pair | no active verified trust |
| public verify mismatched pair | reject safely |
| Pi API timeout | fail secure |
| Supabase write failure | fail secure |
| partial durable write | review-required / locked |
| safe error response | no internal leak |
| brute-force IDs | no useful enumeration |

---

## V11 Reviewer Questions

A reviewer should ask:

1. Can access unlock without backend-verified payment?
2. Can receipt/evidence be created before verified payment?
3. Can duplicate approve or complete mutate state incorrectly?
4. Can a paymentId or txid be replayed?
5. Can a mismatched pair return active verified trust?
6. Can public verify expose payment identifiers?
7. Can backend failure unlock access accidentally?
8. Can database failure create inconsistent trust state?
9. Are errors public-safe?
10. Which tests are proven and which are planned?

---

## Safe V11 Language

SafeGate may say:

- V11 security hardening direction is documented
- selected V9.1 safe negative backend checks passed
- duplicate/replay/failure cases remain hardening targets
- production readiness is not claimed
- formal audit is not claimed
- security certification is not claimed

SafeGate should not say:

- fully secure
- production secure
- formally audited
- security certified
- all replay risks solved
- all duplicate risks solved
- all timeout/failure risks solved
- tamper-proof infrastructure completed
- independent cryptographic verification completed

---

## V11 Completion Criteria

V11 should only be considered stronger if SafeGate can show public-safe evidence for:

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- real paymentId replay rejection
- real txid replay rejection
- paymentId mismatch rejection
- public verify mismatched pair rejection
- Pi API timeout fail-secure behavior
- Supabase failure fail-secure behavior
- safe error response coverage
- public verify freshness behavior
- receipt/evidence integrity behavior

Until then, V11 remains a plan / direction.

---

## Final V11 Position

SafeGate has passed a controlled Pi Testnet payment-spine flow.

SafeGate has passed selected V9.1 safe negative backend validation.

V11 defines the next security hardening frontier:

- duplicate
- replay
- mismatch
- timeout
- durable-state failure
- public verify freshness
- receipt/evidence integrity
- safe errors
- rate limiting

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
