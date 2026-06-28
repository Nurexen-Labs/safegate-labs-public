# SafeGate V13 Endpoint Map Template

## Purpose

This document maps SafeGate endpoint categories for V13 controlled hardening review.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.

AI will automate payments. SafeGate will automate trust.

---

## Current Deployment Boundary

The current live deployment uses the Vercel Hobby plan and has reached the 12 serverless function limit.

Because of this:

- no new V13 serverless API function should be added on the current Hobby deployment
- V13 Backend Policy Simulation is delivered as static HTML
- Agent-Readable Trust Preview is delivered as static HTML
- real backend hardening evidence requires either existing endpoint reuse, endpoint consolidation, or a dedicated backend environment

---

## Current Endpoint / Page Categories

| Category | Current Form | Status | Boundary |
|---|---|---|---|
| Main Review Hub | Static HTML | Live | Public review only |
| Pilot Review Index | Static HTML | Live | Public review only |
| Pilot Readiness | Static HTML | Live | Controlled pilot planning |
| V9 Payment Spine Evidence | Static HTML | Live | Controlled evidence |
| V9.1 Backend Behavior Validation | Existing validation page | Live | Selected safe negative validation |
| V13 Controlled Hardening Scope | Static HTML | Live | Scope open |
| V13 Public Surface Validation | Static HTML + client fetch checks | Passed | Public surface only |
| V13 Backend Policy Simulation | Static HTML client-side simulation | Passed | Not real backend API |
| Fee Architecture Decision | Static HTML | Live | No custody / no escrow |
| Agent-Readable Trust Preview | Static HTML | Live | No MCP/tool/agent execution |
| Public Repository Docs | Markdown | Live | Public-safe review materials |

---

## Existing API Boundary

The current live repository already contains existing `api/*.js` functions.

New V13 serverless functions should not be added on the current Hobby deployment unless one of the following happens:

1. an existing unused function is removed
2. endpoints are consolidated
3. the deployment is moved to a dedicated backend environment
4. the Vercel plan changes
5. a separate test deployment is created for backend hardening evidence

---

## V13 Real Backend Evidence Endpoint Categories

Future backend evidence should map to these categories:

| ID | Endpoint Category | Required Behavior | Evidence Needed |
|---|---|---|---|
| ENDPOINT-001 | Invoice Create | Creates invoice/payment intent safely. | Request/response evidence |
| ENDPOINT-002 | Payment Verify | Verifies payment through backend-controlled logic. | Verified / not verified evidence |
| ENDPOINT-003 | Payment Approve | Handles approval boundary safely. | Controlled approval evidence |
| ENDPOINT-004 | Payment Complete | Handles completion boundary safely. | Controlled completion evidence |
| ENDPOINT-005 | Receipt Create | Creates receipt only after verified state. | Deny-before-verify and allow-after-verify evidence |
| ENDPOINT-006 | Evidence Create | Creates evidence only after verified state. | Deny-before-verify and allow-after-verify evidence |
| ENDPOINT-007 | Access Unlock | Unlocks only after verified final state. | Locked-before-final / unlocked-after-final evidence |
| ENDPOINT-008 | Public Verify | Returns safe false for unknown or mismatched pairs. | Unknown/mismatch/confirmed evidence |
| ENDPOINT-009 | Fee Verify | Confirms required fee state before finalization. | Missing-fee blocked / fee-verified evidence |
| ENDPOINT-010 | Duplicate Callback Guard | Duplicate callback is idempotent. | No double-finalization evidence |
| ENDPOINT-011 | Replay Guard | Reused payment/tx identity is blocked. | Replay blocked evidence |
| ENDPOINT-012 | Mismatch Guard | Payment/invoice mismatch fails secure. | Mismatch blocked evidence |
| ENDPOINT-013 | Timeout Guard | Timeout/ambiguous verification fails secure. | Access locked evidence |
| ENDPOINT-014 | Durable Failure Guard | Durable write failure keeps access locked. | Write failure blocked evidence |
| ENDPOINT-015 | Safe Error Output | No secrets, stack traces, or internals exposed. | Public-safe error evidence |

---

## Agent-Readable Endpoint Boundary

Current status:

- Agent-Readable Trust Preview is static only
- no MCP endpoint
- no tool endpoint
- no agent execution
- no autonomous payment
- no autonomous access unlock
- no agent-created receipt
- no agent-created evidence

Future endpoint categories may include read-only trust-state endpoints, but only after real backend hardening.

Possible future read-only categories:

| Future Category | Purpose | Boundary |
|---|---|---|
| Trust State Read | Let app/agent read safe trust state. | Read-only |
| Receipt Proof Read | Let app/agent read public-safe receipt proof. | No secrets |
| Evidence State Read | Let app/agent read evidence status. | Public-safe only |
| Access State Read | Let app/agent read locked/unlocked state. | No unlock authority |
| Public Verify Read | Let app/agent read public verify result. | Safe false on unknown/mismatch |
| Limitation Read | Let app/agent read known limitations. | No execution |

No future agent-readable endpoint should allow:

- autonomous payment execution
- autonomous access unlock
- receipt creation
- evidence creation
- fee finalization
- custody behavior
- escrow behavior
- private key/passphrase request
- raw secret exposure

---

## Privacy-Aware Endpoint Boundary

SafeGate is privacy-aware today, not a privacy protocol.

Current endpoint boundary:

- minimum public-safe evidence
- no passphrase collection
- no private key collection
- no raw secrets
- no sensitive customer data in public verify
- limited receipt/evidence/public verify output
- no SilentSwap-like private transaction claim
- no hidden blockchain transaction claim

Future privacy-preserving adapter categories may exist later, but not in current V13.

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

## Endpoint Evidence Safety Rules

Allowed public evidence:

- endpoint category
- public-safe response status
- pass/fail result
- expected vs observed behavior
- public-safe JSON
- timestamps where safe
- limitation notes

Not allowed in public evidence:

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
