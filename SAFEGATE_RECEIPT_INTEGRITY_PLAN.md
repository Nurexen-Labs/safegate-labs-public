# SafeGate Receipt Integrity Plan

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document defines SafeGate’s receipt and evidence integrity direction.

The purpose is to clarify how SafeGate can move from minimum-disclosure public verification toward stronger tamper-evident receipt and evidence records.

This is a plan.

This is not a production-readiness claim.

This is not a formal audit.

This is not a completed cryptographic proof system.

This is not a tamper-proof infrastructure claim.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Receipt Integrity | Planned / documented direction |
| Pilot Readiness | Open |
| Production Readiness | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Tamper-Proof Infrastructure | Not claimed |
| Independent Cryptographic Verification | Not yet claimed |

---

## Completed Evidence Context

SafeGate has already passed two important controlled evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

These milestones show:

- backend-verified payment state direction
- receipt/evidence generated after verified payment state
- access unlock after backend-verified receipt
- selected negative backend cases rejected safely
- public verify minimum-disclosure direction
- no obvious secret/internal leak terms detected in selected validation responses

They do not prove full receipt integrity.

---

## Controlled Pi Testnet Payment Spine — PASSED

SafeGate completed a controlled Pi Testnet payment-spine execution in Pi Browser.

Observed public-safe result:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- public verify: confirmed
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

Related document:

SAFEGATE_CONTROLLED_PI_TESTNET_PAYMENT_SPINE_PASSED.md

Live evidence page:

https://www.safegatelabs.xyz/v9-pi-payment-spine-evidence.html

---

## V9.1 Safe Negative Backend Validation — PASSED

SafeGate completed a live V9.1 safe negative backend behavior validation.

Observed validation result:

- total visible checks: 18
- passed checks: 18
- failed checks: 0
- warning checks: 0

Confirmed selected public-safe behavior:

- invoice creation kept access locked
- invoice creation did not create receipt/evidence
- receipt/evidence before verified payment was rejected safely
- missing invoiceId was rejected safely
- missing paymentId was rejected safely
- public verify missing inputs were rejected safely
- public verify unknown pair did not return active verified trust
- validation responses did not show obvious secret/internal leak terms

Related document:

SAFEGATE_V9_1_SAFE_NEGATIVE_BACKEND_VALIDATION_PASSED.md

Live validation page:

https://www.safegatelabs.xyz/v9-1-backend-behavior-validation.html

Boundary:

This validation does not prove receipt cryptographic integrity, tamper-proof infrastructure, independent verification, production readiness, or formal audit completion.

---

## Why Receipt Integrity Matters

Minimum-disclosure public verify protects privacy.

However, privacy alone is not integrity.

A stronger receipt integrity layer should help answer:

- Was this receipt created by SafeGate backend?
- Was it created after backend-verified payment state?
- Is the receipt bound to the correct invoice?
- Is the evidence bound to the correct receipt?
- Has the receipt payload changed?
- Has the evidence payload changed?
- Is the public verify record stale, revoked, disputed, or still active?
- Can a reviewer detect tampering without seeing sensitive payment identifiers?

---

## Core Receipt Integrity Principle

Receipt integrity should preserve this rule:

No receipt, no evidence, and no access unlock before backend-verified payment state.

Receipt integrity should strengthen, not replace, backend verification.

A signed or hashed receipt is only meaningful if it is generated from a valid backend state.

---

## Planned Receipt Integrity Components

Future receipt integrity should include:

- canonical receipt payload
- canonical evidence payload
- receipt hash
- evidence hash
- receipt/evidence binding
- invoice binding
- verified payment-state binding
- issuedAt timestamp
- optional expiresAt timestamp
- server-side HMAC or signature
- integrityStatus field
- freshnessStatus field
- tamper-detection behavior
- minimum-disclosure public verify metadata

---

## Canonical Receipt Payload

A canonical receipt payload should use stable field order and stable values.

Possible public-safe fields:

- receiptId
- invoiceId
- evidenceId
- paymentState
- accessState
- receiptStatus
- evidenceStatus
- issuedAt
- verifiedAt
- minimumDisclosure
- publicSafe
- privateDataIncluded
- secretsIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded

Sensitive identifiers should not be included in public payloads.

Raw paymentId and txid should not be exposed publicly.

---

## Canonical Evidence Payload

A canonical evidence payload should summarize the verified outcome without exposing sensitive payment data.

Possible fields:

- evidenceId
- receiptId
- invoiceId
- evidenceType
- evidenceStatus
- paymentState
- accessState
- verifiedAt
- issuedAt
- source
- publicSafe
- minimumDisclosure
- privateDataIncluded
- secretsIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded

---

## Hash Direction

Future receipt/evidence records may include:

- receiptHash
- evidenceHash
- combinedTrustHash

Hash purpose:

- detect payload changes
- bind receipt and evidence
- make tampering easier to detect
- support future audit trail
- support reviewer confidence

Boundary:

A hash alone does not prove who created the record.

A hash alone is not a signature.

A hash alone is not a formal audit.

---

## Signature / HMAC Direction

A stronger future layer may use a server-side HMAC or signature.

Possible fields:

- signatureVersion
- signatureAlgorithm
- receiptSignature
- evidenceSignature
- signedAt
- keyVersion

Purpose:

- prove receipt/evidence was generated by SafeGate backend
- detect tampering
- support future key rotation
- support future auditability

Boundary:

SafeGate does not currently claim completed independent cryptographic verification.

SafeGate does not currently claim tamper-proof infrastructure.

---

## Binding Rules

Receipt and evidence should be bound together.

Expected binding:

- receiptId binds to invoiceId
- evidenceId binds to receiptId
- receipt binds to backend-verified payment state
- evidence binds to receipt
- access unlock binds to backend-verified receipt
- public verify binds to receipt/evidence pair

Invalid behavior:

- receipt without verified payment
- evidence without receipt
- access unlock without receipt
- public verify active trust for unknown pair
- public verify active trust for mismatched pair
- duplicate receipt for replayed payment
- duplicate evidence from duplicate callback

---

## Integrity Status Direction

Future public verify may include an integrity status.

Possible values:

- INTEGRITY_NOT_IMPLEMENTED
- INTEGRITY_VALID
- INTEGRITY_MISMATCH
- INTEGRITY_UNAVAILABLE
- INTEGRITY_REVIEW_REQUIRED

Current boundary:

Integrity is planned.

SafeGate should not currently claim integrity proof is complete.

---

## Freshness Status Direction

Receipt integrity should connect with public verify freshness.

Possible values:

- ACTIVE
- EXPIRED
- REVOKED
- DISPUTED
- STALE
- UNAVAILABLE
- REVIEW_REQUIRED

A receipt that was valid at one time may later require freshness context.

---

## Public Verify Integrity Direction

Public verify should eventually show a minimum-disclosure integrity summary.

Public-safe possible fields:

- verified
- publicSafe
- minimumDisclosure
- receiptStatus
- evidenceStatus
- accessState
- integrityStatus
- freshnessStatus
- issuedAt
- verifiedAt
- lastCheckedAt
- privateDataIncluded
- secretsIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded

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

## Tamper Detection Direction

If receipt/evidence payload changes, future verification should detect it.

Expected behavior:

- recompute receipt hash
- compare stored receipt hash
- recompute evidence hash
- compare stored evidence hash
- verify signature or HMAC if implemented
- return integrity mismatch if invalid
- do not return active verified trust from mismatched payload

---

## Failure Behavior

If integrity validation is unavailable or ambiguous, SafeGate should fail safe.

Expected behavior:

- do not claim integrity valid
- do not expose secrets
- return review-required or unavailable state
- preserve minimum disclosure
- avoid false verified trust
- log internally if safe

---

## Relationship To V9.1 Validation

The V9.1 safe negative backend validation strengthened the integrity direction by confirming selected pre-integrity behavior:

- invoice creation does not create receipt/evidence
- receipt/evidence before verified payment is rejected
- unknown public verify pair does not return active verified trust
- selected responses do not show obvious secret/internal leak terms

These are prerequisites for receipt integrity.

If receipt/evidence can be created before verified payment, integrity does not matter.

---

## Remaining Receipt Integrity Work

Remaining work includes:

- define canonical receipt payload
- define canonical evidence payload
- implement receiptHash
- implement evidenceHash
- implement combinedTrustHash if useful
- implement server-side HMAC or signature
- define key versioning
- define tamper-detection behavior
- define public verify integrityStatus
- define freshnessStatus behavior
- test mismatched receipt/evidence pair
- test duplicate receipt/evidence request
- test replayed paymentId behavior
- test replayed txid behavior
- document safe error model

---

## Reviewer Questions

A technical reviewer should ask:

1. What exactly is hashed?
2. Are fields canonicalized?
3. Is the receipt bound to invoice?
4. Is evidence bound to receipt?
5. Is access unlock bound to backend-verified receipt?
6. Can receipt/evidence be duplicated?
7. Can receipt/evidence be changed after issuance?
8. Can public verify detect tampering?
9. Does public verify expose sensitive identifiers?
10. What happens if integrity validation fails?

---

## Safe Public Language

SafeGate may say:

- receipt integrity direction is documented
- receipt/evidence should be generated only after backend-verified payment state
- V9.1 validation confirmed receipt/evidence before verified payment was rejected safely
- future receipt integrity may use hashes, HMAC, or signatures
- tamper-proof infrastructure is not claimed
- independent cryptographic verification is not yet claimed

SafeGate should not say:

- receipts are tamper-proof
- cryptographic integrity is complete
- independent verification is complete
- formal audit passed
- production-grade receipt integrity is ready
- all replay and duplicate risks are solved

---

## Final Receipt Integrity Position

SafeGate has a clear receipt/evidence integrity direction.

SafeGate has not yet completed full cryptographic receipt integrity.

The next engineering step is to implement and test canonical payloads, hashes, signatures/HMAC direction, integrity status, freshness status, and mismatch behavior.

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
