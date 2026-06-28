# SafeGate V13 Validation Runner Spec

## Purpose

This document defines the validation runner direction for SafeGate V13 controlled hardening.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.

AI will automate payments. SafeGate will automate trust.

---

## Current Status

- V13 Controlled Hardening Scope: Open
- V13 Public Surface Validation: Passed
- V13 Backend Policy Simulation: Passed
- Agent-Readable Trust Preview: Open
- Fee Architecture Decision: Documented
- Real Backend Hardening Evidence: Not passed yet
- Real Pi Payment Endpoint Validation: Not tested here
- Real Database Durability Validation: Not tested here
- Production Readiness: Not claimed
- Formal Audit: Not claimed
- Official Pi Partnership: Not claimed

---

## Current Live Validation Pages

V13 Public Surface Validation:

https://www.safegatelabs.xyz/v13-public-surface-validation.html

V13 Backend Policy Simulation:

https://www.safegatelabs.xyz/v13-backend-policy-simulation.html

Agent-Readable Trust Preview:

https://www.safegatelabs.xyz/agent-readable-trust-preview.html

V13 Controlled Hardening Scope:

https://www.safegatelabs.xyz/v13-controlled-hardening-scope.html

---

## Deployment Boundary

The current live deployment uses the Vercel Hobby plan and has reached the 12 serverless function limit.

Because of this:

- V13 Backend Policy Simulation is delivered as static HTML
- Agent-Readable Trust Preview is delivered as static HTML
- no new V13 serverless API function should be added on the current Hobby deployment
- real backend hardening evidence requires existing endpoint reuse, endpoint consolidation, a dedicated backend environment, or a separate test deployment

This protects the live production deployment from serverless-function-limit failures.

---

## Runner Type 1: Public Surface Validation

Current status:

Passed.

Purpose:

Checks live public pages for availability and claim-boundary language.

Validates:

- main hub available
- V13 page available
- fee page available
- pilot review index synced
- pilot readiness synced
- V13 not complete boundary present
- V13 not passed boundary present
- no custody boundary present
- no escrow boundary present
- no production readiness claim
- no formal audit claim
- no official Pi partnership claim

Boundary:

This is public-surface validation only.

It does not test real backend behavior.

It does not call real Pi payment endpoints.

It does not test real database durability.

---

## Runner Type 2: Backend Policy Simulation

Current status:

Passed.

Purpose:

Runs static client-side policy simulation for expected V13 fail-secure behavior.

Validates simulated behavior for:

- access locked before backend verification
- receipt and evidence denied before payment verification
- duplicate callback idempotency
- replay blocking
- payment / invoice mismatch blocking
- timeout / ambiguous verification fail-secure behavior
- durable write failure fail-secure behavior
- unknown public verify pair safety
- safe public error output
- fee-required finalization blocking before fee verification
- fee-verified finalization path
- boundary claims remaining false

Boundary:

This is static HTML client-side simulation.

It is not a live serverless API endpoint.

It is not real Pi payment validation.

It is not real database durability validation.

It does not mean overall V13 backend hardening has passed.

---

## Runner Type 3: Future Real Backend Hardening Runner

Current status:

Not implemented in current live deployment.

Purpose:

Future runner should validate real backend behavior against existing endpoints, consolidated endpoints, or a dedicated backend environment.

Required checks:

- duplicate callback behavior
- idempotency
- replay resistance
- payment / invoice mismatch handling
- timeout and ambiguous verification handling
- durable state failure behavior
- public verify unknown or mismatched pair safety
- safe error output
- access unlock regression checks
- fee settlement confirmation

Boundary:

This future runner must not expose secrets.

It must not expose service role keys.

It must not expose raw backend internals.

It must not expose sensitive merchant or user data.

It must not create production readiness claims automatically.

---

## Expected Runner Output Shape

Future validation runners should return or display public-safe output such as:

- runner name
- generated timestamp
- total checks
- passed checks
- failed checks
- public-safe status
- test scope
- limitation notes
- per-check result
- no secret output
- no raw stack trace output

Expected status labels:

- PASSED
- FAILED
- BLOCKED
- NOT_TESTED_HERE
- NOT_IMPLEMENTED
- PUBLIC_SAFE
- FAIL_SECURE

---

## Required Check Categories

| ID | Check | Required Behavior | Current Status |
|---|---|---|---|
| RUNNER-001 | Access Guard | Access remains locked before backend-verified final state. | Simulated / real evidence pending |
| RUNNER-002 | Receipt Guard | Receipt is not created before verified payment state. | Simulated / real evidence pending |
| RUNNER-003 | Evidence Guard | Evidence is not created before verified payment state. | Simulated / real evidence pending |
| RUNNER-004 | Duplicate Callback | Duplicate callback is idempotent and does not double-finalize. | Simulated / real evidence pending |
| RUNNER-005 | Replay Guard | Reused payment/tx identity cannot verify another invoice. | Simulated / real evidence pending |
| RUNNER-006 | Mismatch Guard | Payment / invoice mismatch fails secure. | Simulated / real evidence pending |
| RUNNER-007 | Timeout Guard | Timeout or ambiguous verification is not treated as verified. | Simulated / real evidence pending |
| RUNNER-008 | Durable Failure Guard | Access remains locked if receipt/evidence durable write fails. | Simulated / real evidence pending |
| RUNNER-009 | Public Verify Guard | Unknown or mismatched receipt/evidence pair returns not verified. | Simulated / real evidence pending |
| RUNNER-010 | Safe Error Output | Public errors do not expose secrets, stack traces, or internals. | Simulated / real evidence pending |
| RUNNER-011 | Fee Missing Guard | Fee-required flow does not finalize before fee verification. | Simulated / real evidence pending |
| RUNNER-012 | Fee Verified Path | Payment + fee verified flow may finalize receipt/evidence/access/public verify. | Simulated / real evidence pending |
| RUNNER-013 | Boundary Claims | Production, audit, custody, escrow, partnership, and unsupported split claims remain false. | Simulated / real evidence pending |
| RUNNER-014 | Agent Readability | Agent-readable trust state remains read-only preview. | Static preview open |
| RUNNER-015 | Agent Execution | No autonomous payment, unlock, receipt, evidence, MCP, or tool execution is enabled. | Boundary active |
| RUNNER-016 | Privacy-Aware Evidence | Public evidence remains minimum, public-safe, and non-sensitive. | Boundary active |
| RUNNER-017 | Privacy Protocol Claim | No SilentSwap-like private transaction or hidden blockchain transaction claim. | Boundary active |
| RUNNER-018 | Deployment Boundary | No new serverless API function added while Vercel Hobby function limit is reached. | Boundary active |

---

## Agent-Readable Runner Boundary

Agent-readable trust preview is open as a static preview only.

Current runner boundary:

- no MCP endpoint
- no tool endpoint
- no agent execution
- no autonomous payment
- no autonomous access unlock
- no receipt creation by agent
- no evidence creation by agent

Agent-readable does not mean agent-executable.

MCP/tool endpoints come only after real backend hardening.

Any future agent-readable runner must be read-only first.

It must not give agents write, unlock, payment, receipt, evidence, or fee-finalization authority.

---

## Privacy-Aware Runner Boundary

SafeGate is privacy-aware today, not a privacy protocol.

Current privacy boundary:

- minimum public-safe evidence
- no passphrase collection
- no private key collection
- no raw secrets in public evidence
- no sensitive customer data in public verify
- limited receipt/evidence/public verify output
- no SilentSwap-like private transaction claim
- no hidden blockchain transaction claim

Future privacy-preserving adapter tests may exist later, but not in current V13.

Correct future framing:

SilentSwap can protect payment privacy.

SafeGate can verify what happened after the private payment.

Current boundary:

- no SilentSwap integration claim today
- no private transaction claim today
- no mixer claim
- no privacy protocol claim
- no private payment network claim

---

## Evidence Safety Rules

Runner output may include:

- public-safe pass/fail status
- endpoint category
- check ID
- expected behavior
- observed behavior
- public-safe JSON
- limitation notes
- timestamp
- browser/device context where safe

Runner output must not include:

- passphrases
- private keys
- service role keys
- Pi app secrets
- Vercel environment secrets
- database credentials
- wallet secrets
- raw backend internals
- raw payment secrets
- sensitive merchant data
- sensitive user data
- raw stack traces
- raw database errors

---

## Pass Gate Before Claiming V13 Passed

SafeGate must not claim overall V13 passed until real backend evidence exists for:

- duplicate callback behavior
- replay resistance
- payment / invoice mismatch
- timeout and ambiguous verification
- durable write failure
- public verify safety
- safe error output
- access unlock guard
- fee-required finalization
- fee-verified finalization
- public-safe evidence output

Public surface validation and backend policy simulation are valuable, but they are not enough to claim real V13 backend hardening passed.

---

## Current Safe Statement

V13 Public Surface Validation has passed.

V13 Backend Policy Simulation has passed.

Agent-Readable Trust Preview is open as a static preview only.

SafeGate is privacy-aware today, not a privacy protocol.

Real backend hardening evidence has not passed yet.

No new V13 serverless API function should be added on the current Hobby deployment until endpoint capacity or backend environment is resolved.

SafeGate is not production-ready.

SafeGate is not formally audited.

SafeGate is not claiming official Pi partnership.

Architecture is destiny.
