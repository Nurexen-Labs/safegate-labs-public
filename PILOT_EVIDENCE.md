# SafeGate Pilot Evidence

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Current Evidence Status

| Evidence Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Public-Safe Evidence Pack | Available |
| Pilot Readiness | Open |
| Controlled Pilot Execution | Not yet claimed complete |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |

---

## Evidence Boundary

This file tracks public-safe SafeGate pilot evidence.

This file does not claim:

- production readiness
- Pi Mainnet settlement
- official Pi Core Team partnership
- custodial payment processing
- regulatory approval
- formal third-party audit
- security certification
- completed commercial pilot
- tamper-proof infrastructure
- independent cryptographic verification

---

## Evidence Item 1 — Controlled Pi Testnet Payment Spine

**Status:** PASSED  
**Environment:** Pi Browser / Pi Sandbox / Testnet-oriented controlled flow  
**Public Phase:** Pilot Readiness  
**Production Claim:** No  

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

Observed controlled flow:

1. V9 invoice created
2. initial access state remained locked
3. Pi Browser authentication succeeded
4. username + payments scope succeeded
5. Pi wallet payment succeeded in Sandbox / Testnet context
6. backend approval / completion flow executed
7. backend-verified payment state reached
8. receipt and evidence generated after verified state
9. public verify confirmed minimum-disclosure result
10. access unlocked only after backend-verified receipt

Public-safe outcome:

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

## Evidence Item 2 — V9.1 Safe Negative Backend Validation

**Status:** PASSED  
**Validation Type:** Safe negative backend behavior validation  
**Visible Checks:** 18  
**Passed:** 18  
**Failed:** 0  
**Warnings:** 0  
**Production Claim:** No  

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

This validation does not prove real duplicate approve/complete behavior, real paymentId/txid replay handling, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## Evidence Item 3 — Public-Safe Evidence Pack

**Status:** Available

The public-safe evidence pack defines how SafeGate evidence should be shared without exposing sensitive details.

Related document:

SAFEGATE_PUBLIC_SAFE_PAYMENT_SPINE_EVIDENCE_PACK.md

Public-safe evidence may show:

- verified: true
- minimumDisclosure: true
- publicSafe: true
- privateDataIncluded: false
- secretsIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

Public evidence should not expose:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- service role key
- raw Pi API response
- private customer data
- private wallet data

---

## Evidence Item 4 — External Technical Review Surface

**Status:** Available

SafeGate includes an external technical review note for software/security reviewers.

Related document:

SAFEGATE_EXTERNAL_TECHNICAL_REVIEW_NOTE.md

The most useful review question is:

Where would this architecture break first if it moved from controlled Testnet evidence to a real pilot environment?

Review focus:

- backend state transitions
- duplicate callback behavior
- idempotency
- replay resistance
- receipt/evidence guard
- public verify minimum disclosure
- public verify freshness
- Supabase / durable-state failure behavior
- safe error responses
- receipt/evidence integrity direction

---

## Evidence Item 5 — Controlled Pilot Checklist

**Status:** Available

SafeGate includes a controlled pilot execution checklist.

Related document:

SAFEGATE_CONTROLLED_PILOT_EXECUTION_CHECKLIST.md

The checklist defines:

- pilot boundaries
- stop conditions
- public-safe evidence rules
- reviewer/tester guidance
- merchant-facing boundaries
- backend behavior requirements
- remaining hardening targets

Controlled pilot execution is not yet claimed complete.

---

## Evidence Item 6 — Backend Behavior Matrix

**Status:** Available

Related document:

SAFEGATE_BACKEND_BEHAVIOR_EVIDENCE_MATRIX.md

The matrix defines expected backend behavior for cases such as:

- valid invoice create
- valid approve
- duplicate approve
- valid complete
- duplicate complete
- missing paymentId
- missing txid
- paymentId mismatch
- receipt before payment verified
- duplicate receipt/evidence request
- unknown public verify pair
- backend internal error

Current boundary:

Selected safe negative cases passed in the live V9.1 validation.

Deeper duplicate/replay/failure tests remain open.

---

## Evidence Item 7 — AI Review Risk Consensus

**Status:** Available

Related document:

SAFEGATE_AI_REVIEW_RISK_CONSENSUS_V9.md

Common strengths identified:

- backend-verified access unlock
- payment/trust layer separation
- receipt/evidence guard
- minimum-disclosure public verify
- disciplined claim boundaries

Common risks identified:

- duplicate callback behavior
- idempotency
- replay protection
- timeout / pending states
- database failure behavior
- backend behavior external reviewability
- receipt/evidence cryptographic integrity
- Pi verification depth
- public verify freshness
- rate limiting / API abuse
- merchant-facing simplicity

This is advisory risk tracking.

It is not a formal audit.

---

## Evidence Item 8 — Receipt Integrity Direction

**Status:** Planned / documented direction

Related document:

SAFEGATE_RECEIPT_INTEGRITY_PLAN.md

Current direction includes:

- canonical receipt payload
- canonical evidence summary payload
- receipt hash
- evidence hash
- server-side HMAC or signature direction
- issuedAt timestamp
- optional expiresAt
- immutable receipt/evidence binding
- tamper-detection behavior
- public-safe integrity metadata

Boundary:

SafeGate does not currently claim tamper-proof infrastructure.

SafeGate does not currently claim independent cryptographic verification.

---

## Evidence Item 9 — Pi Verification Depth

**Status:** Documented direction

Related document:

SAFEGATE_PI_VERIFICATION_DEPTH_NOTE.md

Backend verification should map to concrete checks such as:

- invoice exists
- invoice is still eligible
- paymentId exists
- paymentId belongs to invoice
- amount matches invoice
- payment environment is correct
- payment state is completed/verified
- txid exists when required
- txid is not reused
- receipt/evidence is not duplicated
- access state remains locked before verification

Boundary:

Controlled Pi Testnet payment-spine pass does not equal Pi Mainnet settlement.

---

## Evidence Item 10 — Public Verify Freshness

**Status:** Documented direction

Related document:

SAFEGATE_PUBLIC_VERIFY_FRESHNESS_POLICY.md

Future public verify should distinguish states such as:

- active
- expired
- revoked
- disputed
- stale
- unavailable
- review-required

Boundary:

A record that was verified at one time should not automatically imply access remains active forever.

---

## Evidence Item 11 — Formal State Machine

**Status:** Documented direction

Related document:

SAFEGATE_FORMAL_STATE_MACHINE_TABLE.md

Core rule:

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

---

## Evidence Item 12 — Merchant Explanation

**Status:** Available

Related document:

SAFEGATE_MERCHANT_EXPLANATION.md

Merchant-facing value:

- clearer post-payment records
- safer access unlock logic
- receipt/evidence trail
- public-safe proof
- support/dispute context
- less reliance on screenshots or manual claims
- structured trust state after payment

---

## Evidence Review Order

Recommended review order:

1. Main Review Hub
2. V9 Payment Spine Evidence
3. V9.1 Backend Behavior Validation
4. Current Review Package
5. Controlled Pi Testnet Payment Spine Pass Note
6. V9.1 Safe Negative Backend Validation Pass Note
7. Public-Safe Evidence Pack
8. External Technical Review Note
9. Controlled Pilot Execution Checklist
10. AI Review Risk Consensus
11. Backend Behavior Evidence Matrix
12. Receipt Integrity Plan
13. Pi Verification Depth Note
14. Public Verify Freshness Policy
15. Formal State Machine Table
16. Merchant Explanation

---

## Live Evidence Links

| Evidence | Link |
|---|---|
| Main Review Hub | https://www.safegatelabs.xyz |
| V9 Payment Spine Evidence | https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html |
| V9.1 Backend Behavior Validation | https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html |
| Controlled Checkout | https://www.safegatelabs.xyz/safegate-checkout.html |
| Pi Auth Diagnostic | https://www.safegatelabs.xyz/v9-pi-auth-diagnostic.html |
| Pilot Review Index | https://www.safegatelabs.xyz/pilot-review-index.html |
| Pilot Readiness | https://www.safegatelabs.xyz/pilot-readiness.html |
| Pilot Evidence Intake | https://www.safegatelabs.xyz/pilot-evidence-intake.html |

---

## Remaining Evidence Gaps

Remaining evidence gaps before stronger pilot claims:

- duplicate approve test
- duplicate complete test
- duplicate receipt/evidence test
- real paymentId replay behavior
- real txid replay behavior
- paymentId mismatch behavior
- public verify mismatched receipt/evidence pair
- Pi API timeout behavior
- Supabase write failure behavior
- durable-state recovery behavior
- receipt/evidence integrity implementation
- public verify freshness implementation
- rate limiting / abuse resistance
- deeper safe error response validation
- external reviewer feedback capture

---

## Final Evidence Statement

SafeGate has passed:

- Controlled Pi Testnet Payment Spine
- V9.1 Safe Negative Backend Validation

SafeGate has available:

- public-safe evidence pack
- external technical review note
- controlled pilot checklist
- backend behavior matrix
- AI review risk consensus
- receipt integrity plan
- Pi verification depth note
- public verify freshness policy
- formal state machine table
- merchant explanation

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
