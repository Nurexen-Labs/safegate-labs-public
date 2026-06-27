# SafeGate Fee Architecture Decision

## Purpose

This document defines the SafeGate fee architecture decision for controlled pilot and future multi-chain payment environments.

SafeGate verifies what happened after payment.

SafeGate does not process payments.

SafeGate does not custody funds.

SafeGate does not act as escrow.

SafeGate does not ask for passphrases, private keys, or wallet secrets.

---

## Core Fee Principle

SafeGate charges for successful post-payment trust verification.

The fee is not a payment processing fee.

The fee is not escrow.

The fee is not custody.

The fee is for:

- backend payment verification
- receipt state
- evidence state
- access state
- public-safe verification state
- trust outcome validation

---

## Internal Philosophy

SafeGate taraf olmaz.

SafeGate kasa olmaz.

SafeGate hakem olur.

Hakemlik / doğrulama ücretini alır.

---

## Current Intended Commercial Model

The first successful verified transactions may be free during controlled pilot onboarding.

After the free pilot threshold, SafeGate may charge a transparent 1% verification fee after successful verified outcomes.

Example:

| Item | Amount |
|---|---|
| Merchant service amount | 100 |
| SafeGate verification fee | 1 |
| Total buyer-visible amount | 101 |

---

## Critical Decision

SafeGate should not operate on a postpaid merchant receivable model as the primary model.

SafeGate should not depend on delayed merchant payment, accumulated debt, manual collection, or informal settlement as the main fee model.

SafeGate should not chase merchants for later payment.

SafeGate should not create verified receipt, evidence, access unlock, or public verify before the SafeGate verification fee is also confirmed where the fee applies.

---

## Supported Split Model

Where native non-custodial split or audited smart contract routing is supported:

| Party | Result |
|---|---|
| Buyer pays | 101 |
| Merchant receives | 100 |
| SafeGate receives | 1 |

This model may be used only when the routing is:

- non-custodial
- transparent
- technically safe
- auditable
- simple enough to avoid unnecessary custody risk
- compatible with the payment environment

SafeGate should not hold the full buyer payment and then redistribute funds.

---

## Non-Split Model

Where native split is not available, including Pi if multi-recipient split is not supported, SafeGate uses two verified payment intents.

Example:

| Step | Payment |
|---|---|
| 1 | Buyer pays merchant service amount |
| 2 | Buyer pays SafeGate verification fee |

Example:

| Payment | Amount | Recipient |
|---|---|---|
| Service payment | 100 Pi | Merchant |
| SafeGate verification fee | 1 Pi | SafeGate / Nurexen |
| Buyer-visible total | 101 Pi | Split across two verified payment intents |

---

## Non-Split Verification Rule

In a non-split environment, SafeGate verification is not finalized until both payments are backend-verified.

Required flags:

| Flag | Required |
|---|---|
| merchantPaymentVerified | true |
| safegateFeeVerified | true |

If either required payment is missing, pending, failed, ambiguous, mismatched, replayed, or unavailable:

- receipt is not created
- evidence is not created
- access is not unlocked
- public verify is not active
- SafeGate verified outcome is not finalized

---

## SafeGate Does Not Automatically Deduct Fees Without Split

If native split is not available, SafeGate cannot automatically deduct the 1% fee from the merchant payment.

In that case, SafeGate must create a separate verified fee payment flow.

The system does not “cut” the fee from a single payment unless the underlying payment environment supports safe native split or audited non-custodial routing.

---

## Why Postpaid Merchant Debt Is Not Primary

A postpaid merchant receivable model creates unnecessary risk:

- collection risk
- accounting friction
- merchant debt tracking
- manual settlement burden
- delayed SafeGate revenue
- unclear user experience
- possible trust misalignment

SafeGate’s preferred model is transaction-time fee settlement.

---

## Fee Settlement Rule

SafeGate verification fee must be settled during the verified transaction flow where the fee applies.

Where native split exists:

- fee is routed automatically

Where native split does not exist:

- fee is collected through a separate verified payment intent

In both cases:

- no fee settlement means no finalized SafeGate verification
- no finalized verification means no verified receipt/evidence/access/public verify

---

## Pi-Specific Direction

SafeGate does not assume native Pi payment splitting.

If Pi does not support native multi-recipient payment split, SafeGate should use the dual verified payment model:

1. buyer pays merchant
2. buyer pays SafeGate verification fee
3. backend verifies both payment states
4. SafeGate finalizes trust only after both are verified

This avoids:

- custody
- escrow
- merchant debt
- delayed settlement
- pretending a split exists when it does not

---

## Future EVM / Solana Direction

For EVM, Solana, or other programmable environments, SafeGate may later support non-custodial splitter/router logic.

This should only happen if the implementation is:

- audited or reviewable
- minimal
- non-custodial
- transparent
- chain-specific
- separated from payment verification logic
- documented with clear boundaries

---

## Adapter Separation

SafeGate should keep fee logic separate from payment verification logic.

Suggested architecture:

| Adapter | Purpose |
|---|---|
| PaymentVerificationAdapter | verifies payment state |
| FeeCollectionAdapter | verifies SafeGate fee settlement |
| TreasuryLedger | records SafeGate/Nurexen fee receipts |
| TrustStateEngine | finalizes receipt/evidence/access/public verify only after required states are true |

Core rule:

Payment verification is not the same as fee collection.

---

## Public-Safe Buyer Display

A buyer may see:

| Item | Amount |
|---|---|
| Service payment | 100 Pi |
| SafeGate verification fee | 1 Pi |
| Total verified checkout cost | 101 Pi |

If split is not supported, the interface should clearly explain that the total is completed through two verified payment steps.

---

## Public-Safe Merchant Display

A merchant may see:

| Item | Status |
|---|---|
| Service amount | received by merchant |
| SafeGate verification fee | paid directly to SafeGate / Nurexen |
| SafeGate custody | false |
| Escrow | false |
| Postpaid merchant debt | not primary model |

---

## What SafeGate Must Not Claim

SafeGate must not claim:

- production readiness
- formal third-party audit
- security certification
- official Pi partnership
- Pi Mainnet settlement if not live
- native Pi split if not supported
- custody
- escrow
- payment processing
- automatic deduction where split does not exist
- all fee models solved across all chains

---

## Current Safe Statement

SafeGate may use a transparent 100 + 1 verification fee model.

Where native non-custodial split is supported, the fee can be routed automatically.

Where native split is not supported, SafeGate uses a separate verified fee payment.

SafeGate does not finalize verified receipt, evidence, access unlock, or public verify until the required service payment and SafeGate verification fee are confirmed.

SafeGate never holds buyer or merchant funds.
