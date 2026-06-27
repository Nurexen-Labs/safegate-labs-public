# SafeGate Roadmap

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Current Roadmap Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Pilot Readiness | Open |
| V9.1 Backend + Integrity Hardening | Active direction |
| Controlled Pilot Execution | Planned |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |

---

## Current Phase

SafeGate is currently in:

**Pilot Readiness + V9.1 Backend / Integrity Hardening**

This means SafeGate has passed important controlled evidence steps, but is not yet claiming production readiness.

---

## Completed Evidence Milestones

### Controlled Pi Testnet Payment Spine — PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow demonstrated:

- V9 invoice creation
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend approval / completion flow
- backend-verified payment state
- receipt/evidence generation after verified state
- minimum-disclosure public verification
- access unlock only after backend-verified receipt

Observed result:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- public verify: confirmed
- verified: true
- minimumDisclosure: true
- publicSafe: true
- privateDataIncluded: false
- secretsIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

Public-safe pass note:

SAFEGATE_CONTROLLED_PI_TESTNET_PAYMENT_SPINE_PASSED.md

Live evidence page:

https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html

---

### V9.1 Safe Negative Backend Validation — PASSED

SafeGate completed a live V9.1 safe negative backend behavior validation.

Live validation page:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Public-safe pass note:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

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

Boundary:

This validation does not prove real duplicate approve/complete behavior, real paymentId/txid replay handling, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## Roadmap Phases

## Phase V8 — Controlled MVP Evidence Pack

Status:

**Completed / archived as controlled evidence**

V8 established the first public-safe MVP evidence surface:

- Invoice Lite
- Multi Receipt / Evidence
- Durable Evidence Write
- Public Receipt Verify
- Merchant Console
- Tester Evidence Intake
- Start Here landing
- Final MVP Review Index
- Technical Architecture
- Hash Proof Preview
- Evidence Ledger Preview

V8 did not claim production readiness.

---

## Phase V9 — Controlled Pi Testnet Payment Spine

Status:

**Passed controlled Pi Testnet payment-spine execution**

V9 focused on the Pi payment spine:

- Pi Browser authentication
- payments scope
- Pi Sandbox / Testnet payment flow
- backend approval / completion direction
- backend-verified payment state
- guarded receipt/evidence
- minimum-disclosure public verify
- access unlock after backend-verified receipt

Main V9 result:

**Controlled Pi Testnet Payment Spine: PASSED**

V9 did not claim Pi Mainnet settlement or production readiness.

---

## Phase V9.1 — Backend + Integrity Hardening

Status:

**Active direction**

V9.1 focuses on making backend behavior clearer, safer, and more externally reviewable.

First V9.1 result:

**V9.1 Safe Negative Backend Validation: PASSED**

Core V9.1 hardening targets:

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- idempotency
- replay resistance
- paymentId mismatch behavior
- txid mismatch behavior
- public verify mismatched pair behavior
- timeout / pending-state behavior
- Supabase / durable-state failure behavior
- safe error response model
- receipt/evidence integrity direction
- public verify freshness
- rate limiting / abuse resistance
- formal state machine clarity

V9.1 does not claim production readiness.

V9.1 does not claim formal security audit completion.

---

## Phase V10 — Controlled Pilot Preparation

Status:

**Planned**

V10 should prepare a limited controlled pilot path.

V10 should not claim public launch.

V10 should focus on:

- narrow controlled test scenario
- trusted reviewer / tester flow
- public-safe evidence capture
- redaction rules
- pilot stop conditions
- merchant explanation
- external reviewer feedback
- backend behavior repeat tests
- safer pilot checklist
- limited scope and clear boundaries

Before V10 can be treated as stronger controlled pilot execution, SafeGate should complete deeper V9.1 backend hardening tests.

---

## Phase V11 — Security Logic Preview

Status:

**Preview available**

V11 is a security logic preview.

It demonstrates browser-side logic concepts such as:

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

---

## Phase V12 — Trust-State / Privacy / Web3 Architecture Preview

Status:

**Preview available**

V12 explains the longer-term architecture direction:

- trust-state mapping
- privacy-aware verification
- post-payment evidence trails
- merchant-side proof
- audit-aware outcome verification
- chain-agnostic future direction
- Web3 trust-state architecture

Boundary:

V12 is an architecture preview.

It is not a completed privacy protocol.

It is not an audited enterprise trust infrastructure.

---

## Future Phase — AI Agent Readiness

Status:

**Future layer, not current priority**

A later SafeGate layer may focus on AI Agent Readiness:

- machine-readable trust states
- OpenAPI specification
- llms.txt
- agent integration guide
- privacy-first receipt verification
- agent-readable public verify responses

However, AI Agent Readiness should not outrun backend hardening.

AI agents increase:

- request volume
- replay risk
- automation abuse
- callback pressure
- API misuse risk
- rate-limit requirements

Therefore, backend behavior, idempotency, replay protection, integrity, and rate limiting should come first.

---

## Immediate Next Technical Targets

The next practical technical targets are:

1. duplicate approve test
2. duplicate complete test
3. duplicate receipt/evidence test
4. real paymentId replay behavior
5. real txid replay behavior
6. paymentId mismatch behavior
7. public verify mismatched receipt/evidence pair
8. Pi API timeout behavior
9. Supabase write failure behavior
10. durable-state recovery behavior
11. receipt/evidence integrity implementation
12. public verify freshness implementation
13. rate limiting / abuse resistance
14. safe error response expansion
15. controlled pilot reviewer flow

---

## Current Public Review Package

The current public review surface includes:

- Main Review Hub
- V9 Payment Spine Evidence
- V9.1 Backend Behavior Validation
- Current Review Package
- Controlled Pi Testnet Payment Spine Pass Note
- V9.1 Safe Negative Backend Validation Pass Note
- Public-Safe Evidence Pack
- External Technical Review Note
- Controlled Pilot Execution Checklist
- AI Review Risk Consensus
- V9.1 Backend Hardening Plan
- Backend Behavior Evidence Matrix
- Receipt Integrity Plan
- Pi Verification Depth Note
- Public Verify Freshness Policy
- Formal State Machine Table
- Merchant Explanation

---

## Safe Public Claim Boundaries

SafeGate may currently say:

- controlled Pi Testnet payment-spine passed
- V9.1 safe negative backend validation passed
- backend-verified payment state was reached in controlled Testnet flow
- access unlocked only after backend-verified receipt
- receipt/evidence was generated after verified state
- public verify used minimum-disclosure direction
- selected negative backend cases failed safely
- SafeGate is in Pilot Readiness
- production readiness is not claimed
- formal audit is not claimed

SafeGate should not say:

- production ready
- formally audited
- enterprise-grade secure
- tamper-proof
- all replay risks solved
- all duplicate callback risks solved
- Pi Mainnet settlement completed
- official Pi Core Team partnership
- regulatory approval completed
- commercial pilot completed
- independent cryptographic verification completed

---

## Roadmap Principle

SafeGate should move in this order:

1. preserve controlled evidence
2. keep claims boundary-safe
3. strengthen backend behavior
4. validate duplicate/replay/failure cases
5. improve receipt/evidence integrity
6. improve public verify freshness
7. collect external technical review
8. prepare narrow controlled pilot
9. only then consider stronger pilot or production claims

---

## Final Roadmap Position

SafeGate is no longer only a static idea.

SafeGate has passed a controlled Pi Testnet payment-spine flow.

SafeGate has passed a live V9.1 safe negative backend validation for selected public-safe cases.

SafeGate is in Pilot Readiness with V9.1 backend + integrity hardening as the active engineering direction.

SafeGate is not production-ready.

SafeGate is not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
