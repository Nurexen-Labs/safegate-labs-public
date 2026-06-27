# SafeGate External Technical Review Note

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This note is for external technical reviewers.

The goal is not to claim production readiness.

The goal is to make SafeGate’s current architecture, evidence, boundaries, and remaining risks easy to review.

The most useful review question is:

Where would this architecture break first if it moved from controlled Testnet evidence to a real pilot environment?

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

## Main Technical Result

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The demonstrated flow:

1. V9 invoice creation
2. initial access state remains locked
3. Pi Browser authentication succeeds
4. username + payments scope succeeds
5. Pi wallet payment succeeds in Sandbox / Testnet context
6. backend approval / completion flow executes
7. backend-verified payment state is reached
8. receipt and evidence are generated only after verified payment state
9. public verify confirms a minimum-disclosure record
10. access unlocks only after backend-verified receipt

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

---

## V9.1 Safe Negative Backend Validation

SafeGate also completed a live V9.1 safe negative backend behavior validation.

Live validation page:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Public-safe pass note:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

Observed validation result:

- total visible checks: 18
- passed checks: 18
- failed checks: 0
- warning checks: 0

Confirmed public-safe behavior:

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

## Core Architecture Principle

SafeGate must not unlock access from frontend callback alone.

Access unlock should require:

1. backend-verified payment state
2. guarded receipt/evidence generation
3. public verification path becoming available
4. final access state transition after verified receipt/evidence

Core rule:

No receipt, no evidence, and no access unlock before backend-verified payment state.

---

## What To Review First

Recommended external technical review order:

1. controlled Pi Testnet payment-spine result
2. V9.1 safe negative backend validation result
3. backend state transition strictness
4. approve / complete idempotency
5. duplicate callback handling
6. replay resistance
7. receipt/evidence creation guard
8. public verify minimum-disclosure behavior
9. public verify freshness / stale-data handling
10. Supabase / durable-state failure behavior
11. safe error model
12. receipt/evidence integrity direction
13. rate limiting and abuse resistance
14. pilot-readiness boundary discipline

---

## Live Review Links

| Item | Link |
|---|---|
| Main Review Hub | https://www.safegatelabs.xyz |
| V9 Payment Spine Evidence | https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html |
| V9.1 Backend Behavior Validation | https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html |
| Controlled Checkout | https://www.safegatelabs.xyz/safegate-checkout.html |
| Pi Auth Diagnostic | https://www.safegatelabs.xyz/v9-pi-auth-diagnostic.html |
| Pilot Review Index | https://www.safegatelabs.xyz/pilot-review-index.html |
| Pilot Readiness | https://www.safegatelabs.xyz/pilot-readiness.html |
| Public Repository | https://github.com/Nurexen-Labs/safegate-labs-public |

---

## Key Public Documents

| Document | Purpose |
|---|---|
| README.md | Public repository overview |
| LINKS.md | Full public link index and reviewer order |
| SAFEGATE_CURRENT_REVIEW_PACKAGE.md | One-file current review package |
| SAFEGATE_CONTROLLED_PI_TESTNET_PAYMENT_SPINE_PASSED.md | Public-safe controlled Pi Testnet pass note |
| SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md | Public-safe V9.1 backend validation pass note |
| SAFEGATE_PUBLIC_SAFE_PAYMENT_SPINE_EVIDENCE_PACK.md | Public-safe evidence packaging rules |
| SAFEGATE_AI_REVIEW_RISK_CONSENSUS_V9.md | AI-assisted technical risk consensus |
| SAFEGATE_V9_1_BACKEND_HARDENING_PLAN.md | Backend + integrity hardening plan |
| SAFEGATE_BACKEND_BEHAVIOR_EVIDENCE_MATRIX.md | Backend behavior edge-case matrix |
| SAFEGATE_RECEIPT_INTEGRITY_PLAN.md | Receipt/evidence integrity direction |
| SAFEGATE_PI_VERIFICATION_DEPTH_NOTE.md | Pi verification depth clarification |
| SAFEGATE_PUBLIC_VERIFY_FRESHNESS_POLICY.md | Public verify freshness/stale-data policy |
| SAFEGATE_FORMAL_STATE_MACHINE_TABLE.md | State transition planning table |
| SAFEGATE_MERCHANT_EXPLANATION.md | Merchant-facing explanation |
| SAFEGATE_CONTROLLED_PILOT_EXECUTION_CHECKLIST.md | Controlled pilot execution checklist |

---

## Review Focus: Backend Verification

The phrase backend verified should be treated as an engineering claim, not a slogan.

A stronger backend verification model should check:

- invoice exists
- invoice is still eligible
- paymentId exists
- paymentId belongs to the expected invoice
- payment amount matches invoice
- payment environment is correct
- payment state is completed or verified
- txid exists when required
- txid is not reused
- paymentId is not reused across invoices
- receipt/evidence is not duplicated
- access state remains locked before verification
- illegal transitions are rejected
- ambiguous responses fail secure

Controlled Pi Testnet payment-spine pass does not equal Pi Mainnet settlement.

---

## Review Focus: State Machine

SafeGate should define legal and illegal transitions clearly.

Important legal transition direction:

- invoice created
- payment pending
- payment backend verified
- receipt/evidence created
- public verify available
- access unlocked

Important illegal transition examples:

- invoice created → access unlocked
- frontend callback → access unlocked
- payment pending → receipt issued
- missing paymentId → receipt issued
- missing invoiceId → evidence issued
- unknown public verify pair → active verified trust
- failed payment → access unlocked
- expired invoice → access unlocked
- duplicate replay → new receipt/evidence generated

The first V9.1 safe negative validation confirmed selected illegal paths are rejected safely.

---

## Review Focus: Idempotency And Replay

The next hard technical questions are:

- What happens if approve callback is repeated?
- What happens if complete callback is repeated?
- What happens if receipt/evidence is requested twice?
- What happens if the same paymentId is reused?
- What happens if the same txid is reused?
- What happens if paymentId belongs to another invoice?
- What happens if a stale invoice is completed later?
- What happens if public verify receives mismatched receipt/evidence IDs?

Expected direction:

Duplicate or replayed events should return stable, safe, idempotent behavior.

They should not create a second receipt.

They should not create a second evidence record.

They should not unlock access from an invalid or mismatched state.

---

## Review Focus: Public Verify

Public verify should remain minimum-disclosure.

It should confirm trust outcome without exposing sensitive payment data.

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

The V9.1 validation scanned selected public-safe responses for obvious secret/internal leak terms and passed.

Future public verify should also address freshness.

A record that was once verified may later become expired, revoked, disputed, stale, unavailable, or review-required.

---

## Review Focus: Durable State

SafeGate currently uses durable-state direction.

The next important review area is failure behavior:

- What happens if database write fails after payment verification?
- What happens if receipt write succeeds but evidence write fails?
- What happens if evidence write succeeds but access unlock fails?
- What happens if public verify reads stale state?
- What happens if backend returns partial success?
- What happens if Supabase is temporarily unavailable?

Expected direction:

The system should fail secure.

No access unlock should occur from ambiguous or partial state.

---

## Review Focus: Receipt Integrity

Minimum-disclosure is privacy-oriented.

It is not the same as tamper-evident integrity.

Future receipt/evidence integrity should include:

- canonical receipt payload
- canonical evidence payload
- receipt hash
- evidence hash
- server-side HMAC or signature direction
- issuedAt
- optional expiresAt
- immutable receipt/evidence binding
- tamper-detection behavior
- public-safe integrity metadata

SafeGate does not currently claim tamper-proof infrastructure.

SafeGate does not currently claim independent cryptographic verification.

---

## Review Focus: Safe Errors

SafeGate should avoid leaking internal details in error responses.

Error responses should not expose:

- environment variables
- service role keys
- stack traces
- raw database errors
- raw Pi API responses
- access tokens
- internal table structure
- private identifiers
- wallet details
- customer data

The first V9.1 validation found no obvious secret/internal leak terms in selected responses.

This should be expanded in deeper backend testing.

---

## Current Strengths

Current strengths:

- clear post-payment trust positioning
- payment and trust layer separation
- controlled Pi Testnet payment-spine pass
- access unlock after backend-verified receipt
- receipt/evidence guard direction
- minimum-disclosure public verify
- live safe negative backend validation pass
- disciplined public boundaries
- public technical documentation package
- risk consensus captured from multiple AI reviews
- backend hardening direction is explicit

---

## Current Open Risks

Open risks:

- real duplicate approve behavior not yet fully proven
- real duplicate complete behavior not yet fully proven
- real paymentId replay behavior not yet fully proven
- real txid replay behavior not yet fully proven
- paymentId mismatch behavior needs deeper validation
- Pi API timeout behavior needs testing
- Supabase failure behavior needs testing
- receipt/evidence cryptographic integrity is still a plan
- public verify freshness needs implementation detail
- rate limiting and abuse resistance need deeper definition
- formal audit has not been completed

---

## Safe Public Language

SafeGate may say:

- controlled Pi Testnet payment-spine passed
- V9.1 safe negative backend validation passed
- invoice creation kept access locked
- receipt/evidence before verified payment was rejected safely
- unknown public verify pair did not return active verified trust
- public-safe responses did not show obvious secret/internal leak terms
- SafeGate is in Pilot Readiness
- SafeGate is not production-ready
- SafeGate is not formally audited

SafeGate should not say:

- production backend is secure
- all backend risks are solved
- all replay risks are solved
- all duplicate callback risks are solved
- tamper-proof infrastructure is complete
- independent cryptographic verification is complete
- official Pi partnership exists
- Pi Mainnet settlement is complete
- formal audit passed

---

## Suggested Reviewer Comment Format

A useful external review comment should answer:

1. What is the strongest part of this architecture?
2. Where would it break first?
3. Which backend state transition is most dangerous?
4. Which duplicate/replay case should be tested next?
5. Which public verify detail is unclear?
6. Which claim should be softened?
7. What should be implemented before a controlled pilot?

---

## Final Review Statement

SafeGate is no longer only a static concept.

It has passed a controlled Pi Testnet payment-spine flow.

It has passed a live V9.1 safe negative backend validation for selected public-safe cases.

It has not completed production hardening.

It has not completed a formal security audit.

The most useful next review is backend behavior review under duplicate, replay, mismatch, timeout, database failure, public verify freshness, and receipt integrity scenarios.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
