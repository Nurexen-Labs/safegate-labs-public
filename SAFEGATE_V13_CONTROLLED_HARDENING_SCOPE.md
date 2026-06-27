# SafeGate V13 Controlled Hardening Scope

## Status

SafeGate V13 is now opened as a controlled backend hardening scope.

V13 is not complete.

V13 is not passed.

V13 is not production readiness.

V13 is not a formal audit.

V13 is not a marketing version.

---

## Why V13 Is Opened

V13 is opened because a concrete implementation target has been selected:

Controlled backend hardening against the highest-risk post-payment trust failures.

This follows:

- V10 Submission Ready
- V10.1 Review Feedback Open
- V11 Hardening Backlog
- V11 Hardening Test Plan
- V11 Implementation Sprint Scope
- V11 Developer Handoff

---

## Current Position

| Area | Status |
|---|---|
| Controlled Pi Testnet Payment Spine | PASSED |
| V9.1 Safe Negative Backend Validation | PASSED |
| V9.1 Public Review Sync | COMPLETE |
| V10 Submission Ready | READY FOR SERIOUS TECHNICAL REVIEW |
| V10.1 Review Feedback | OPEN |
| V11 Hardening Backlog | OPEN |
| V11 Hardening Test Plan | OPEN |
| V11 Implementation Sprint Scope | OPEN |
| V13 Controlled Hardening Scope | OPEN |
| V13 Implementation | NOT COMPLETE |
| V13 Evidence | NOT YET PASSED |
| Production Readiness | NOT CLAIMED |
| Formal Third-Party Audit | NOT CLAIMED |

---

## V13 One-Line Definition

V13 is the controlled backend hardening sprint for SafeGate’s post-payment trust architecture.

---

## Core Rule

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Unknown, missing, ambiguous, duplicate, replayed, mismatched, expired, failed, stale, or unavailable states should fail secure.

---

## V13 Primary Goal

Reduce real backend trust risk before any broader pilot execution.

V13 focuses on the highest-risk cases:

- duplicate callbacks
- duplicate approve
- duplicate complete
- duplicate receipt creation
- duplicate evidence creation
- paymentId replay
- txid replay
- paymentId / invoice mismatch
- Pi API timeout
- Pi API ambiguous response
- durable state failure
- incomplete receipt / evidence state
- public verify unknown or mismatched pair
- safe error handling
- access unlock regression

---

## V13 Included Scope

### 1. Idempotency Guard

Target:

Repeated backend calls must not create conflicting payment, receipt, evidence, or access states.

Expected behavior:

- duplicate approve returns safe deterministic response
- duplicate complete returns safe deterministic response
- duplicate receipt/evidence creation does not create conflicting records
- access state remains consistent
- public verify remains consistent

Status:

OPEN

---

### 2. Replay Protection

Target:

A paymentId or txid must not be reusable against another invoice or trust record.

Expected behavior:

- reused paymentId fails secure
- reused txid fails secure when txid is used
- payment from another invoice cannot unlock access
- no receipt/evidence is created for replay attempt
- public verify does not show active verified trust for replay attempt

Status:

OPEN

---

### 3. Payment / Invoice Mismatch Handling

Target:

A valid-looking payment must still match the correct invoice and payment context.

Expected behavior:

- wrong invoiceId fails secure
- wrong paymentId fails secure
- wrong payment context fails secure
- wrong amount fails secure when amount check is available
- no access unlock for mismatch
- no receipt/evidence for mismatch

Status:

OPEN

---

### 4. Pi API Timeout / Ambiguous Response Fail-Secure

Target:

Pi API timeout, backend timeout, malformed response, missing response fields, or ambiguous verification must never be treated as verified.

Expected behavior:

- access remains locked
- no receipt is created
- no evidence is created
- safe error is returned
- no raw Pi API response is exposed publicly

Status:

OPEN

---

### 5. Durable State Failure Fail-Secure

Target:

Database write/read failure or incomplete durable state must not create false trust.

Expected behavior:

- partial write does not unlock access
- receipt without evidence does not show normal active verified trust
- evidence without receipt does not show normal active verified trust
- public verify fails safe on incomplete state
- safe error is returned

Status:

OPEN

---

### 6. Public Verify Unknown / Mismatch Safety

Target:

Unknown, missing, stale, mismatched, or incomplete public verify records must not return active verified trust.

Expected behavior:

- unknown pair returns safe not verified / not found / unavailable state
- mismatched pair does not show active verified trust
- incomplete pair does not show active verified trust
- no private identifiers or secrets are exposed

Status:

OPEN

---

### 7. Safe Error Handling

Target:

Public errors must not expose internal implementation details.

Public output must not expose:

- service role
- secret key
- private key
- passphrase
- environment variable
- stack trace
- raw database error
- raw Pi API response
- sensitive customer data

Status:

OPEN

---

### 8. Access Unlock Regression

Target:

Access must only unlock through the legal backend-verified path.

Expected behavior:

- invoice created state remains locked
- payment pending state remains locked
- timeout state remains locked
- ambiguous state remains locked
- mismatch state remains locked
- replay state remains locked
- incomplete receipt/evidence state remains locked
- verified receipt/evidence state may unlock only through legal backend path

Status:

OPEN

---

## V13 Explicitly Excluded

V13 does not include:

- production launch
- Pi Mainnet settlement claim
- official Pi partnership claim
- formal third-party audit claim
- security certification claim
- complete privacy protocol
- complete AI-agent readiness
- all-chain adapter implementation
- commercial pilot completion claim
- tamper-proof infrastructure claim

---

## V13 Evidence Requirement

V13 cannot be called passed until public-safe evidence exists for:

- duplicate approve behavior
- duplicate complete behavior
- duplicate receipt/evidence behavior
- paymentId replay behavior
- txid replay behavior if applicable
- paymentId / invoice mismatch behavior
- Pi API timeout behavior
- Pi API ambiguous response behavior
- durable state failure behavior
- public verify unknown pair behavior
- public verify mismatched pair behavior
- access unlock regression behavior
- safe error scan behavior

---

## V13 Minimum Test Output

A V13 evidence pack should include:

- test case name
- expected safe behavior
- observed behavior
- result label
- public-safe output summary
- remaining limitation if any

Allowed result labels:

- PASS
- FAIL
- BLOCKED
- NEEDS REVIEW
- NOT IMPLEMENTED

---

## Public-Safe Evidence Rules

Do not expose:

- raw paymentId
- raw txid
- wallet address
- recipient address
- access token
- service role key
- private key
- passphrase
- raw Pi API response
- raw database error
- stack trace
- environment variables
- sensitive customer data

---

## V13 Exit Criteria

V13 can only be considered complete if:

1. Included hardening items have clear observed behavior.
2. Duplicate behavior is deterministic.
3. Replay attempts fail secure.
4. Mismatch attempts fail secure.
5. Timeout and ambiguous responses fail secure.
6. Durable state failure does not create false trust.
7. Public verify unknown/mismatched pair does not show active verified trust.
8. Access unlock guard still holds.
9. Safe error scan passes.
10. Remaining limitations are documented.
11. No production/audit/partnership claim is added.

---

## V13 Current Safe Statement

SafeGate V13 Controlled Hardening Scope is open.

V13 is not complete.

V13 has not passed evidence validation yet.

SafeGate remains V10 Submission Ready from a public review-positioning perspective.

The next work is backend hardening implementation and public-safe V13 evidence collection.
