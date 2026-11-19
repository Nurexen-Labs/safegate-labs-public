# Architecture

The payment is not the product. What can be proved after the payment is the product.

SafeGate keeps one core and pushes rail-specific logic into adapters. The core does not bend to
an integration.

## Paid verification flow

1. A client calls the paid endpoint with no payment header.
2. SafeGate returns `402 Payment Required` with the full terms: scheme, network, asset, exact
   amount, payee, timeout and the resource being paid for. The same object is carried in both the
   body and the `Payment-Required` header, so price and payee cannot drift between them.
3. The client signs an EIP-3009 `transferWithAuthorization` for the exact amount and presents it.
4. SafeGate binds the presented requirements to its own. Any mismatch in scheme, network, asset,
   payee, amount or resource is refused, and nothing is charged.
5. Verification runs.
6. Settlement is attempted **only** when the verdict is `VERIFIED`. A `FAILED` or
   `INDETERMINATE` verdict never reaches the settle call.
7. The result is recorded durably before settlement is attempted. If that record cannot be
   advanced, settlement is deliberately not attempted and the caller is told so.

Re-presenting the same authorization returns the stored result. There is no second charge.

## What verification checks

| Layer | Independent of SafeGate? |
|---|---|
| Signer key pinned to a published registry | No. It makes the signature independent of the proof. |
| NSGoods signed screening, checked against the NSGoods manifest | Yes |
| On-chain legs confirmed by two independent RPC providers that agree | Yes |

A proof is read tolerantly and forwarded untouched. Normalizing a payment payload before passing
it on caused three separate field mismatches during integration, so the rule now is: read
tolerantly, forward unchanged.

## Assurance levels

Every proof names how strong its own evidence is.

- **CLAIMED** — a provider or execution system asserts that an outcome happened.
- **OBSERVED** — SafeGate directly observed the paid request, execution, response or equivalent.
- **VALIDATED** — an independent mechanism verified the field, with the mechanism named.
- **THIRD-PARTY ATTESTED** — a separately identified external party contributed evidence under
  its own trust.

There is no silent upgrade between levels. `commerce_verified` is never reported as true.

## Payment custody

Payment settles to the facilitator's treasury, not to SafeGate. SafeGate is credited as the
registered payee and paid out separately. This separation is deliberate: SafeGate never custodies
buyer funds and holds no prepaid credits.

## Trust objects

Request ID, payment transaction, payment sender, merchant receiver, amount in base units,
screening verdict and its raw signed bytes, signer key id, assurance level, field-level
provenance, and the signed CommerceProof itself.

The request ID appears in every section of a proof, including inside the third party's own signed
bytes. A proof whose request ID does not match across all of them is refused.
