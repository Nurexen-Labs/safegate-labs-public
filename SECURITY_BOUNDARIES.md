# SafeGate Security Boundaries

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document defines SafeGate’s current public security boundaries.

The purpose is to prevent overclaiming and to make SafeGate’s current security posture clear for reviewers, testers, merchants, and pilot discussions.

This is not a formal audit.

This is not a security certification.

This is not a production-readiness claim.

This is a boundary document.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Pilot Readiness | Open |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Security Certification | Not claimed |
| Tamper-Proof Infrastructure | Not claimed |
| Independent Cryptographic Verification | Not yet claimed |

---

## Current Evidence

SafeGate has passed two controlled evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

These milestones support Pilot Readiness and technical review.

They do not prove production-grade security.

They do not replace a formal audit.

They do not prove every duplicate, replay, timeout, failure, freshness, or integrity scenario.

---

## Controlled Pi Testnet Payment Spine — PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

Observed public-safe result:

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

This validation does not prove full backend security, real duplicate approve/complete behavior, real paymentId/txid replay handling, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## Core Security Boundary

SafeGate’s core security boundary is:

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Unknown, missing, ambiguous, duplicate, replayed, mismatched, expired, failed, stale, or unavailable states should fail secure.

---

## What SafeGate Can Currently Claim

SafeGate can currently claim:

- controlled Pi Testnet payment-spine evidence passed
- V9.1 safe negative backend validation passed
- access unlock after backend-verified receipt was demonstrated in controlled flow
- invoice creation kept access locked in V9.1 validation
- invoice creation did not create receipt/evidence in V9.1 validation
- receipt/evidence before verified payment was rejected safely in V9.1 validation
- missing invoiceId and paymentId were rejected safely in V9.1 validation
- unknown public verify pair did not return active verified trust in V9.1 validation
- selected validation responses did not show obvious secret/internal leak terms
- public-safe evidence boundaries are documented
- Pilot Readiness is open

---

## What SafeGate Must Not Claim

SafeGate must not currently claim:

- production readiness
- fully secure backend
- formal security audit passed
- enterprise-grade security certification
- tamper-proof infrastructure
- independent cryptographic verification
- Pi Mainnet settlement
- official Pi Core Team partnership
- regulatory approval
- completed commercial pilot
- all duplicate callback risks solved
- all replay risks solved
- all timeout/failure risks solved
- full receipt integrity implementation
- complete public verify freshness implementation

---

## Public Data Boundary

Public verify and public evidence should not expose:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- bearer token
- service role key
- private API key
- raw Pi API response
- raw Supabase error
- database URL
- environment variables
- stack traces
- private customer data
- private wallet data
- seed phrase
- private key

---

## Public-Safe Allowed Fields

Public-safe evidence may show:

- paymentState label
- accessState label
- verified status
- publicSafe status
- minimumDisclosure status
- receipt/evidence status
- privateDataIncluded: false
- secretsIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false
- visible pass/fail counts
- general test environment
- general reviewer feedback

---

## Backend Security Boundary

SafeGate backend behavior should preserve:

- strict state transitions
- backend-verified payment before receipt/evidence
- receipt/evidence before access unlock
- public verify minimum disclosure
- safe rejection for missing inputs
- safe rejection for unknown public verify pair
- safe error responses
- no secret leakage
- no frontend-only access unlock

Current evidence confirms selected parts of this boundary.

Deeper backend security remains a hardening target.

---

## Duplicate / Replay Boundary

SafeGate has not yet fully proven:

- duplicate approve idempotency
- duplicate complete idempotency
- duplicate receipt/evidence idempotency
- real paymentId replay rejection
- real txid replay rejection
- paymentId mismatch rejection
- txid mismatch rejection
- public verify mismatched pair rejection

These remain V9.1 hardening targets.

SafeGate should not claim all duplicate or replay risks are solved.

---

## Timeout / Failure Boundary

SafeGate has not yet fully proven:

- Pi API timeout behavior
- Pi API ambiguous response behavior
- Supabase read failure behavior
- Supabase write failure behavior
- receipt write succeeds but evidence write fails
- evidence write succeeds but access unlock fails
- public verify stale read behavior
- backend unavailable behavior

Expected future behavior:

- fail secure
- do not unlock access from ambiguous state
- do not return active verified trust from unavailable state
- do not expose internal error details

---

## Receipt Integrity Boundary

SafeGate has a receipt integrity direction.

SafeGate does not yet claim completed receipt cryptographic integrity.

Future direction may include:

- canonical receipt payload
- canonical evidence payload
- receipt hash
- evidence hash
- server-side HMAC or signature
- key versioning
- integrityStatus
- tamper-detection behavior
- public-safe integrity metadata

Current boundary:

Tamper-proof infrastructure is not claimed.

Independent cryptographic verification is not yet claimed.

---

## Public Verify Freshness Boundary

SafeGate has a public verify freshness direction.

SafeGate does not yet claim completed public verify freshness implementation.

Future direction may include:

- active
- expired
- revoked
- disputed
- stale
- unavailable
- review-required
- lastCheckedAt
- verifiedAt
- issuedAt
- expiresAt

Current boundary:

A historical verified record should not be overclaimed as permanently active without freshness context.

---

## AI Agent / Automation Boundary

Future AI agents or automated systems may increase:

- request volume
- replay attempts
- callback pressure
- endpoint probing
- rate-limit pressure
- abuse risk

SafeGate should not claim AI-agent readiness until backend hardening, rate limiting, idempotency, replay resistance, and safe error handling are stronger.

---

## Reviewer Security Questions

A reviewer should ask:

1. Can access unlock without backend-verified payment?
2. Can receipt/evidence be created before verified payment?
3. Can frontend callback alone unlock access?
4. Can duplicate callbacks create duplicate records?
5. Can paymentId or txid be replayed?
6. Can public verify return active trust for unknown or mismatched pair?
7. Can public verify expose sensitive identifiers?
8. Can backend errors leak secrets?
9. What happens if Pi API times out?
10. What happens if Supabase write fails?

---

## Safe Security Language

SafeGate may say:

- security boundaries are documented
- selected safe negative backend checks passed
- production readiness is not claimed
- formal audit is not claimed
- duplicate/replay/failure cases remain hardening targets
- public evidence follows minimum-disclosure direction

SafeGate should not say:

- fully secure
- production secure
- formally audited
- enterprise-grade security certified
- tamper-proof
- all security risks solved
- all duplicate/replay risks solved
- independent cryptographic verification completed

---

## Final Security Boundary Statement

SafeGate has meaningful controlled evidence and selected V9.1 backend validation evidence.

SafeGate remains in Pilot Readiness.

SafeGate is not production-ready.

SafeGate is not formally audited.

SafeGate’s next security credibility layer is deeper duplicate, replay, mismatch, timeout, durable-state failure, freshness, receipt-integrity, safe-error, and rate-limit validation.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
