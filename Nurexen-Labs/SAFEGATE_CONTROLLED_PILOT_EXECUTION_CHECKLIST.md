# SafeGate Controlled Pilot Execution Checklist

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Checklist Status

**Status:** Prepared for controlled pilot planning  
**Current Phase:** Pilot Readiness  
**Controlled Pi Testnet Payment Spine:** PASSED  
**Production Claim:** No  
**Pi Mainnet Settlement Claim:** No  
**Official Pi Partnership Claim:** No  
**Formal Audit Claim:** No  

---

## Purpose

This checklist defines what must be ready before SafeGate moves from controlled Testnet evidence toward a controlled pilot execution.

This is not a production launch checklist.

This is not a Mainnet settlement checklist.

This is not a regulatory approval checklist.

This is a controlled pilot preparation checklist.

---

## Current Foundation

SafeGate has already demonstrated:

- V9 invoice creation
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend approval / completion flow
- backend-verified payment state
- receipt and evidence generation
- public verification
- access unlock after backend-verified receipt

Current strongest evidence:

- Controlled Pi Testnet Payment Spine: PASSED
- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- public verify: confirmed
- minimumDisclosure: true
- publicSafe: true

---

## Controlled Pilot Definition

A controlled pilot means:

- limited scope
- limited users
- limited merchant scenario
- controlled evidence capture
- no production readiness claim
- no Mainnet settlement claim unless separately verified
- no public exposure of sensitive identifiers
- no broad onboarding
- no promise of commercial operation

A controlled pilot is used to answer one question:

Can SafeGate reliably verify post-payment outcomes in a realistic but limited merchant-style scenario?

---

## Pre-Pilot Requirements

Before starting a controlled pilot, the following should be ready:

- Main Review Hub is live
- V9 Payment Spine Evidence page is live
- Public-safe evidence pack is available
- External Technical Review Note is available
- Controlled Pi Testnet pass note is available
- Pilot Review Index is updated
- Pilot Readiness page is updated
- README and LINKS are updated
- raw evidence screenshots are stored privately
- public screenshots are redacted
- no secrets or private payment identifiers are exposed

---

## Merchant Scenario Requirement

A controlled pilot should use one simple merchant-style scenario.

Recommended first pilot scenario:

- one merchant
- one product or service
- one controlled payment amount
- one buyer/tester
- one invoice
- one payment
- one receipt/evidence pair
- one public verify result
- one final pilot review note

The scenario should be simple enough to inspect manually.

Do not start with many merchants.

Do not start with multiple products.

Do not start with production promises.

---

## Payment Flow Checklist

The controlled pilot flow should verify:

1. Invoice is created.
2. Initial access state is locked.
3. Pi authentication succeeds.
4. Payment scope is granted.
5. Pi wallet payment is completed in the intended environment.
6. Backend approval is called.
7. Backend completion is called.
8. Backend-verified payment state is reached.
9. Receipt is generated after verified state.
10. Evidence is generated after verified state.
11. Public verify confirms the record.
12. Access unlock happens only after backend-verified receipt.

---

## State Machine Checklist

The payment state machine should be reviewed for:

- invoice created state
- pending / approval state
- server-approved state
- completion failed state
- completion retry-safe state
- backend-verified payment state
- receipt/evidence created state
- access unlocked state
- failed state
- cancelled state
- expired state
- disputed state
- refunded state

Illegal transitions should be blocked.

Examples of blocked transitions:

- invoice created → access unlocked
- frontend callback → access unlocked
- failed payment → receipt generated
- missing txid → payment verified
- payment mismatch → receipt generated
- unknown receipt/evidence pair → public verify true

---

## Fail-Secure Checklist

The controlled pilot must fail secure if:

- Pi API is unavailable
- Pi API times out
- Pi API returns an error
- paymentId is missing
- txid is missing
- invoiceId is missing
- invoice does not exist
- paymentId belongs to another invoice
- receipt/evidence pair does not match
- Supabase durable state is unavailable
- backend cannot verify payment state
- public verify cannot find the record

Fail-secure means:

- no fake success
- no receipt
- no evidence
- no access unlock
- no public verify true

---

## Idempotency Checklist

The pilot should check duplicate calls:

- duplicate invoice create
- duplicate approve callback
- duplicate complete callback
- duplicate receipt/evidence request
- duplicate public verify request

Expected behavior:

- duplicate approve should not create duplicate success
- duplicate complete should not create duplicate receipt/evidence
- duplicate receipt/evidence should return existing record
- duplicate public verify should return the same minimum-disclosure result
- duplicate or replayed paymentId should not bind to a different invoice

---

## Replay Protection Checklist

The pilot should check:

- paymentId cannot be reused for a different invoice
- verified txid cannot be replaced with another txid
- receipt/evidence pair cannot be mixed
- public verify fails for mismatched receipt/evidence pair
- malformed receipt/evidence IDs are rejected
- unknown receipt/evidence IDs return not found

---

## Public Verify Checklist

Public verify should expose only public-safe fields.

Allowed public-safe fields:

- verified
- receiptId
- evidenceId
- paymentState
- accessState
- createdAt
- updatedAt
- minimumDisclosure
- publicSafe
- accessUnlocked

Public verify should not expose:

- paymentId
- txid
- raw Pi API response
- service role key
- private wallet data
- customer data
- access token
- internal secrets
- private logs

---

## Evidence Capture Checklist

For each controlled pilot run, capture:

- date
- environment
- tester type
- browser/app context
- invoice creation result
- initial locked state
- authentication result
- payment result
- backend verified state
- receipt/evidence result
- public verify result
- final access state
- observed errors
- reviewer notes

Do not capture or publish private data unnecessarily.

---

## Screenshot Redaction Checklist

Before sharing screenshots, redact:

- paymentId
- txid
- accessToken
- uid
- wallet address
- recipient address
- app identifiers if unnecessary
- raw internal logs
- long private identifiers
- anything that looks like a token, key, secret, or private address

Keep visible:

- PAYMENT_VERIFIED_TESTNET
- ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- verified: true
- minimumDisclosure: true
- publicSafe: true
- paymentIdIncluded: false
- txidIncluded: false
- secretsIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

---

## Reviewer Questions

After a controlled pilot run, reviewers should answer:

1. Did access remain locked before backend verification?
2. Did receipt/evidence generate only after verified payment state?
3. Did public verify expose only minimum-disclosure fields?
4. Did duplicate callbacks behave safely?
5. Did failed states avoid fake success?
6. Did error responses avoid leaking secrets?
7. Did the user journey make sense?
8. Did the merchant proof make sense?
9. What would break first if this moved to a real pilot?
10. What must be hardened before production?

---

## Operational Boundaries

During controlled pilot execution:

- do not invite broad public users
- do not claim production readiness
- do not claim Pi Mainnet settlement
- do not claim official Pi partnership
- do not collect private keys
- do not collect seed phrases
- do not expose service keys
- do not publish raw payment identifiers
- do not publish unredacted logs
- do not promise refunds, disputes, or legal compliance automation unless implemented

---

## Minimum Controlled Pilot Success Criteria

A controlled pilot run can be marked successful only if:

- invoice creation succeeds
- access starts locked
- authentication succeeds
- payment flow succeeds in the intended environment
- backend verified state is reached
- receipt/evidence is created after verified state
- public verify confirms minimum-disclosure record
- access unlock happens after backend-verified receipt
- no sensitive payment identifiers are exposed in public evidence
- reviewer notes are captured

---

## Pilot Failure Criteria

A controlled pilot run should be marked failed or partial if:

- payment cannot be authenticated
- payment cannot be completed
- backend verification fails
- receipt/evidence is created before verified payment state
- access unlocks too early
- public verify exposes private payment data
- duplicate callbacks create inconsistent state
- state mismatch occurs
- raw secrets or private identifiers appear in public output

---

## Current Known Remaining Work

Before broader pilot or production claims, SafeGate still needs:

- additional external controlled payment-spine test
- redacted public evidence package
- stronger production-grade rate limiting
- deeper idempotency testing
- deeper replay testing
- formal threat model packaging
- third-party technical review
- clearer merchant onboarding boundaries
- commercial pilot terms
- operational incident plan
- no production claim until controls are stronger

---

## Recommended Next Pilot Shape

Recommended next pilot shape:

- one trusted external tester
- one controlled merchant-style scenario
- one Pi Browser run
- one 0.01 Test-Pi payment
- one public verify result
- one redacted evidence note
- one reviewer feedback note

Goal:

Repeat the controlled payment-spine pass without exposing sensitive data.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
