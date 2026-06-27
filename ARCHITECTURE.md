# SafeGate Architecture

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document explains the current SafeGate architecture.

The goal is to make the architecture understandable for technical reviewers, pilot reviewers, merchants, and ecosystem observers.

This is not a production-readiness claim.

This is not a formal audit.

This is not a security certification.

---

## Current Architecture Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Pilot Readiness | Open |
| V9.1 Backend + Integrity Hardening | Active direction |
| Controlled Pilot Execution | Planned / not yet claimed complete |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |

---

## Architecture Summary

SafeGate is a post-payment trust layer.

It is designed to connect a payment event to a verified outcome.

SafeGate’s architecture focuses on:

- invoice creation
- locked initial access state
- payment intent
- backend payment verification
- legal state transitions
- guarded receipt creation
- guarded evidence creation
- public-safe verification
- access unlock after backend-verified receipt
- merchant/reviewer proof
- future receipt integrity
- future freshness and dispute handling

SafeGate is Pi-first, not Pi-only.

---

## Core Architecture Rule

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Unknown, missing, ambiguous, duplicate, replayed, mismatched, expired, failed, stale, or unavailable states should fail secure.

---

## Controlled Pi Testnet Payment Spine — PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow demonstrated:

1. V9 invoice created
2. access initially remained locked
3. Pi Browser authentication succeeded
4. username + payments scope succeeded
5. Pi wallet payment succeeded in Sandbox / Testnet context
6. backend approval / completion flow executed
7. backend-verified payment state reached
8. receipt and evidence generated after verified state
9. public verify confirmed minimum-disclosure result
10. access unlocked only after backend-verified receipt

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

## Main Architecture Layers

SafeGate architecture can be understood in seven layers:

1. User / buyer layer
2. Merchant / service layer
3. Payment interface layer
4. Backend verification layer
5. Receipt / evidence layer
6. Public verify layer
7. Access / trust-state layer

---

## Layer 1 — User / Buyer Layer

The user interacts with a payment or checkout flow.

In the current Pi-first evidence path, the user may use:

- Pi Browser
- Pi authentication
- payments scope
- Pi Wallet
- Pi Sandbox / Testnet context

Boundary:

User-side payment signals are not enough to unlock access.

Frontend callback alone must not be treated as final trust.

---

## Layer 2 — Merchant / Service Layer

The merchant or app defines the service or access outcome.

Merchant-side questions:

- Was an invoice created?
- Did access start locked?
- Was payment backend verified?
- Was a receipt created?
- Was evidence created?
- Was access unlocked only after verified receipt?
- Can the result be checked later?
- Can this be shown without exposing private payment data?

SafeGate helps answer these post-payment questions.

---

## Layer 3 — Payment Interface Layer

SafeGate can integrate with a payment flow, but SafeGate does not process payments.

SafeGate should not be described as:

- a custodial wallet
- a bank
- a payment processor
- a production payment gateway
- a regulated financial institution

SafeGate observes and verifies post-payment outcome state.

---

## Layer 4 — Backend Verification Layer

Backend verification is the core trust gate.

Backend verification should check:

- invoice exists
- invoice is still eligible
- paymentId exists
- paymentId belongs to invoice
- amount matches invoice
- environment is correct
- payment state is completed or verified
- txid exists when required
- txid is not reused
- paymentId is not reused across invoices
- receipt/evidence is not duplicated
- access remains locked before verification
- final transition is legal

Current boundary:

Controlled Pi Testnet payment-spine evidence passed.

Full production-grade backend verification is not claimed.

---

## Layer 5 — Receipt / Evidence Layer

Receipt and evidence should be created only after backend-verified payment state.

Receipt/evidence should eventually support:

- receiptId
- evidenceId
- invoice binding
- payment-state binding
- issuedAt
- verifiedAt
- receiptStatus
- evidenceStatus
- publicSafe status
- minimumDisclosure status
- future receiptHash
- future evidenceHash
- future signature or HMAC direction

Current boundary:

Receipt/evidence generation after verified state was demonstrated in controlled flow.

Full cryptographic receipt integrity is not yet claimed.

---

## Layer 6 — Public Verify Layer

Public verify should confirm the outcome without exposing sensitive payment information.

Public verify may show:

- verified
- publicSafe
- minimumDisclosure
- paymentState label
- accessState label
- receiptStatus
- evidenceStatus
- privateDataIncluded
- secretsIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded

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

Current V9.1 validation confirmed selected missing/unknown public verify inputs failed safely.

---

## Layer 7 — Access / Trust-State Layer

Access state is the final sensitive outcome.

Access should remain locked until backend-verified receipt.

Important access states:

- LOCKED_UNTIL_BACKEND_VERIFIED_PAYMENT
- ACCESS_LOCKED
- ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- ACCESS_EXPIRED
- ACCESS_REVOKED
- ACCESS_DISPUTED
- ACCESS_REVIEW_REQUIRED
- ACCESS_UNAVAILABLE

Current evidence:

Controlled flow reached ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT.

V9.1 validation confirmed invoice creation kept access locked.

---

## State Machine Direction

SafeGate should preserve strict legal state transitions.

Expected legal direction:

1. invoice created
2. access locked
3. payment pending
4. backend verified payment
5. receipt/evidence created
6. public verify available
7. access unlocked after backend-verified receipt

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

Selected illegal paths passed V9.1 validation.

Deeper duplicate/replay/failure transitions remain open.

---

## Backend Behavior Direction

SafeGate backend behavior should be reviewable under:

- valid invoice create
- valid payment verification
- duplicate approve
- duplicate complete
- duplicate receipt/evidence request
- missing paymentId
- missing txid
- paymentId mismatch
- txid mismatch
- receipt before verified payment
- unknown public verify pair
- mismatched public verify pair
- Pi API timeout
- Supabase failure
- internal backend error

Current V9.1 validation passed selected public-safe negative cases.

Full backend behavior proof remains future hardening work.

---

## Receipt Integrity Direction

Receipt integrity should eventually include:

- canonical receipt payload
- canonical evidence payload
- receipt hash
- evidence hash
- combined trust hash if useful
- server-side HMAC or signature
- issuedAt
- optional expiresAt
- key versioning
- integrityStatus
- tamper-detection behavior
- public-safe integrity metadata

Boundary:

Tamper-proof infrastructure is not currently claimed.

Independent cryptographic verification is not yet claimed.

---

## Public Verify Freshness Direction

Public verify should eventually distinguish:

- active
- expired
- revoked
- disputed
- stale
- unavailable
- review-required

A payment or receipt that was verified at one time should not automatically imply current active trust forever.

Boundary:

Full public verify freshness implementation is not yet claimed.

---

## Security Boundary

SafeGate’s security boundary is deliberately conservative.

SafeGate may say:

- selected safe negative backend checks passed
- production readiness is not claimed
- formal audit is not claimed
- duplicate/replay/failure cases remain hardening targets
- public evidence follows minimum-disclosure direction

SafeGate should not say:

- fully secure
- production secure
- formally audited
- all duplicate/replay risks solved
- tamper-proof
- independent cryptographic verification complete

---

## Pilot Readiness Architecture Meaning

Pilot Readiness means the architecture is reviewable.

It does not mean production-ready.

Current Pilot Readiness includes:

- live review hub
- controlled payment-spine evidence
- live V9.1 backend validation page
- public-safe pass notes
- evidence templates
- risk consensus
- backend behavior matrix
- receipt integrity plan
- public verify freshness policy
- formal state machine table
- merchant explanation
- security boundaries
- controlled pilot checklist

---

## Current Public Review Surface

Main review links:

- https://www.safegatelabs.xyz
- https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html
- https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html
- README.md
- LINKS.md
- SAFEGATE_CURRENT_REVIEW_PACKAGE.md
- SAFEGATE_CONTROLLED_PI_TESTNET_PAYMENT_SPINE_PASSED.md
- SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md
- SAFEGATE_PUBLIC_SAFE_PAYMENT_SPINE_EVIDENCE_PACK.md
- SAFEGATE_EXTERNAL_TECHNICAL_REVIEW_NOTE.md
- SAFEGATE_CONTROLLED_PILOT_EXECUTION_CHECKLIST.md
- SAFEGATE_AI_REVIEW_RISK_CONSENSUS_V9.md
- SAFEGATE_V9_1_BACKEND_HARDENING_PLAN.md
- SAFEGATE_BACKEND_BEHAVIOR_EVIDENCE_MATRIX.md
- SAFEGATE_RECEIPT_INTEGRITY_PLAN.md
- SAFEGATE_PI_VERIFICATION_DEPTH_NOTE.md
- SAFEGATE_PUBLIC_VERIFY_FRESHNESS_POLICY.md
- SAFEGATE_FORMAL_STATE_MACHINE_TABLE.md
- SAFEGATE_MERCHANT_EXPLANATION.md
- SECURITY_BOUNDARIES.md

---

## Remaining Architecture Gaps

Remaining gaps before stronger pilot or production claims:

- duplicate approve idempotency
- duplicate complete idempotency
- duplicate receipt/evidence behavior
- real paymentId replay handling
- real txid replay handling
- paymentId mismatch behavior
- txid mismatch behavior
- public verify mismatched pair behavior
- Pi API timeout behavior
- Supabase write failure behavior
- partial write failure behavior
- receipt/evidence integrity implementation
- public verify freshness implementation
- rate limiting / abuse resistance
- deeper safe error response validation
- external technical reviewer feedback

---

## Architecture Review Question

The most useful architecture review question remains:

Where would this architecture break first if it moved from controlled Testnet evidence to a real pilot environment?

Reviewers should focus on:

- state machine strictness
- backend verification depth
- duplicate callback behavior
- idempotency
- replay resistance
- durable-state failure behavior
- public verify minimum disclosure
- public verify freshness
- receipt/evidence integrity
- safe error model
- rate limiting

---

## Final Architecture Position

SafeGate is no longer only a static concept.

SafeGate has passed a controlled Pi Testnet payment-spine flow.

SafeGate has passed a live V9.1 safe negative backend validation for selected public-safe cases.

SafeGate has a reviewable post-payment trust architecture.

SafeGate is in Pilot Readiness.

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
