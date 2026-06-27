# SafeGate V12 Trust-State / Privacy / Web3 Plan

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document explains SafeGate’s V12 trust-state, privacy, and Web3 architecture direction.

V12 is a forward-looking architecture plan.

It builds on the current V9 / V9.1 evidence baseline:

- Controlled Pi Testnet Payment Spine: PASSED
- V9.1 Safe Negative Backend Validation: PASSED

This is not a production-readiness claim.

This is not a completed privacy protocol.

This is not a formal audit.

This is not Pi Mainnet settlement.

This is not an official Pi partnership claim.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| V12 Trust-State / Privacy / Web3 Plan | Architecture direction |
| Pilot Readiness | Open |
| V9.1 Backend + Integrity Hardening | Active direction |
| V10 Controlled Pilot Preparation | Planned |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Complete Privacy Protocol | Not claimed |

---

## Current Evidence Baseline

SafeGate has crossed two important evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

These milestones show:

- controlled Pi Browser payment-spine evidence
- backend-verified payment-state direction
- receipt/evidence generated after verified payment state
- access unlock after backend-verified receipt
- selected negative backend cases rejected safely
- public verify minimum-disclosure direction
- no obvious secret/internal leak terms detected in selected validation responses

These milestones strengthen the V12 architecture direction.

They do not prove production readiness or full privacy infrastructure.

---

## Evidence Milestone 1 — Controlled Pi Testnet Payment Spine

**Status:** PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow demonstrated:

- V9 invoice creation
- initial access state remained locked
- Pi Browser authentication succeeded
- username + payments scope succeeded
- Pi wallet payment succeeded in Sandbox / Testnet context
- backend approval / completion flow executed
- backend-verified payment state reached
- receipt/evidence generated after verified state
- public verify confirmed minimum-disclosure result
- access unlocked only after backend-verified receipt

Public-safe result:

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

## Evidence Milestone 2 — V9.1 Safe Negative Backend Validation

**Status:** PASSED

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

## V12 Architecture Idea

V12 treats SafeGate as a trust-state architecture.

Payment is not the final product.

Payment is the trigger.

The product is a verifiable post-payment trust state.

SafeGate should map:

- invoice state
- payment state
- backend verification state
- receipt state
- evidence state
- public verify state
- access state
- freshness state
- integrity state
- dispute / review state

This makes SafeGate more than a checkout page.

---

## Trust-State Layer

SafeGate should define machine-readable trust states.

Possible trust states:

- INVOICE_CREATED
- PAYMENT_PENDING
- PAYMENT_VERIFIED_TESTNET
- RECEIPT_CREATED
- EVIDENCE_CREATED
- PUBLIC_VERIFY_AVAILABLE
- ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- ACCESS_EXPIRED
- ACCESS_REVOKED
- ACCESS_DISPUTED
- REVIEW_REQUIRED
- STALE
- UNAVAILABLE

The core rule remains:

No receipt, no evidence, and no access unlock before backend-verified payment state.

---

## Privacy Layer

SafeGate privacy direction is minimum disclosure.

Public verify should confirm what happened without exposing sensitive identifiers.

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
- internal backend logs
- stack traces
- environment variables

Public verify may expose safe fields:

- verified
- publicSafe
- minimumDisclosure
- paymentState label
- accessState label
- receiptStatus
- evidenceStatus
- freshnessStatus
- integrityStatus
- privateDataIncluded
- secretsIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded

---

## Web3 Layer

SafeGate is Pi-first, not Pi-only.

The current evidence path is Pi-first because Pi provides:

- Pi Browser
- Pi authentication
- payments scope
- Pi Wallet
- Pi payment flow
- app ecosystem direction
- merchant utility potential
- identity-oriented environment

The long-term Web3 direction can remain chain-agnostic.

However:

Pi-first does not mean all Pi production claims are complete.

Chain-agnostic direction does not mean all chains are integrated.

V12 should preserve this boundary.

---

## Post-Payment Trust Model

A SafeGate trust record should answer:

- Was an invoice created?
- Did access start locked?
- Was payment backend verified?
- Was receipt created after verified state?
- Was evidence created after verified state?
- Was public verify available?
- Did public verify avoid sensitive identifiers?
- Did access unlock only after backend-verified receipt?
- Is the record still fresh?
- Is the record still active?
- Is integrity valid if implemented?
- Is there a dispute or review-required state?

---

## Public Verify As Trust Interface

Public verify should become the public-safe trust interface.

It should allow a reviewer, merchant, or user to check the outcome without seeing private data.

Possible future public verify response:

- verified: true
- publicSafe: true
- minimumDisclosure: true
- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- receiptStatus: RECEIPT_CREATED
- evidenceStatus: EVIDENCE_CREATED
- freshnessStatus: ACTIVE
- integrityStatus: INTEGRITY_VALID
- privateDataIncluded: false
- secretsIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false

Boundary:

Freshness and integrity are not fully implemented claims yet.

---

## Receipt / Evidence Integrity Direction

V12 should connect with the receipt integrity plan.

Future receipt/evidence integrity may include:

- canonical receipt payload
- canonical evidence payload
- receipt hash
- evidence hash
- combined trust hash
- server-side HMAC or signature
- issuedAt
- verifiedAt
- optional expiresAt
- key versioning
- integrityStatus
- tamper-detection behavior
- public-safe integrity metadata

Boundary:

SafeGate does not currently claim tamper-proof infrastructure.

SafeGate does not currently claim independent cryptographic verification.

---

## Freshness Direction

V12 should connect with public verify freshness.

Public verify should eventually distinguish:

- ACTIVE
- EXPIRED
- REVOKED
- DISPUTED
- STALE
- UNAVAILABLE
- REVIEW_REQUIRED
- FRESHNESS_NOT_IMPLEMENTED

A historical verification should not automatically mean current active trust forever.

Boundary:

SafeGate does not currently claim completed freshness implementation.

---

## Dispute / Review Direction

Real trust systems need dispute and review states.

Future states may include:

- ACCESS_DISPUTED
- RECEIPT_REVIEW_REQUIRED
- EVIDENCE_REVIEW_REQUIRED
- PUBLIC_VERIFY_REVIEW_REQUIRED
- PAYMENT_REVIEW_REQUIRED
- TRUST_RECORD_REVIEW_REQUIRED

These states should prevent overclaiming clean active trust when the backend state is ambiguous.

---

## Merchant Trust Direction

For merchants, V12 should help answer:

What happened after payment?

Merchant value:

- clearer post-payment records
- receipt/evidence trail
- access-state clarity
- public-safe proof
- support context
- dispute context
- reduced screenshot dependence
- structured reviewer evidence

SafeGate should remain clear:

It does not process payments.

It verifies the outcome after payment.

---

## AI Agent Readiness Direction

V12 can prepare a future AI-agent-readable trust layer.

Possible future AI-agent components:

- machine-readable trust states
- OpenAPI specification
- llms.txt
- agent integration guide
- privacy-first receipt verification
- agent-readable public verify responses

But AI Agent Readiness should not outrun backend hardening.

AI agents increase:

- request volume
- replay risk
- automation abuse
- callback pressure
- API misuse risk
- rate-limit requirements

Therefore backend behavior, idempotency, replay protection, safe errors, integrity, freshness, and rate limiting must come first.

---

## V12 Data Minimization Direction

SafeGate should follow data minimization.

Store and expose only what is needed for trust-state verification.

Avoid unnecessary exposure of:

- raw payment identifiers
- wallet addresses
- private user identifiers
- customer data
- internal API responses
- raw logs
- secrets
- access tokens

Public verification should remain minimum-disclosure by design.

---

## V12 Chain-Agnostic Direction

Long-term SafeGate may support a chain-agnostic trust model.

Potential direction:

- PaymentAdapter abstraction
- PiAdapter
- future EVM adapter
- future Stellar/Soroban adapter
- future multi-chain receipt mapping
- shared trust-state model
- shared public verify model
- shared privacy boundary

Boundary:

The current evidence is Pi Testnet / Sandbox-oriented.

Multi-chain production support is not claimed.

---

## V12 Reviewer Questions

A reviewer should ask:

1. Are trust states clearly defined?
2. Can frontend callback unlock access?
3. Can receipt/evidence be created before backend verification?
4. Does public verify expose sensitive data?
5. Can public verify become stale?
6. Is receipt integrity implemented or only planned?
7. Can duplicate/replay events create new trust outcomes?
8. What happens if backend state is unavailable?
9. How would AI agents safely read trust states?
10. Which parts are current evidence versus future architecture?

---

## Relationship To V9.1 Hardening

V12 should not bypass V9.1.

V9.1 hardening remains the active engineering layer.

V12 depends on V9.1 work such as:

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- real paymentId replay handling
- real txid replay handling
- paymentId mismatch behavior
- public verify mismatched pair behavior
- Pi API timeout behavior
- Supabase failure behavior
- receipt/evidence integrity implementation
- public verify freshness implementation
- rate limiting / abuse resistance
- deeper safe error response validation

---

## Safe Public Language

SafeGate may say:

- V12 trust-state / privacy / Web3 direction is documented
- SafeGate is Pi-first, not Pi-only
- SafeGate’s current evidence includes controlled Pi Testnet payment-spine pass
- SafeGate’s current evidence includes V9.1 safe negative backend validation pass
- V12 is an architecture direction
- production readiness is not claimed
- formal audit is not claimed
- complete privacy protocol is not claimed

SafeGate should not say:

- V12 is production-ready
- complete privacy protocol is finished
- all chains are integrated
- formal audit passed
- tamper-proof infrastructure completed
- independent cryptographic verification completed
- AI-agent readiness is complete
- all duplicate/replay risks are solved

---

## Final V12 Position

V12 is SafeGate’s trust-state, privacy, and Web3 architecture direction.

It builds on a meaningful current evidence baseline:

- Controlled Pi Testnet Payment Spine: PASSED
- V9.1 Safe Negative Backend Validation: PASSED

V12 should remain disciplined:

- trust states first
- minimum disclosure always
- backend hardening before AI agents
- public verify freshness before strong current-trust claims
- receipt integrity before tamper-evidence claims
- pilot readiness before production claims

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
