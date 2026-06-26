# SafeGate — Pilot Evidence Template

## Purpose

This document defines a public-safe evidence format for SafeGate pilot testing.

The goal is to collect clear, reviewable, privacy-aware pilot evidence without exposing private user data, private keys, wallet secrets, service-role secrets, or sensitive merchant/customer information.

SafeGate does not process payments.  
SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.

---

## Pilot Evidence Rules

Pilot evidence must be:

- public-safe
- screenshot-friendly
- easy to review
- easy to compare across testers
- free of secrets
- free of raw personally identifiable data
- clear about device/browser context
- clear about test result
- clear about limitations

Do not publish:

- private keys
- seed phrases
- service-role secrets
- raw personally identifiable data
- private wallet data
- private customer records
- sensitive merchant/customer financial data

---

## Evidence Entry Template

Use the following format for each pilot evidence entry.

### Pilot Evidence Entry

Tester Type:

Device / Browser:

General Location:

Date:

SafeGate Page Tested:

Test Scenario:

Expected Result:

Observed Result:

Pass / Fail:

Screenshot or Recording:

Notes:

Boundary Confirmed:

---

## Example Entry

Tester Type: External tester / reviewer-style user

Device / Browser: Android / Pi Browser or Chrome

General Location: Türkiye

Date: YYYY-MM-DD

SafeGate Page Tested: Live Review Hub / Trust State Page / Public Verification Page

Test Scenario: Tester opens SafeGate review flow and checks whether the post-payment trust state is understandable.

Expected Result: Tester should understand that SafeGate does not process payments and only verifies what happened after payment.

Observed Result: Tester understood the trust-state flow and confirmed that payment processing claims were not made.

Pass / Fail: Pass

Screenshot or Recording: Public-safe screenshot attached

Notes: Tester found the evidence chain understandable.

Boundary Confirmed: No production readiness claim, no Pi Mainnet settlement claim, no custodial payment claim.

---

## Suggested Pilot Evidence Categories

Pilot evidence can be grouped into:

- reviewer clarity
- merchant clarity
- user clarity
- payment-state readability
- receipt/evidence readability
- access-state clarity
- public verification clarity
- failure-state clarity
- refund/dispute-state clarity
- privacy-boundary clarity
- partner/API direction clarity

---

## Minimum Pilot Evidence Target

Initial controlled pilot target:

- 5 to 10 pilot merchant/test scenarios
- 10 to 30 external tester/reviewer-style sessions
- public-safe screenshots or short screen recordings
- short written feedback
- no sensitive data exposure

---

## Recommended Evidence Table

| ID | Tester Type | Page Tested | Scenario | Result | Evidence Type | Notes |
|---|---|---|---|---|---|---|
| SG-PILOT-001 | External tester | Review Hub | Open and understand SafeGate positioning | Pending | Screenshot | Public-safe |
| SG-PILOT-002 | Merchant-style tester | Trust State Page | Review post-payment state object | Pending | Screenshot | Public-safe |
| SG-PILOT-003 | Reviewer-style tester | V12 Freeze Note | Check boundaries and claims | Pending | Screenshot | Public-safe |
| SG-PILOT-004 | Technical reviewer | V11 Security Index | Check hardening evidence | Pending | Screenshot | Public-safe |
| SG-PILOT-005 | Pilot merchant | Public verification flow | Understand receipt/evidence meaning | Pending | Screenshot | Public-safe |

---

## Pilot Feedback Questions

Ask each pilot tester:

1. What did you understand SafeGate does?
2. What did you understand SafeGate does not claim?
3. Was the post-payment trust flow clear?
4. Was the evidence chain easy to follow?
5. Was the access-state logic understandable?
6. Was the public verification idea useful?
7. Was anything confusing?
8. What should be improved before a wider pilot?
9. Would this be useful for merchants or digital services?
10. Would you be comfortable reviewing this as a controlled MVP evidence layer?

---

## Pilot Success Criteria

Pilot evidence is considered useful if:

- the tester understands SafeGate’s core positioning
- the tester understands the project boundary
- the tester can follow the evidence chain
- the tester can explain the post-payment trust value
- the tester does not confuse SafeGate with a payment processor
- the tester does not assume Pi Mainnet settlement
- the tester can identify where receipt/evidence/access state appears
- the tester can provide clear feedback

---

## Final Boundary

Pilot evidence collection does not mean production launch.

Pilot evidence collection does not mean regulatory approval.

Pilot evidence collection does not mean official Pi partnership.

Pilot evidence collection does not mean Pi Mainnet settlement.

Pilot evidence collection is for controlled review, clarity, feedback, and pilot readiness.

---

## Final Positioning

SafeGate does not process payments.  
SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.

Next public phase: Pilot Readiness.
