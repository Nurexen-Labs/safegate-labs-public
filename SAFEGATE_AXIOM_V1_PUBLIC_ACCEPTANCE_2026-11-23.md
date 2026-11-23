# SafeGate AXIOM V1 — Paid Response Evidence Acceptance

**Status: ACCEPTANCE PASSED (defined V1 scope)**  
**Acceptance date:** 23 November 2026  
**Project:** SafeGate / Nurexen Labs  
**Environment:** Live paid CommerceProof verification endpoint, Base Mainnet  
**Scope:** One internal, owner-funded end-to-end paid verification and durable response-evidence comparison

> AXIOM V1 has completed its *specified technical acceptance test*. This is not a claim of independent delivery confirmation, third-party customer traction, an external audit, or a general-purpose portable receipt product.

## What problem does this address?

A blockchain payment proves that a transfer was settled, but does not by itself show which HTTP verification result a paid service prepared for that request.

SafeGate AXIOM V1 associates a settled payment claim with a server-prepared HTTP response's **status**, **byte length**, and **SHA-256 digest** in a durable record. A separately captured client response can be compared with the recorded values.

## Observed acceptance evidence — 23 November 2026

| Check | Observed result |
| --- | --- |
| Network | Base Mainnet (`eip155:8453`) |
| Payment asset | USDC |
| Single test payment | **0.05 USDC** |
| Payment / claim state | `SETTLED` |
| Verification verdict | `VERIFIED` |
| HTTP response status | `200` |
| Captured response length | **19,549 bytes** |
| Durable HTTP response length | **19,549 bytes** |
| Client-captured vs. durable SHA-256 | **MATCH** |
| Acceptance verdict | **PASS** |

**Public on-chain settlement transaction:**  
[`0xd25117e036772bcde19f9f53acd4816b53de78f11eac1bb6f82436f29c5bbee2`](https://basescan.org/tx/0xd25117e036772bcde19f9f53acd4816b53de78f11eac1bb6f82436f29c5bbee2)

**Response SHA-256 (both sides):**

```text
b4cfd0c17287078b06a84ff605e98ee58ef47a7b1b0dd10a3a385100d52a8072
```

The acceptance operator captured the HTTP response bytes and compared their length and SHA-256 with the corresponding `http_response` fields in the **production durable payment claim**. Both matched. The record also contained the settlement transaction, a `SETTLED` state and a `VERIFIED` verdict.

The public blockchain transaction independently evidences the token transfer. The response digest comparison was performed against the operator's own production record; this document does **not** expose the private production database or assert an independent third-party audit of that record.

## Evidence boundary — essential reading

**What was shown:**

1. One real, internal paid verification settled on Base.
2. The paid endpoint returned an HTTP 200 response with `VERIFIED` as the verification verdict.
3. The HTTP body captured during this test had the same SHA-256 and byte length as the response evidence saved in the durable claim.
4. The durable claim associated the response evidence with the settlement event.

**What was not shown:**

- A general guarantee that every buyer receives every response. The stored record explicitly marks `delivery_proven: false` and `buyer_receipt_proven: false`.
- Independent confirmation that physical goods or services were delivered.
- Independent merchant identity/authorization verification beyond the precise verifier's stated scope.
- A portable, third-party-signed receipt or independently auditable customer acknowledgment.
- A purchase by an unrelated external customer: **this was an internal acceptance payment**.
- Confirmation of any later facilitator-to-SafeGate net payout. Settlement and later disbursement are separate.
- A completed human-facing proof or receipt application.

The response digest applies to the **exact server-prepared body before proxy transformations**. In this acceptance test, the separately captured HTTP response matched that digest. A match is meaningful for this test; it is not an assertion of universal end-to-end delivery.

The underlying CommerceProof verification also has its own assurance boundaries. In particular, `VERIFIED` in the tested sample does not mean that goods were delivered: the proof reported `commerce_verified: false`.

## API context

Paid verification endpoint:

```text
POST https://pay.safegatelabs.xyz/api/commerceproof/verify-paid
```

Protocol and scope: x402 v2 on Base Mainnet; USDC payment under the currently published endpoint terms. Refer to the official published documentation for up-to-date protocol requirements and pricing.

## Product and commercial interpretation

This acceptance test demonstrates that SafeGate can bind a paid verification request to a durable record of the service response's exact content fingerprint. It adds an evidence and reconciliation capability for **AI agents, paid APIs, and service integrations**.

The technical milestone is completed for AXIOM V1's defined scope. **Recurring external revenue, third-party usage and product-market fit remain separate commercial goals** and are not established by this test.

## Relationship to future Human Proof

**SafeGate Human Proof V1 is only a future proposal. It is NOT implemented by AXIOM V1.** Its envisioned human-friendly page, PDF/QR sharing features, receipt verification model and merchant pilot require separate design, testing, and explicit approval.

## Operational policy

This is **public documentation only**. No backend code, deployment instructions, private Redis keys, secrets, authorization nonce or credentials are included. Publishing this file does not authorize any production, database, environment or wallet changes, and does not require another payment.

---

**SafeGate / Nurexen Labs**  
**Document classification:** Public technical acceptance summary  
**V1 technical acceptance:** PASS  
**Independent buyer delivery proof:** NOT CLAIMED  
**External customer revenue:** NOT CLAIMED
