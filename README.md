# SafeGate

SafeGate is a Pi-first, not Pi-only, post-payment trust architecture by Nurexen Labs.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.

AI will automate payments. SafeGate will automate trust.

---

## What SafeGate Is

SafeGate is a post-payment trust layer for controlled payment verification, receipt proof, evidence records, access state, public verification, merchant trust records, and future agent-readable trust states.

SafeGate is designed to answer one core question:

Did the promised post-payment outcome actually happen?

---

## What SafeGate Is Not

SafeGate is not a payment processor.

SafeGate does not custody buyer or merchant funds.

SafeGate does not act as escrow.

SafeGate does not ask for passphrases or private keys.

SafeGate does not claim production readiness.

SafeGate does not claim a formal third-party audit.

SafeGate does not claim official Pi partnership.

SafeGate does not claim Pi Mainnet settlement.

SafeGate does not currently provide SilentSwap-like private transactions.

SafeGate does not currently expose MCP or tool endpoints.

SafeGate does not currently enable autonomous AI-agent execution.

---

## Current Public Status

- V9 Payment Spine: Passed
- V9.1 Backend Behavior Validation: Passed
- V13 Public Surface Validation: Passed
- V13 Backend Policy Simulation: Passed
- Agent-Readable Trust Preview: Open
- Fee Architecture Decision: Documented
- V13 Controlled Hardening Scope: Open
- Controlled Pilot Planning: Open
- Real Backend Hardening Evidence: Not passed yet
- Production Readiness: Not claimed
- Formal Third-Party Audit: Not claimed
- Official Pi Partnership: Not claimed

---

## Live Review Hub

Main review hub:

https://www.safegatelabs.xyz

Pilot Review Index:

https://www.safegatelabs.xyz/pilot-review-index.html

Pilot Readiness:

https://www.safegatelabs.xyz/pilot-readiness.html

V13 Controlled Hardening Scope:

https://www.safegatelabs.xyz/v13-controlled-hardening-scope.html

Fee Architecture Decision:

https://www.safegatelabs.xyz/fee-architecture-decision.html

V13 Public Surface Validation:

https://www.safegatelabs.xyz/v13-public-surface-validation.html

V13 Backend Policy Simulation:

https://www.safegatelabs.xyz/v13-backend-policy-simulation.html

Agent-Readable Trust Preview:

https://www.safegatelabs.xyz/agent-readable-trust-preview.html

---

## Core SafeGate Flow

SafeGate’s trust flow is built around the following sequence:

1. Invoice or payment intent is created.
2. Payment state is verified through backend-controlled logic.
3. Receipt proof is created only after verified payment state.
4. Evidence record is created only after verified payment state.
5. Access remains locked until verified final state.
6. Public verify returns safe false for unknown or mismatched receipt/evidence pairs.
7. Merchant trust records can be built from verified outcomes.
8. Fee-required flows must not finalize without verified fee state.

---

## V9 Payment Spine

SafeGate V9 established a controlled Pi Testnet payment-spine direction.

The V9 focus included:

- Pi-first payment flow direction
- backend verification boundary
- receipt/evidence direction
- access locked until verified state
- public-safe review flow

V9 is not a Pi Mainnet settlement claim.

V9 is not production readiness.

---

## V9.1 Backend Behavior Validation

SafeGate V9.1 added selected safe negative backend validation evidence.

The purpose was to show that SafeGate should not treat incomplete, unknown, mismatched, or invalid states as verified trust.

This supports the fail-secure principle:

If verification is incomplete, ambiguous, mismatched, or unknown, SafeGate should not unlock access or create a verified trust outcome.

---

## V13 Controlled Hardening Scope

V13 is the controlled hardening phase.

V13 focuses on:

- duplicate callback behavior
- idempotency
- replay resistance
- payment / invoice mismatch handling
- timeout and ambiguous verification behavior
- durable state failure behavior
- public verify unknown or mismatched pair safety
- safe error output
- access unlock regression checks
- fee settlement confirmation

Current V13 status:

- V13 scope is open
- V13 public surface validation passed
- V13 backend policy simulation passed
- real backend hardening evidence has not passed yet
- real Pi payment endpoint validation is not tested here
- real database durability validation is not tested here

---

## V13 Public Surface Validation

SafeGate V13 Public Surface Validation passed 12/12 public-surface checks.

This means live public pages and claim-boundary language are available and consistent.

This is not real backend hardening evidence.

This is not a formal audit.

This is not production readiness.

---

## V13 Backend Policy Simulation

SafeGate V13 Backend Policy Simulation passed 12/12 client-side policy checks.

This simulation covers expected fail-secure behavior for:

- access locked before verification
- receipt/evidence denied before payment verification
- duplicate callback idempotency
- replay blocking
- payment/invoice mismatch blocking
- timeout/ambiguous verification fail-secure behavior
- durable write failure fail-secure behavior
- unknown public verify pair safety
- safe public error output
- fee-required finalization blocking before fee verification
- fee-verified finalization path
- boundary claims remaining false

Important boundary:

This is a static HTML client-side policy simulation.

It does not use a new serverless API function.

It does not call real Pi payment endpoints.

It does not test real database durability.

It does not mean overall V13 backend hardening has passed.

---

## Fee Architecture

SafeGate may use a transparent 100 + 1 verification fee model.

Where native non-custodial split is supported:

- buyer pays 101
- merchant receives 100
- SafeGate receives 1

Where native split is not supported:

- buyer pays the merchant amount
- buyer separately pays the SafeGate verification fee
- SafeGate finalizes verified trust only after required payment and fee states are confirmed

SafeGate does not use postpaid merchant debt as the primary fee model.

SafeGate does not custody funds.

SafeGate does not act as escrow.

No fee confirmation means no finalized SafeGate trust outcome where the fee applies.

---

## Agent-Readable Trust Direction

SafeGate is preparing for a future where trust states are readable not only by humans, but also by apps and AI agents.

The current agent-readable direction is intentionally conservative.

Current status:

- static preview only
- no MCP endpoint
- no tool endpoint
- no agent execution
- no autonomous payment permission
- no autonomous access unlock permission
- no receipt creation by agent
- no evidence creation by agent

Future sequence:

1. Finish safe payment / trust spine
2. Strengthen hash and tamper-evident receipt proof
3. Prepare pilot merchant pack
4. Expand agent-readable trust schema
5. Add controlled read-only MCP/tool endpoints only after hardening

Safe agent principles:

- read before act
- verify before trust
- fail closed on ambiguity
- never unlock before final state
- never expose secrets
- never treat simulation as real settlement
- never expose MCP/tools before hardening

---

## Privacy-Aware Direction

SafeGate is privacy-aware today, not a privacy protocol.

Current SafeGate privacy boundary:

- minimum public-safe evidence
- no passphrase collection
- no private key collection
- no raw secrets in public evidence
- no sensitive customer data in public verify
- limited receipt/evidence/public verify output
- no SilentSwap-like private transaction claim

Future privacy direction:

SafeGate may become privacy-preserving through future adapter integrations.

A future Privacy-Preserving Trust Adapter could allow SafeGate to verify post-payment trust outcomes after a privacy-preserving payment layer.

Correct future framing:

SilentSwap can protect payment privacy.

SafeGate can verify what happened after the private payment.

Current boundary:

SafeGate does not claim SilentSwap integration today.

SafeGate does not claim private transactions today.

SafeGate does not claim that blockchain transactions are hidden.

SafeGate does not act as a mixer, privacy protocol, or private payment network.

---

## Deployment Boundary

The current live deployment uses the Vercel Hobby plan and has reached the 12 serverless function limit.

Because of this, V13 Backend Policy Simulation and Agent-Readable Trust Preview are delivered as static HTML public-safe pages.

No new serverless function was added for these previews.

This protects the live production deployment from serverless-function-limit failures.

---

## Public Repository Purpose

This public repository is for public-safe review materials.

It may include:

- positioning
- public review package
- architecture summaries
- fee architecture decision
- V13 hardening scope
- pilot evidence materials
- claim boundaries
- public review links

This public repository must not include:

- Supabase service role keys
- Pi app secrets
- Vercel environment secrets
- database credentials
- private API secrets
- wallet secrets
- passphrases
- private keys
- sensitive merchant data
- sensitive user data
- raw backend secrets
- raw payment secrets

---

## Main Review Question

Where would this architecture break first if it moved from controlled Pi Testnet evidence to a small real pilot environment?

Key review areas:

- duplicate callback behavior
- replay resistance
- invoice / payment mismatch
- timeout / ambiguous verification
- durable receipt and evidence failure
- public verify safety
- fee-required finalization
- safe error output
- access unlock guard
- future agent-readable trust-state consumers
- future privacy-preserving trust adapter boundaries

---

## Current Safe Statement

SafeGate is ready for serious technical review and controlled pilot planning.

SafeGate is not production-ready.

SafeGate is not formally audited.

SafeGate is not claiming official Pi partnership.

SafeGate’s public surface validation and backend policy simulation have passed as public-safe checks.

SafeGate’s agent-readable trust preview is open as a static preview only.

SafeGate is privacy-aware today, not a privacy protocol.

Real backend hardening evidence remains the next frontier.

Architecture is destiny.
