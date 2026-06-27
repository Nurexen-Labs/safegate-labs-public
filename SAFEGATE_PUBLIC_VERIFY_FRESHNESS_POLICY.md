# SafeGate Public Verify Freshness Policy

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Purpose

This document defines SafeGate’s public verify freshness direction.

Public verify should not become a stale or misleading trust signal.

A record that was verified at one time may later become expired, revoked, disputed, stale, unavailable, or review-required.

This is a policy direction document.

This is not a production-readiness claim.

This is not a formal audit.

This is not a completed freshness implementation claim.

---

## Current Status

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| Public Verify Freshness | Documented direction |
| Pilot Readiness | Open |
| Production Readiness | Not claimed |
| Pi Mainnet Settlement | Not claimed |
| Official Pi Partnership | Not claimed |
| Formal Third-Party Audit | Not claimed |
| Security Certification | Not claimed |

---

## Completed Evidence Context

SafeGate has passed two important controlled evidence milestones:

1. Controlled Pi Testnet Payment Spine
2. V9.1 Safe Negative Backend Validation

These milestones show:

- backend-verified payment-state direction
- receipt/evidence generation after verified state
- access unlock after backend-verified receipt
- public verify minimum-disclosure direction
- selected missing/unknown public verify inputs rejected safely
- public verify unknown pair did not return active verified trust
- no obvious secret/internal leak terms detected in selected validation responses

They do not prove full public verify freshness implementation.

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

This validation does not prove full public verify freshness, revocation, dispute handling, expiry handling, real duplicate approve/complete behavior, Pi API timeout behavior, Supabase failure recovery, production readiness, or formal audit completion.

---

## Why Freshness Matters

Public verify is useful only if it is not misleading.

A public verify response should answer:

- Was this outcome verified?
- When was it verified?
- Is it still active?
- Has it expired?
- Was it revoked?
- Is it disputed?
- Is the result stale?
- Is the backend unavailable?
- Should the record be reviewed manually?
- Does the response avoid exposing sensitive data?

Without freshness, a record that was once true may be misunderstood as permanently active.

---

## Core Freshness Principle

Public verify should distinguish between:

- historical verification
- current trust status
- current access status
- current receipt/evidence status
- current freshness status
- current integrity status if implemented

A verified payment at one point in time does not always mean access remains active forever.

---

## Minimum Public Verify Fields

A stronger public verify response may eventually include:

- verified
- publicSafe
- minimumDisclosure
- paymentState
- accessState
- receiptStatus
- evidenceStatus
- freshnessStatus
- integrityStatus
- issuedAt
- verifiedAt
- lastCheckedAt
- updatedAt
- expiresAt if implemented
- privateDataIncluded
- secretsIncluded
- paymentIdIncluded
- txidIncluded
- customerDataIncluded

Public verify should remain minimum-disclosure.

---

## Public Verify Must Not Expose

Public verify should not expose:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- service role key
- raw Pi API response
- raw backend logs
- private customer data
- private wallet data
- internal database details
- internal stack traces

The V9.1 validation confirmed selected responses did not show obvious secret/internal leak terms.

Deeper safe error testing remains open.

---

## Freshness Status Direction

Future public verify may use freshness states such as:

- ACTIVE
- EXPIRED
- REVOKED
- DISPUTED
- STALE
- UNAVAILABLE
- REVIEW_REQUIRED
- FRESHNESS_NOT_IMPLEMENTED

These statuses should help prevent misleading public trust claims.

---

## ACTIVE

Meaning:

The record is currently considered active.

Expected conditions:

- receipt/evidence pair is valid
- access state is active or correctly unlocked
- record is not expired
- record is not revoked
- record is not disputed
- backend state is readable
- public verify response is current enough

Boundary:

ACTIVE should not expose sensitive payment identifiers.

---

## EXPIRED

Meaning:

The record was previously valid but is no longer active due to time or policy.

Possible examples:

- access window expired
- service period ended
- invoice expired before finalization
- receipt is outside valid display period
- pilot evidence is historical only

Expected behavior:

- do not show active trust
- show minimum-disclosure expired status
- avoid exposing sensitive identifiers

---

## REVOKED

Meaning:

The record was previously issued but later revoked.

Possible examples:

- merchant revoked access
- backend detected invalid state later
- receipt/evidence was corrected
- administrative review invalidated the trust state

Expected behavior:

- do not show active verified trust
- show minimum-disclosure revoked status
- preserve evidence trail if safe

---

## DISPUTED

Meaning:

The record is under dispute or review.

Possible examples:

- user disputes access
- merchant disputes outcome
- payment/outcome mismatch reported
- receipt/evidence mismatch detected
- support case opened

Expected behavior:

- do not show clean active trust without context
- show review-required or disputed status
- preserve public-safe disclosure

---

## STALE

Meaning:

The record may be old or not recently checked.

Possible examples:

- public verify record has not been refreshed
- backend read is old
- lastCheckedAt is outside acceptable window
- status could have changed after issuance

Expected behavior:

- do not overclaim current active trust
- show stale status
- provide minimum-disclosure state only

---

## UNAVAILABLE

Meaning:

SafeGate cannot currently determine freshness.

Possible examples:

- backend unavailable
- database read failure
- verification service unavailable
- public verify cannot confirm latest state

Expected behavior:

- fail safe
- do not return active verified trust from unavailable state
- avoid exposing internal error details

---

## REVIEW_REQUIRED

Meaning:

The record needs manual or deeper review.

Possible examples:

- ambiguous backend state
- partial write state
- receipt/evidence mismatch
- duplicate callback conflict
- replay suspicion
- integrity mismatch if implemented

Expected behavior:

- do not show active verified trust
- show review-required status
- preserve public-safe boundaries

---

## FRESHNESS_NOT_IMPLEMENTED

Meaning:

Freshness logic is not yet fully implemented.

This is acceptable as a transparent current boundary.

Expected behavior:

- do not overclaim freshness
- state that freshness is a planned direction
- avoid implying active status forever

---

## Freshness Matrix

| Scenario | Expected Public Verify Result | Current Status |
|---|---|---|
| Valid controlled receipt/evidence | minimum-disclosure verified result | PASSED controlled flow |
| Missing receiptId | reject safely | PASSED V9.1 validation |
| Missing evidenceId | reject safely | PASSED V9.1 validation |
| Unknown pair | not active verified trust | PASSED V9.1 validation |
| Mismatched real pair | not verified / review-required | Open target |
| Expired record | expired / not active | Future target |
| Revoked record | revoked / not active | Future target |
| Disputed record | disputed / review-required | Future target |
| Stale record | stale / review-required | Future target |
| Backend unavailable | unavailable / fail secure | Future target |
| Integrity mismatch | review-required / integrity mismatch | Future target |

---

## Relationship To V9.1 Validation

The V9.1 safe negative backend validation strengthens freshness direction because it confirmed selected basic public verify safety rules:

- missing public verify inputs are rejected safely
- unknown public verify pair does not return active verified trust
- selected responses do not show obvious secret/internal leak terms

This is a foundation for future freshness.

It is not a complete freshness implementation.

---

## Relationship To Receipt Integrity

Freshness and integrity should eventually work together.

Possible future public verify fields:

- freshnessStatus
- integrityStatus
- receiptStatus
- evidenceStatus
- accessStatus
- lastCheckedAt
- verifiedAt
- issuedAt
- expiresAt

If integrity is invalid, freshness should not show clean active trust.

If freshness is stale, integrity alone should not imply current active trust.

---

## Relationship To Access State

Access state should not be treated as permanently active without context.

Possible access states:

- LOCKED_UNTIL_BACKEND_VERIFIED_PAYMENT
- ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- ACCESS_EXPIRED
- ACCESS_REVOKED
- ACCESS_DISPUTED
- ACCESS_REVIEW_REQUIRED
- ACCESS_UNAVAILABLE

Public verify should eventually show enough minimum-disclosure state to avoid misleading access interpretation.

---

## Safe Error And Freshness

If public verify cannot determine current status, it should fail safe.

It should not expose:

- stack traces
- database errors
- raw Supabase internals
- raw Pi API responses
- environment variables
- service role keys
- access tokens

It should return a safe public status such as:

- UNAVAILABLE
- REVIEW_REQUIRED
- STALE

---

## Current Boundary

SafeGate currently does not claim:

- completed public verify freshness implementation
- automatic expiry enforcement
- revocation system
- dispute workflow
- production-grade freshness model
- independent freshness verification
- formal audit completion

SafeGate does claim:

- freshness policy direction is documented
- public verify should avoid misleading stale trust
- selected missing/unknown public verify cases passed V9.1 validation
- production readiness is not claimed

---

## Remaining Freshness Work

Remaining work includes:

- define freshnessStatus in public verify responses
- define lastCheckedAt
- define updatedAt
- define verifiedAt
- define issuedAt
- define optional expiresAt
- define active / expired / revoked / disputed / stale / unavailable / review-required
- implement stale record behavior
- implement revoked record behavior
- implement disputed record behavior
- test public verify mismatched receipt/evidence pair
- test backend unavailable public verify behavior
- test safe error responses
- connect freshness with receipt integrity
- document pilot-ready freshness boundaries

---

## Reviewer Questions

A reviewer should ask:

1. Does public verify show current status or only historical verification?
2. Can a verified record become expired?
3. Can a verified record be revoked?
4. Can a verified record be disputed?
5. What happens if backend state is unavailable?
6. What happens if public verify reads stale state?
7. Does public verify expose sensitive payment data?
8. Does public verify return active trust for unknown pair?
9. How is freshness connected to receipt integrity?
10. What should a merchant/user trust from the public verify response?

---

## Safe Public Language

SafeGate may say:

- public verify freshness policy is documented
- public verify should not become a stale trust signal
- missing/unknown public verify inputs passed selected V9.1 safe negative validation
- unknown public verify pair did not return active verified trust
- freshness implementation remains a hardening target
- production readiness is not claimed
- formal audit is not claimed

SafeGate should not say:

- freshness system is complete
- public verify is production-grade
- revocation system is complete
- dispute workflow is complete
- stale trust risk is fully solved
- independent verification is complete
- formal audit passed

---

## Final Freshness Position

SafeGate has a clear public verify freshness direction.

SafeGate has passed selected public verify negative checks in V9.1.

SafeGate still needs deeper freshness, expiry, revocation, dispute, stale-state, unavailable-state, and integrity-connected implementation.

SafeGate remains not production-ready and not formally audited.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
