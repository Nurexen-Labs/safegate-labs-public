# SafeGate Public-Safe Payment Spine Evidence Pack

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This evidence pack defines what SafeGate can share publicly after controlled payment-spine and backend validation tests.

The purpose is to preserve useful technical evidence without exposing sensitive payment data, wallet data, identifiers, secrets, or internal infrastructure details.

This is a public-safe evidence pack.

It is not a production-readiness claim.

It is not a formal audit.

It is not a security certification.

---

## Current Evidence Status

| Evidence Area | Status |
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

## Evidence Item 1 — Controlled Pi Testnet Payment Spine

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow demonstrated:

1. V9 invoice creation
2. initial access state remained locked
3. Pi Browser authentication succeeded
4. username + payments scope succeeded
5. Pi wallet payment succeeded in Sandbox / Testnet context
6. backend approval / completion flow executed
7. backend-verified payment state reached
8. receipt and evidence generated after verified state
9. public verify confirmed minimum-disclosure result
10. access unlocked only after backend-verified receipt

Public-safe observed result:

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

SafeGate completed a live V9.1 safe negative backend behavior validation.

Live validation page:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Related document:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

Observed validation result:

- total visible checks: 18
- passed checks: 18
- failed checks: 0
- warning checks: 0

Confirmed selected public-safe negative behavior:

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

## Core Public-Safe Evidence Principle

SafeGate evidence should prove the minimum useful outcome without exposing sensitive data.

Public evidence should answer:

- Was backend-verified payment state reached?
- Did access remain locked before backend verification?
- Was receipt/evidence generated only after verified state?
- Did public verify confirm the outcome?
- Did public verify avoid exposing sensitive data?
- Did selected negative backend cases fail safely?

Public evidence should not expose:

- raw payment identifiers
- raw transaction identifiers
- wallet addresses
- recipient addresses
- access tokens
- service role keys
- raw Pi API responses
- internal logs
- private wallet data
- customer data

---

## Allowed Public-Safe Fields

The following fields may be shown publicly when they do not contain sensitive identifiers:

- paymentState
- accessState
- receiptStatus
- evidenceStatus
- verified
- publicSafe
- minimumDisclosure
- privateDataIncluded
- secretsIncluded
- rawPiApiResponseIncluded
- serviceRoleKeyIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded
- accessUnlocked
- visible check count
- passed check count
- failed check count
- warning check count
- general browser environment
- general test context
- pass/fail outcome

Example public-safe outcome:

- verified: true
- minimumDisclosure: true
- publicSafe: true
- privateDataIncluded: false
- secretsIncluded: false
- paymentIdIncluded: false
- txidIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

---

## Do Not Publish

Do not publish unredacted screenshots, logs, JSON responses, or browser console outputs containing:

- paymentId
- txid
- wallet address
- recipient address
- Pi account identifier
- user identifier
- app identifier
- access token
- auth token
- bearer token
- service role key
- Supabase secret
- database URL
- internal API key
- raw Pi API response
- raw backend logs
- internal stack trace
- private customer data
- private wallet data
- seed phrase
- private key

If such data appears in a screenshot or log, redact before public sharing.

---

## Public-Safe Payment Spine Evidence Template

Use this structure when recording controlled payment-spine evidence:

### Test Type

Controlled Pi Testnet Payment Spine

### Environment

Pi Browser / Sandbox / Testnet-oriented controlled flow

### Production Claim

No

### Result

PASSED or FAILED

### Public-Safe Observations

- invoice created:
- access initially locked:
- Pi authentication:
- payments scope:
- Pi wallet payment:
- backend approval / completion:
- backend-verified payment state:
- receipt/evidence generated:
- public verify:
- access unlocked after backend-verified receipt:
- minimumDisclosure:
- publicSafe:
- privateDataIncluded:
- secretsIncluded:
- paymentIdIncluded:
- txidIncluded:
- customerDataIncluded:

### Boundary

This is controlled Testnet evidence.

This is not Pi Mainnet settlement.

This is not production readiness.

This is not a formal audit.

---

## Public-Safe V9.1 Backend Validation Evidence Template

Use this structure when recording safe negative backend validation evidence:

### Test Type

V9.1 Safe Negative Backend Behavior Validation

### Environment

Live SafeGate validation page / public-safe backend checks

### Production Claim

No

### Result

PASSED or FAILED

### Visible Checks

- total visible checks:
- passed checks:
- failed checks:
- warning checks:

### Confirmed Public-Safe Behavior

- backend health returned ok:
- invoice creation kept access locked:
- invoice creation did not create receipt/evidence:
- receipt/evidence before verified payment rejected safely:
- missing invoiceId rejected safely:
- missing paymentId rejected safely:
- public verify missing inputs rejected safely:
- public verify unknown pair did not return active verified trust:
- obvious secret/internal leak terms detected:

### Boundary

This does not prove real duplicate approve/complete behavior.

This does not prove real paymentId/txid replay handling.

This does not prove Pi API timeout behavior.

This does not prove Supabase failure recovery.

This does not prove production readiness.

This does not prove formal audit completion.

---

## Evidence Record — Controlled Pi Testnet Payment Spine

**Status:** PASSED  
**Public-Safe:** Yes  
**Sensitive Identifiers Published:** No  
**Production Claim:** No  

Public-safe result:

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

Evidence link:

https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html

Document link:

SAFEGATE_CONTROLLED_PI_TESTNET_PAYMENT_SPINE_PASSED.md

---

## Evidence Record — V9.1 Safe Negative Backend Validation

**Status:** PASSED  
**Visible Checks:** 18  
**Passed:** 18  
**Failed:** 0  
**Warnings:** 0  
**Public-Safe:** Yes  
**Sensitive Identifiers Published:** No  
**Production Claim:** No  

Public-safe result:

- backend health: PASS
- invoice create: PASS
- access locked after invoice create: PASS
- no receipt/evidence at invoice stage: PASS
- receipt before verified payment rejected: PASS
- missing invoiceId rejected: PASS
- missing paymentId rejected: PASS
- public verify missing inputs rejected: PASS
- public verify unknown pair did not return active verified trust: PASS
- selected leak scans: PASS

Evidence link:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Document link:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

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
10. Backend Behavior Evidence Matrix
11. AI Review Risk Consensus
12. Receipt Integrity Plan
13. Pi Verification Depth Note
14. Public Verify Freshness Policy
15. Formal State Machine Table
16. Merchant Explanation

---

## Live Public-Safe Evidence Links

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

## Public-Safe Claims

SafeGate may say:

- Controlled Pi Testnet Payment Spine: PASSED
- V9.1 Safe Negative Backend Validation: PASSED
- access unlocked only after backend-verified receipt
- receipt/evidence was generated after backend-verified payment state
- invoice creation kept access locked
- receipt/evidence before verified payment was rejected safely
- public verify unknown pair did not return active verified trust
- public-safe responses did not show obvious secret/internal leak terms
- production readiness is not claimed
- formal audit is not claimed

SafeGate should not say:

- production ready
- fully secure
- tamper-proof
- formally audited
- official Pi partnership
- Pi Mainnet settlement completed
- all duplicate/replay risks solved
- all backend risks solved
- independent cryptographic verification completed
- regulatory approval completed

---

## Reviewer Guidance

A reviewer should focus on:

- whether the payment/trust separation is clear
- whether access unlock depends on backend-verified state
- whether receipt/evidence creation is properly guarded
- whether public verify avoids sensitive identifiers
- whether negative backend behavior fails safely
- whether duplicate/replay/failure cases need deeper testing
- whether claim boundaries are disciplined

Useful reviewer question:

Where would this architecture break first if it moved from controlled Testnet evidence to a real pilot environment?

---

## Remaining Evidence Gaps

The following evidence gaps remain before stronger pilot or production claims:

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- real paymentId replay behavior
- real txid replay behavior
- paymentId mismatch behavior
- txid mismatch behavior
- public verify mismatched receipt/evidence pair
- Pi API timeout behavior
- Supabase write failure behavior
- durable-state recovery behavior
- receipt/evidence integrity implementation
- public verify freshness implementation
- rate limiting / abuse resistance
- deeper safe error response validation
- external technical reviewer feedback

---

## Final Evidence Boundary

This evidence pack strengthens SafeGate’s Pilot Readiness evidence.

It does not claim production readiness.

It does not claim formal audit completion.

It does not claim Pi Mainnet settlement.

It does not claim official Pi partnership.

It does not claim tamper-proof or independently verified cryptographic proof.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
