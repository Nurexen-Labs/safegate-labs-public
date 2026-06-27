# SafeGate V13 Endpoint Map Template

## Purpose

This document provides a public-safe endpoint mapping template for SafeGate V13 controlled backend hardening.

V13 is open.

V13 is not complete.

V13 has not passed evidence validation yet.

This template should be filled before implementation changes are made.

---

## Current Position

| Area | Status |
|---|---|
| V10 Submission Ready | READY FOR SERIOUS TECHNICAL REVIEW |
| V10.1 Review Feedback | OPEN |
| V11 Hardening Backlog | OPEN |
| V11 Hardening Test Plan | OPEN |
| V13 Controlled Hardening Scope | OPEN |
| V13 Test Matrix | OPEN |
| V13 Evidence Log | OPEN |
| V13 Endpoint Map Template | OPEN |
| V13 Complete | NOT CLAIMED |
| V13 Passed | NOT CLAIMED |
| Production Readiness | NOT CLAIMED |
| Formal Third-Party Audit | NOT CLAIMED |

---

## Core Rule

No receipt, no evidence, and no access unlock before backend-verified payment state.

Frontend callback alone must never unlock access.

Unknown, missing, ambiguous, duplicate, replayed, mismatched, expired, failed, stale, or unavailable states should fail secure.

---

## Endpoint Map Table

| Endpoint | Method | Purpose | Inputs | Expected Safe Output | Risk Area | Current Status | V13 Action |
|---|---|---|---|---|---|---|---|
| TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |

---

## Suggested Endpoint Categories

Use the sections below to map actual SafeGate endpoints.

---

## 1. Invoice Creation Endpoint

### Endpoint

TBD

### Method

TBD

### Purpose

Create invoice or payment-intent-like record.

### Expected Inputs

- invoiceId
- merchantId
- amount
- currency / Pi context
- metadata if applicable

### Expected Safe Output

- invoice created
- access locked
- no receipt
- no evidence
- no verified payment state yet

### Risk Areas

- duplicate invoice creation
- malformed input
- missing input
- fake merchant context
- public error leakage
- database write failure

### V13 Required Checks

- duplicate creation behavior
- missing input behavior
- invalid input behavior
- database write failure behavior
- access remains locked
- no receipt/evidence created at invoice stage

### Current Status

TBD

### V13 Action

TBD

---

## 2. Payment Approval Endpoint

### Endpoint

TBD

### Method

TBD

### Purpose

Handle payment approval stage when applicable.

### Expected Inputs

- invoiceId
- paymentId
- user / Pi context if applicable
- auth context if applicable

### Expected Safe Output

- approval accepted only if valid
- no access unlock from frontend callback alone
- no final receipt/evidence before backend verification

### Risk Areas

- duplicate approve
- fake paymentId
- paymentId mismatch
- missing invoiceId
- missing paymentId
- raw API error leakage

### V13 Required Checks

- duplicate approve
- missing invoiceId
- missing paymentId
- paymentId / invoice mismatch
- no receipt/evidence before verified state
- no access unlock before verified state

### Current Status

TBD

### V13 Action

TBD

---

## 3. Payment Completion Endpoint

### Endpoint

TBD

### Method

TBD

### Purpose

Handle payment completion / verification stage when applicable.

### Expected Inputs

- invoiceId
- paymentId
- txid if applicable
- backend verification context

### Expected Safe Output

- payment verified only after backend verification
- safe failure for timeout / ambiguous response
- no raw Pi API response exposed publicly

### Risk Areas

- duplicate complete
- paymentId replay
- txid replay
- timeout treated as verified
- ambiguous response treated as verified
- malformed API response
- database write failure

### V13 Required Checks

- duplicate complete
- paymentId replay
- txid replay if applicable
- timeout handling
- ambiguous response handling
- database write failure handling
- safe error output

### Current Status

TBD

### V13 Action

TBD

---

## 4. Receipt Creation Endpoint

### Endpoint

TBD

### Method

TBD

### Purpose

Create receipt only after verified payment state.

### Expected Inputs

- invoiceId
- paymentId
- verified payment state
- merchant context if applicable

### Expected Safe Output

- receipt created only after verified payment state
- no duplicate conflicting receipt
- no receipt for mismatch / replay / timeout / ambiguous state

### Risk Areas

- receipt before verified payment
- duplicate receipt
- receipt for replayed payment
- receipt for mismatched invoice
- receipt write failure
- secret leakage

### V13 Required Checks

- duplicate receipt creation
- receipt before verified payment rejected
- receipt for mismatch rejected
- receipt for replay rejected
- receipt write failure fails secure

### Current Status

TBD

### V13 Action

TBD

---

## 5. Evidence Creation Endpoint

### Endpoint

TBD

### Method

TBD

### Purpose

Create evidence record only after verified payment state and required receipt state.

### Expected Inputs

- invoiceId
- receiptId
- payment verification context
- public-safe evidence context

### Expected Safe Output

- evidence created only through legal verified path
- no duplicate conflicting evidence
- no evidence for mismatch / replay / timeout / ambiguous state

### Risk Areas

- evidence before verified payment
- duplicate evidence
- evidence without receipt
- evidence write failure
- public unsafe evidence data

### V13 Required Checks

- duplicate evidence creation
- evidence before verified payment rejected
- evidence without receipt handled safely
- evidence write failure fails secure
- no sensitive data in evidence output

### Current Status

TBD

### V13 Action

TBD

---

## 6. Public Verify Endpoint

### Endpoint

TBD

### Method

TBD

### Purpose

Allow public-safe verification of receipt/evidence/trust state.

### Expected Inputs

- receiptId
- evidenceId
- public verify token if applicable

### Expected Safe Output

- verified status
- publicSafe status
- minimumDisclosure status
- safe payment/access/receipt/evidence state labels
- no raw paymentId
- no raw txid
- no secret data

### Risk Areas

- unknown pair returns active verified trust
- mismatched pair returns active verified trust
- missing input leakage
- raw identifier exposure
- stale trust state
- incomplete receipt/evidence state

### V13 Required Checks

- unknown pair safe rejection
- mismatched pair safe rejection
- missing receiptId safe rejection
- missing evidenceId safe rejection
- incomplete receipt/evidence safe state
- safe error scan

### Current Status

TBD

### V13 Action

TBD

---

## 7. Access Unlock Endpoint / Logic

### Endpoint Or Logic Location

TBD

### Method

TBD

### Purpose

Unlock access only after backend-verified payment state and required receipt/evidence state.

### Expected Inputs

- invoiceId
- payment state
- receipt state
- evidence state
- access state

### Expected Safe Output

- access locked for invoice-created state
- access locked for pending state
- access locked for timeout state
- access locked for ambiguous state
- access locked for mismatch state
- access locked for replay rejected state
- access unlocked only through legal backend-verified path

### Risk Areas

- frontend callback unlock
- pending state unlock
- timeout state unlock
- mismatch state unlock
- incomplete receipt/evidence unlock
- duplicate unlock conflict

### V13 Required Checks

- invoice created remains locked
- pending remains locked
- timeout remains locked
- ambiguous remains locked
- mismatch remains locked
- replay rejected remains locked
- incomplete receipt/evidence remains locked
- verified receipt/evidence legal path may unlock

### Current Status

TBD

### V13 Action

TBD

---

## 8. Validation Runner / Diagnostic Page

### Endpoint Or Page

TBD

### Purpose

Run public-safe validation checks without exposing secrets.

### Expected Safe Output

- test count
- pass/fail count
- public-safe result labels
- no raw secrets
- no raw internal errors

### Risk Areas

- test output exposes secret
- test output exposes raw API response
- test output exposes database error
- false pass from incomplete tests
- misleading production-ready language

### V13 Required Checks

- safe error scan
- no secret leakage
- no production-ready claim
- no formal audit claim
- clear limitations

### Current Status

TBD

### V13 Action

TBD

---

## Safe Error Scan Terms

Public outputs should be checked for:

- service role
- secret key
- private key
- passphrase
- environment variable
- stack trace
- raw database error
- raw Pi API response
- sensitive customer data
- access token
- wallet secret

---

## Endpoint Mapping Output Required

Before implementation, produce a filled endpoint map with:

- endpoint path
- HTTP method
- purpose
- required inputs
- optional inputs
- expected safe output
- current known behavior
- unknown behavior
- V13 risk category
- V13 planned action

---

## Public-Safe Boundary

Do not publish:

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

## V13 Current Safe Statement

SafeGate V13 Controlled Hardening Scope is open.

V13 Endpoint Map Template is open.

V13 is not complete.

V13 has not passed evidence validation yet.

SafeGate remains not production-ready and not formally audited.
