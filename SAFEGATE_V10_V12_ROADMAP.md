# SafeGate V10 / V12 Roadmap

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This roadmap explains how SafeGate should move from the current V9 / V9.1 evidence layer toward V10 controlled pilot preparation and V12 trust-state / privacy / Web3 architecture.

This is not a production-readiness claim.

This is not a Pi Mainnet settlement claim.

This is not an official Pi partnership claim.

This is not a formal audit claim.

This is a roadmap document.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Pilot Readiness | Open |
| V9.1 Backend + Integrity Hardening | Active direction |
| V10 Controlled Pilot Preparation | Planned |
| V12 Trust-State / Privacy / Web3 Architecture | Preview / direction |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |

---

## Current Evidence Baseline

SafeGate has passed two major controlled evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

These two milestones form the current baseline for V10 and V12 planning.

They show:

- controlled Pi Browser payment-spine evidence
- backend-verified payment-state direction
- receipt/evidence generation after verified payment state
- access unlock after backend-verified receipt
- selected negative backend cases rejected safely
- public verify minimum-disclosure direction
- public-safe evidence packaging
- clear production and audit boundaries

They do not prove production readiness.

They do not prove formal audit completion.

They do not prove Pi Mainnet settlement.

---

## Evidence Milestone 1 — Controlled Pi Testnet Payment Spine

**Status:** PASSED

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

## Phase V9.1 — Backend + Integrity Hardening

V9.1 is the active engineering direction before stronger pilot claims.

V9.1 focuses on:

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- idempotency
- replay resistance
- paymentId mismatch behavior
- txid mismatch behavior
- public verify mismatched pair behavior
- Pi API timeout behavior
- Supabase write failure behavior
- durable-state recovery behavior
- receipt/evidence integrity
- public verify freshness
- rate limiting / abuse resistance
- deeper safe error response validation

Current V9.1 result:

- selected safe negative backend validation passed
- deeper duplicate/replay/failure coverage remains open

---

## Phase V10 — Controlled Pilot Preparation

V10 should be the controlled pilot preparation layer.

V10 should not be a production launch.

V10 should focus on:

- limited reviewer/tester path
- controlled merchant-style scenario
- public-safe evidence capture
- redaction rules
- stop conditions
- reviewer feedback
- merchant feedback
- backend behavior repeat tests
- claim boundary discipline
- pilot execution checklist
- review package simplification

V10 should only use safe language:

- controlled pilot planning
- Pilot Readiness
- limited review
- technical feedback
- public-safe evidence

V10 should not claim:

- production readiness
- completed commercial pilot
- Pi Mainnet settlement
- official Pi partnership
- formal audit
- enterprise security certification

---

## V10 Minimum Readiness Checklist

Before stronger V10 pilot positioning, SafeGate should have:

- Main Review Hub live
- V9 Payment Spine Evidence live
- V9.1 Backend Behavior Validation live
- Current Review Package complete
- Simple Reviewer Brief complete
- Final Hackathon Submission Pack complete
- Architecture document complete
- Security Boundaries complete
- Backend Behavior Matrix complete
- Formal State Machine Table complete
- Receipt Integrity Plan complete
- Public Verify Freshness Policy complete
- Merchant Explanation complete
- Controlled Pilot Checklist complete
- Public-Safe Evidence Pack complete
- Pilot Evidence Template complete
- README / LINKS / ROADMAP aligned

Current status:

Most public review documents are now synchronized with the V9.1 validation result.

---

## V10 Main Goal

The main V10 goal is not to add hype.

The main V10 goal is to make SafeGate understandable and reviewable.

A reviewer should quickly understand:

- what SafeGate is
- what SafeGate is not
- what has passed
- what has not passed
- what the strongest technical claim is
- where the risks remain
- what should be tested next
- how pilot evidence should be captured safely

---

## V10 Strongest Claim

The strongest V10-safe claim is:

SafeGate has passed a controlled Pi Testnet payment-spine flow and a live V9.1 safe negative backend validation for selected public-safe cases.

This means SafeGate can show:

- backend-verified payment-state direction
- receipt/evidence after verified state
- access unlock after backend-verified receipt
- invoice creation keeps access locked
- receipt/evidence before verified payment is rejected
- missing inputs are rejected
- unknown public verify pair does not return active verified trust
- selected responses do not show obvious secret/internal leak terms

---

## V10 Remaining Technical Targets

Before stronger controlled pilot claims, SafeGate should continue with:

- duplicate approve test
- duplicate complete test
- duplicate receipt/evidence test
- real paymentId replay behavior
- real txid replay behavior
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

## Phase V11 — Security Logic Preview

V11 remains a security logic preview layer.

V11 can show or document:

- state transition guard
- idempotency guard
- replay guard
- fail-secure behavior
- refund/dispute state preview
- hash/timestamp proof preview
- anti-enumeration ID preview
- audit log preview

Boundary:

V11 is not a formal audit.

V11 is not a penetration test.

V11 is not production security certification.

V11 should support V9.1 hardening, not replace it.

---

## Phase V12 — Trust-State / Privacy / Web3 Architecture

V12 is the longer-term trust-state, privacy, and Web3 architecture direction.

V12 should explain how SafeGate can evolve beyond a single Pi-first flow into a broader post-payment trust architecture.

Possible V12 areas:

- trust-state mapping
- privacy-aware verification
- merchant-side proof
- minimum-disclosure public verify
- receipt/evidence integrity
- public verify freshness
- audit-aware outcome records
- chain-agnostic future direction
- Web3 trust-state architecture
- privacy-preserving reporting direction

Boundary:

V12 is an architecture preview.

V12 is not a completed privacy protocol.

V12 is not a production compliance layer.

V12 is not an audited enterprise trust infrastructure.

---

## V12 Trust-State Direction

V12 should treat payment as a trigger and trust as the product.

Possible trust states:

- invoice created
- payment pending
- payment backend verified
- receipt issued
- evidence issued
- public verify available
- access unlocked
- access expired
- access revoked
- access disputed
- review required
- stale
- unavailable

This lets SafeGate become more than a checkout page.

It becomes a structured outcome layer.

---

## V12 Privacy Direction

V12 privacy direction should preserve minimum disclosure.

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

Public verify may expose safe trust-state fields:

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

## V12 Web3 Direction

V12 can support a broader Web3 trust model:

- Pi-first payment-triggered trust
- chain-agnostic future design
- merchant proof
- public-safe verification
- receipt/evidence records
- trust-state transitions
- privacy-preserving outcome proof
- future multi-chain adapter direction

Boundary:

SafeGate should remain clear:

Pi-first does not mean Pi-only.

Chain-agnostic future does not mean all chains are already integrated.

---

## Future AI Agent Readiness

AI Agent Readiness should come after backend hardening.

Future AI-agent direction may include:

- machine-readable trust states
- OpenAPI specification
- llms.txt
- agent integration guide
- privacy-first receipt verification
- agent-readable public verify responses

However, AI agents increase:

- request volume
- replay risk
- automation abuse
- callback pressure
- API misuse risk
- rate-limit requirements

Therefore, backend behavior, idempotency, replay protection, receipt integrity, freshness, safe errors, and rate limiting should come first.

---

## Recommended Development Order

SafeGate should move in this order:

1. preserve controlled V9 evidence
2. preserve V9.1 validation result
3. keep public docs synchronized
4. strengthen backend behavior
5. test duplicate approve / complete
6. test duplicate receipt/evidence
7. test replay and mismatch behavior
8. test timeout and durable-state failure behavior
9. implement receipt/evidence integrity
10. implement public verify freshness
11. collect external reviewer feedback
12. prepare controlled pilot path
13. only then consider stronger pilot claims
14. later, define AI-agent readiness

---

## Safe Public Language

SafeGate may say:

- V10 submission package is ready for review positioning
- Controlled Pi Testnet Payment Spine passed
- V9.1 Safe Negative Backend Validation passed
- Pilot Readiness is open
- V9.1 backend hardening remains active
- V12 trust-state / privacy / Web3 architecture is a direction
- production readiness is not claimed
- formal audit is not claimed

SafeGate should not say:

- production ready
- fully secure
- formally audited
- official Pi partnership
- Pi Mainnet settlement completed
- commercial pilot completed
- tamper-proof infrastructure completed
- independent cryptographic verification completed
- all duplicate/replay/failure risks solved
- complete privacy protocol finished

---

## Final V10 / V12 Roadmap Position

SafeGate has crossed an important evidence threshold.

SafeGate has passed:

- Controlled Pi Testnet Payment Spine
- V9.1 Safe Negative Backend Validation

The correct next movement is:

- V10: controlled pilot preparation and review package discipline
- V11: security logic preview supporting hardening
- V12: trust-state / privacy / Web3 architecture direction
- future: AI Agent Readiness after backend hardening

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
