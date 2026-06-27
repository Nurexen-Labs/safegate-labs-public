# SafeGate Pilot Evidence Template

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This template is used to capture public-safe SafeGate pilot or reviewer evidence.

It is designed to record useful test results without exposing sensitive payment identifiers, wallet data, private user data, internal logs, secrets, or raw API responses.

This template supports:

- Controlled Pi Testnet Payment Spine evidence
- V9.1 Safe Negative Backend Validation evidence
- pilot readiness feedback
- external reviewer notes
- public-safe tester evidence

This template does not create a production-readiness claim.

---

## Evidence Boundary

This evidence template must not be used to claim:

- production readiness
- Pi Mainnet settlement
- official Pi Core Team partnership
- custodial payment processing
- regulatory approval
- formal security audit
- security certification
- tamper-proof infrastructure
- independent cryptographic verification
- completed commercial pilot

---

## Evidence Record

### Evidence Title

Example:

SafeGate Controlled Pi Testnet Payment Spine — Public-Safe Evidence

or

SafeGate V9.1 Safe Negative Backend Validation — Public-Safe Evidence

---

### Evidence Type

Choose one:

- Controlled Pi Testnet Payment Spine
- V9.1 Safe Negative Backend Validation
- Public Verify Check
- Receipt / Evidence Check
- Merchant Review
- External Technical Review
- Controlled Pilot Tester Feedback
- Other

---

### Evidence Status

Choose one:

- PASSED
- FAILED
- PARTIAL
- REVIEW_REQUIRED
- NOT_REPRODUCED

---

### Test Environment

Use public-safe description only.

Allowed examples:

- Pi Browser
- Chrome
- Mobile browser
- Desktop browser
- Pi Sandbox / Testnet-oriented controlled flow
- Live SafeGate validation page
- Public-safe reviewer flow

Do not include:

- private wallet address
- raw paymentId
- raw txid
- access token
- app secret
- service role key
- internal database detail
- private customer data

---

## Controlled Pi Testnet Payment Spine Section

Use this section only when recording controlled payment-spine evidence.

### Controlled Flow Checklist

- invoice created:
- initial access state remained locked:
- Pi Browser authentication:
- username + payments scope:
- Pi wallet payment in Sandbox / Testnet context:
- backend approval / completion flow:
- backend-verified payment state:
- receipt generated after verified state:
- evidence generated after verified state:
- public verify confirmed:
- access unlocked only after backend-verified receipt:

### Public-Safe Result Fields

- paymentState:
- accessState:
- verified:
- minimumDisclosure:
- publicSafe:
- privateDataIncluded:
- secretsIncluded:
- rawPiApiResponseIncluded:
- serviceRoleKeyIncluded:
- paymentIdIncluded:
- txidIncluded:
- customerDataIncluded:
- accessUnlocked:

### Expected Safe Result Example

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

---

## V9.1 Safe Negative Backend Validation Section

Use this section only when recording V9.1 safe negative backend behavior validation evidence.

### Validation Result

- total visible checks:
- passed checks:
- failed checks:
- warning checks:

### Expected Current V9.1 Result Example

- total visible checks: 18
- passed checks: 18
- failed checks: 0
- warning checks: 0

### Public-Safe Negative Behavior Checklist

- backend health returned ok:
- invoice creation kept access locked:
- invoice creation did not create receipt/evidence:
- receipt/evidence before verified payment rejected safely:
- missing invoiceId rejected safely:
- missing paymentId rejected safely:
- public verify missing inputs rejected safely:
- public verify unknown pair did not return active verified trust:
- validation responses did not show obvious secret/internal leak terms:

### V9.1 Boundary

This validation does not prove:

- real duplicate approve behavior
- real duplicate complete behavior
- real paymentId replay handling
- real txid replay handling
- paymentId mismatch behavior
- Pi API timeout behavior
- Supabase failure recovery
- production readiness
- formal audit completion

---

## Public Verify Evidence Section

Use this section when recording public verify evidence.

### Public Verify Result

- receipt/evidence pair checked:
- public verify returned:
- verified:
- minimumDisclosure:
- publicSafe:
- accessState:
- privateDataIncluded:
- secretsIncluded:
- paymentIdIncluded:
- txidIncluded:
- customerDataIncluded:

### Public Verify Boundary

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

---

## Reviewer Feedback Section

### Reviewer Type

Choose one:

- software developer
- backend reviewer
- security reviewer
- merchant reviewer
- Pi ecosystem reviewer
- tester
- other

### Reviewer Feedback

Write public-safe feedback only.

Suggested questions:

1. What is the strongest part of the architecture?
2. Where would it break first?
3. Which backend state transition is most dangerous?
4. Which duplicate/replay case should be tested next?
5. Which public verify detail is unclear?
6. Which claim should be softened?
7. What should be implemented before controlled pilot execution?

---

## Public-Safe Screenshot Checklist

Before publishing any screenshot, confirm:

- no raw paymentId visible
- no raw txid visible
- no wallet address visible
- no recipient address visible
- no access token visible
- no user identifier visible
- no app identifier visible
- no internal logs visible
- no raw Pi API response visible
- no service role key visible
- no private wallet data visible
- no private customer data visible

If any item is visible, redact before publishing.

---

## Allowed Public-Safe Summary Format

Use this format for public-safe summaries:

SafeGate test type:

Result:

Environment:

Public-safe outcome:

- verified:
- minimumDisclosure:
- publicSafe:
- accessState:
- privateDataIncluded:
- secretsIncluded:
- paymentIdIncluded:
- txidIncluded:
- customerDataIncluded:

Boundary:

This is controlled evidence.

This is not production readiness.

This is not Pi Mainnet settlement.

This is not a formal audit.

---

## Example 1 — Controlled Pi Testnet Payment Spine

SafeGate test type:

Controlled Pi Testnet Payment Spine

Result:

PASSED

Environment:

Pi Browser / Pi Sandbox / Testnet-oriented controlled flow

Public-safe outcome:

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

Boundary:

This is controlled Pi Testnet evidence.

This is not Pi Mainnet settlement.

This is not production readiness.

This is not a formal audit.

---

## Example 2 — V9.1 Safe Negative Backend Validation

SafeGate test type:

V9.1 Safe Negative Backend Validation

Result:

PASSED

Environment:

Live SafeGate validation page / public-safe backend checks

Public-safe outcome:

- total visible checks: 18
- passed checks: 18
- failed checks: 0
- warning checks: 0
- invoice creation kept access locked: true
- invoice creation created receipt/evidence: false
- receipt/evidence before verified payment rejected safely: true
- missing invoiceId rejected safely: true
- missing paymentId rejected safely: true
- unknown public verify pair returned active verified trust: false
- obvious secret/internal leak terms detected: false

Boundary:

This does not prove real duplicate approve/complete behavior.

This does not prove real paymentId/txid replay handling.

This does not prove Pi API timeout behavior.

This does not prove Supabase failure recovery.

This does not prove production readiness.

This does not prove formal audit completion.

---

## Safe Public Language

SafeGate may say:

- controlled Pi Testnet payment-spine passed
- V9.1 safe negative backend validation passed
- public-safe evidence was captured
- access unlocked only after backend-verified receipt
- invoice creation kept access locked
- receipt/evidence before verified payment was rejected safely
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
- independent cryptographic verification completed

---

## Final Evidence Template Position

This template helps SafeGate capture evidence safely.

It protects sensitive payment and user data.

It keeps claims boundary-safe.

It supports Pilot Readiness and controlled technical review.

It does not create a production-readiness claim.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
