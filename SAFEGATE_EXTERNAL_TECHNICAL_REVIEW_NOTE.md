# SafeGate External Technical Review Note

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose of This Note

This note is for external technical reviewers.

The goal is not to ask for praise.

The goal is to help reviewers understand what SafeGate currently does, what it does not claim, and where the next technical risks should be examined.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| Pilot Readiness | Open |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Custodial Payment Processing | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Regulatory Approval | Not claimed |

---

## What Was Demonstrated

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

The tested flow demonstrated:

- V9 invoice creation
- initial access state remained locked
- Pi Browser authentication succeeded
- username + payments scope succeeded
- Pi wallet payment succeeded in Sandbox / Testnet context
- backend approval / completion flow executed
- backend-verified payment state reached
- receipt and evidence were generated after verified state
- public verify confirmed a minimum-disclosure record
- access unlocked only after backend-verified receipt

---

## Key Technical Result

The controlled result reached:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- public verify: confirmed
- minimumDisclosure: true
- publicSafe: true
- paymentIdIncluded: false
- txidIncluded: false
- secretsIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

This is the strongest current SafeGate evidence.

---

## What SafeGate Is

SafeGate is a post-payment trust layer.

It is designed to verify what happens after a payment event:

- invoice creation
- payment intent
- backend payment verification
- state transition control
- receipt generation
- evidence generation
- access state
- public verification
- merchant-side proof
- audit / trust-state direction
- public-safe evidence packaging

SafeGate is Pi-first, not Pi-only.

---

## What SafeGate Is Not

SafeGate is not currently claiming:

- production readiness
- Pi Mainnet settlement
- official Pi Core Team partnership
- custody of payments
- regulatory approval
- formal third-party audit
- enterprise-grade security certification
- high-volume production operation
- completed commercial pilot execution

---

## Primary Technical Principle

SafeGate does not unlock access from the frontend callback alone.

Access is unlocked only after:

1. backend-verified payment state
2. guarded receipt/evidence creation
3. public verification path becoming available

This is the core technical distinction.

---

## Public-Safe Evidence Boundary

Raw screenshots may contain sensitive technical details such as:

- payment identifiers
- transaction identifiers
- wallet information
- user identifiers
- access tokens
- recipient addresses
- app identifiers
- internal logs

Raw screenshots should not be published unredacted.

Public evidence should only show minimum-disclosure confirmation such as:

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

## What Reviewers Should Examine

A technical reviewer should focus on:

- whether the state machine is strict enough
- whether illegal state transitions are blocked
- whether payment approval and completion are idempotent
- whether replay attempts are rejected
- whether receipt/evidence can only be created after verified payment state
- whether public verify exposes only minimum-disclosure data
- whether error handling is fail-secure
- whether Supabase state handling avoids leaking secrets
- whether the system can survive repeated or duplicate callbacks
- whether future production hardening requirements are clearly separated from current controlled evidence

---

## Known Remaining Work

SafeGate still needs:

- additional external controlled payment-spine test
- redacted public evidence package
- clearer commercial pilot checklist
- stronger production-grade rate limiting
- deeper replay/idempotency testing
- formal threat model packaging
- third-party technical review
- no production claim until operational controls are stronger

---

## Main Review Links

| Page | URL |
|---|---|
| Main Review Hub | https://www.safegatelabs.xyz |
| V9 Payment Spine Evidence | https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html |
| Controlled Checkout | https://www.safegatelabs.xyz/safegate-checkout.html |
| Pilot Review Index | https://www.safegatelabs.xyz/pilot-review-index.html |
| Pilot Readiness | https://www.safegatelabs.xyz/pilot-readiness.html |
| Public Repository | https://github.com/Nurexen-Labs/safegate-labs-public |

---

## Review Question

The most useful external review question is:

Where would this architecture break first if it moved from controlled Testnet evidence to a real pilot environment?

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
