# SafeGate V13 Controlled Hardening Scope

## Current Status

SafeGate V13 is open as a controlled hardening phase.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.

AI will automate payments. SafeGate will automate trust.

---

## V13 Purpose

V13 exists to define and harden the backend trust boundary before SafeGate moves toward broader pilot use.

The focus is not marketing expansion.

The focus is safe post-payment trust behavior under failure, mismatch, replay, timeout, incomplete-state, fee-required, and future automation-pressure conditions.

---

## Current V13 Public Status

- V13 Controlled Hardening Scope: Open
- V13 Public Surface Validation: Passed
- V13 Backend Policy Simulation: Passed
- Agent-Readable Trust Preview: Open
- Fee Architecture Decision: Documented
- Real Backend Hardening Evidence: Not passed yet
- Real Pi Payment Endpoint Validation: Not tested here
- Real Database Durability Validation: Not tested here
- Production Readiness: Not claimed
- Formal Third-Party Audit: Not claimed
- Official Pi Partnership: Not claimed

---

## Live V13 Links

Main Review Hub:

https://www.safegatelabs.xyz

V13 Controlled Hardening Scope:

https://www.safegatelabs.xyz/v13-controlled-hardening-scope.html

V13 Public Surface Validation:

https://www.safegatelabs.xyz/v13-public-surface-validation.html

V13 Backend Policy Simulation:

https://www.safegatelabs.xyz/v13-backend-policy-simulation.html

Agent-Readable Trust Preview:

https://www.safegatelabs.xyz/agent-readable-trust-preview.html

Fee Architecture Decision:

https://www.safegatelabs.xyz/fee-architecture-decision.html

Pilot Review Index:

https://www.safegatelabs.xyz/pilot-review-index.html

Pilot Readiness:

https://www.safegatelabs.xyz/pilot-readiness.html

---

## What Has Passed

### V13 Public Surface Validation

SafeGate public pages and claim-boundary language passed 12/12 public-surface checks.

This means the public review surface is available and consistent.

This is not real backend hardening evidence.

This is not a formal audit.

This is not production readiness.

### V13 Backend Policy Simulation

SafeGate V13 Backend Policy Simulation passed 12/12 client-side policy checks.

The simulation covers expected fail-secure policy behavior for:

- access locked before verification
- receipt and evidence denied before payment verification
- duplicate callback idempotency
- replay blocking
- payment / invoice mismatch blocking
- timeout or ambiguous verification fail-secure behavior
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

## V13 Real Backend Hardening Frontier

Real backend hardening evidence is still required.

V13 must still prove backend-level behavior for:

- duplicate callbacks
- idempotency
- replay resistance
- payment / invoice mismatch handling
- timeout and ambiguous verification handling
- durable state failure behavior
- public verify unknown or mismatched pair safety
- safe error output
- access unlock regression checks
- fee settlement confirmation

---

## V13 Pass Gate

SafeGate should not claim V13 passed until backend-level evidence exists for the following gates.

### 1. Duplicate Callback Gate

Backend must show duplicate callbacks are idempotent.

Expected behavior:

- first valid callback may be accepted
- duplicate callback must not create a second finalization
- duplicate callback must not create duplicate receipt or evidence records
- duplicate callback must not double-unlock access
- duplicate callback must not double-count fee settlement

### 2. Replay Resistance Gate

Backend must block reuse of payment or transaction identity across invoices.

Expected behavior:

- one payment identity must not verify multiple invoices
- one transaction identity must not verify multiple invoices
- replay attempts must return safe false / blocked response
- replay must not create receipt, evidence, access unlock, or public verified outcome

### 3. Payment / Invoice Mismatch Gate

Backend must block mismatched payment and invoice states.

Expected behavior:

- payment invoice id must match SafeGate invoice id
- amount and fee-required state must match expected state
- mismatch must fail secure
- mismatch must not unlock access
- mismatch must not create receipt or evidence

### 4. Timeout / Ambiguous Verification Gate

Backend must fail secure when verification is timed out, unavailable, ambiguous, or incomplete.

Expected behavior:

- ambiguous state is not verified state
- timeout is not verified state
- API failure is not verified state
- access remains locked
- receipt and evidence are not created

### 5. Durable State Failure Gate

Backend must fail secure if durable receipt/evidence storage fails.

Expected behavior:

- payment verification alone is not enough to unlock access
- durable receipt/evidence write must succeed before final trust outcome
- write failure must keep access locked
- public verify must not return active trust for missing durable records

### 6. Public Verify Safety Gate

Public verify must return safe false for unknown, missing, or mismatched receipt/evidence pairs.

Expected behavior:

- unknown receipt id returns not verified
- unknown evidence id returns not verified
- mismatched receipt/evidence pair returns not verified
- public verify must not expose sensitive data
- public verify must not expose backend internals

### 7. Safe Error Output Gate

Backend error output must remain public-safe.

Expected behavior:

- no stack traces
- no service role keys
- no database connection strings
- no raw Supabase internals
- no raw Pi API secrets
- no passphrases
- no private keys
- no sensitive customer data

### 8. Access Unlock Guard Gate

Access must remain locked until verified final state.

Expected behavior:

- no frontend-only unlock
- no unlock from callback alone
- no unlock from incomplete payment state
- no unlock before durable receipt/evidence where required
- no unlock before required fee verification

### 9. Fee Settlement Gate

Fee-required flows must not finalize without verified fee state.

Expected behavior:

- payment verification alone is not enough where fee is required
- required fee state must be verified before final SafeGate trust outcome
- no verified receipt/evidence/access/public verify where required fee is missing
- no postpaid merchant debt as the primary fee model

---

## Fee-Linked Hardening Rule

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

## Agent-Readable Trust Boundary

SafeGate is preparing for a future where trust states are readable by apps and AI agents.

Current status:

- static preview only
- no MCP endpoint
- no tool endpoint
- no agent execution
- no autonomous payment permission
- no autonomous access unlock permission
- no receipt creation by agent
- no evidence creation by agent

Agent-readable does not mean agent-executable.

Agent-readable does not mean autonomous payment.

Agent-readable does not mean autonomous access unlock.

Agent-readable does not mean production API.

Future controlled sequence:

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

## Privacy-Aware Boundary

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

Correct future framing:

SilentSwap can protect payment privacy.

SafeGate can verify what happened after the private payment.

Current boundary:

- no SilentSwap integration claim today
- no private transaction claim today
- no hidden blockchain transaction claim today
- no mixer claim
- no privacy protocol claim
- no private payment network claim

---

## Deployment Boundary

The current live deployment uses the Vercel Hobby plan and has reached the 12 serverless function limit.

Because of this, V13 Backend Policy Simulation and Agent-Readable Trust Preview are delivered as static HTML public-safe pages.

No new serverless function was added for these previews.

This protects the live production deployment from serverless-function-limit failures.

---

## What SafeGate Is Not Claiming

SafeGate does not claim:

- production readiness
- formal third-party audit
- official Pi partnership
- Pi Mainnet settlement
- custodial payment processing
- escrow service
- private key collection
- passphrase collection
- SilentSwap integration today
- private transactions today
- hidden blockchain transactions today
- MCP endpoint availability
- tool endpoint availability
- autonomous agent execution
- completed commercial pilot
- completed V13 backend hardening
- overall V13 passed status

---

## V13 Evidence Log Rule

V13 evidence must remain public-safe.

Evidence may include:

- pass/fail screenshots
- controlled test output
- public-safe JSON
- expected vs observed behavior
- browser/device context
- endpoint category
- limitation notes

Evidence must not include:

- passphrases
- private keys
- service role keys
- wallet secrets
- raw database credentials
- raw Pi API secrets
- sensitive merchant data
- sensitive user data
- raw backend internals

---

## Current Safe Statement

SafeGate V13 is open for controlled hardening review.

SafeGate public surface validation has passed.

SafeGate backend policy simulation has passed.

SafeGate agent-readable trust preview is open as a static preview only.

SafeGate is privacy-aware today, not a privacy protocol.

Real backend hardening evidence has not passed yet.

SafeGate is not production-ready.

SafeGate is not formally audited.

SafeGate is not claiming official Pi partnership.

Architecture is destiny.
