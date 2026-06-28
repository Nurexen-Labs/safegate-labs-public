# SafeGate V13 Controlled Hardening Test Matrix

## Purpose

This matrix defines the V13 controlled hardening checks for SafeGate.

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
- Production Readiness: Not claimed
- Formal Audit: Not claimed
- Official Pi Partnership: Not claimed

---

## Important Boundary

This test matrix is for controlled hardening planning.

It does not claim production readiness.

It does not claim formal audit.

It does not claim official Pi partnership.

It does not claim completed V13 backend hardening.

It does not claim SilentSwap integration.

It does not claim private transactions.

It does not expose MCP or tool endpoints.

It does not enable autonomous agent execution.

---

## Test Matrix

| ID | Area | Expected Behavior | Current Status |
|---|---|---|---|
| V13-001 | Access Guard | Access remains locked before backend-verified final state. | Simulated / Real backend evidence pending |
| V13-002 | Receipt Guard | Receipt is not created before verified payment state. | Simulated / Real backend evidence pending |
| V13-003 | Evidence Guard | Evidence is not created before verified payment state. | Simulated / Real backend evidence pending |
| V13-004 | Duplicate Callback | Duplicate callback is idempotent and does not double-finalize. | Simulated / Real backend evidence pending |
| V13-005 | Replay Resistance | Reused payment or tx identity cannot verify another invoice. | Simulated / Real backend evidence pending |
| V13-006 | Invoice Mismatch | Payment / invoice mismatch fails secure. | Simulated / Real backend evidence pending |
| V13-007 | Timeout / Ambiguous | Timeout or ambiguous verification is not treated as verified. | Simulated / Real backend evidence pending |
| V13-008 | Durable Write Failure | Access remains locked if receipt/evidence durable write fails. | Simulated / Real backend evidence pending |
| V13-009 | Public Verify Unknown Pair | Unknown or mismatched receipt/evidence pair returns not verified. | Simulated / Real backend evidence pending |
| V13-010 | Safe Error Output | Public errors do not expose secrets, stack traces, or internals. | Simulated / Real backend evidence pending |
| V13-011 | Fee Required / Missing | Fee-required flow does not finalize before fee verification. | Simulated / Real backend evidence pending |
| V13-012 | Fee Verified | Payment + fee verified flow may finalize receipt/evidence/access/public verify. | Simulated / Real backend evidence pending |
| V13-013 | Boundary Claims | Production, audit, custody, escrow, partnership, and unsupported split claims remain false. | Simulated / Real backend evidence pending |
| V13-014 | Agent Readability | Agent-readable trust state remains read-only preview. | Static preview open |
| V13-015 | Agent Execution | No autonomous payment, unlock, receipt, evidence, MCP, or tool execution is enabled. | Boundary active |
| V13-016 | Privacy-Aware Evidence | Public evidence remains minimum, public-safe, and non-sensitive. | Boundary active |
| V13-017 | Privacy Protocol Claim | No SilentSwap-like private transaction or hidden blockchain transaction claim. | Boundary active |
| V13-018 | Deployment Boundary | No new serverless API function added while Vercel Hobby function limit is reached. | Boundary active |

---

## Public Surface Validation

V13 Public Surface Validation passed 12/12 public-surface checks.

This validates public page availability and claim-boundary language.

It does not validate real backend behavior.

It does not validate real Pi payment endpoints.

It does not validate real database durability.

---

## Backend Policy Simulation

V13 Backend Policy Simulation passed 12/12 client-side policy checks.

This validates expected policy behavior in a public-safe static simulation.

It does not use a new serverless API function.

It does not call real Pi payment endpoints.

It does not test real database durability.

It does not mean overall V13 backend hardening has passed.

---

## Agent-Readable Boundary

Agent-readable trust preview is open as static preview only.

Current boundary:

- No MCP endpoint
- No tool endpoint
- No agent execution
- No autonomous payment
- No autonomous access unlock
- No receipt creation by agent
- No evidence creation by agent

Agent-readable does not mean agent-executable.

MCP/tool endpoints come only after real backend hardening.

---

## Privacy-Aware Boundary

SafeGate is privacy-aware today, not a privacy protocol.

Current boundary:

- Minimum public-safe evidence
- No passphrase collection
- No private key collection
- No raw secrets in public evidence
- No sensitive customer data in public verify
- Limited receipt/evidence/public verify output
- No SilentSwap-like private transaction claim
- No hidden blockchain transaction claim

Future framing:

SilentSwap can protect payment privacy.

SafeGate can verify what happened after the private payment.

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

---

## Current Safe Statement

V13 public surface validation has passed.

V13 backend policy simulation has passed.

Agent-readable trust preview is open.

SafeGate is privacy-aware today, not a privacy protocol.

Real backend hardening evidence has not passed yet.

SafeGate is not production-ready.

SafeGate is not formally audited.

SafeGate is not claiming official Pi partnership.
