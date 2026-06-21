# SafeGate Labs

Payment is the trigger. Trust is the product.

SafeGate Labs is a Pi-first, chain-agnostic payment-triggered trust layer for digital commerce.

SafeGate focuses on what happens after payment:

payment succeeded -> PAYMENT_FINALIZED -> receipt proof -> access unlocked -> merchant record

## Current Status

SafeGate completed a controlled Pi Testnet pilot with 5 testers.

Across 5 controlled testers, the SafeGate flow produced:
- 5 successful 0.01 TEST-PI payments
- 5 PAYMENT_FINALIZED states
- 5 access unlocks
- 5 receipt proofs
- 5 merchant records
- positive usability feedback from all testers

## Live Links

Main app: https://safegatelabs.xyz
Start Here: https://safegatelabs.xyz/start-here-external-review-hub.html
5-Tester Evidence Dashboard: https://safegatelabs.xyz/pilot-evidence-summary-dashboard.html
Pilot Evidence Freeze Index: https://safegatelabs.xyz/pilot-evidence-freeze-index.html
Security Check: https://safegatelabs.xyz/security-hardening-implementation-check.html
Invoice Lite: https://safegatelabs.xyz/invoice-lite.html
Invoice Security Checklist: https://safegatelabs.xyz/invoice-security-binding-checklist.html
Invoice Lite Freeze Index: https://safegatelabs.xyz/invoice-lite-freeze-index.html
---

## V9 Controlled Testnet Payment Spine

SafeGate V9 adds controlled Pi Sandbox / Testnet-oriented payment spine evidence.

Observed flow:

Pi authenticate → payments scope → durable V9 invoice → Pi.createPayment → 0.01 Test-Pi payment succeeded → PAYMENT_VERIFIED_TESTNET → guarded receipt/evidence → public verify URL.

Public evidence:

- V9 Final Freeze Note: https://safegate-v185-pi-debug-ready.vercel.app/v9-final-freeze-note.html
- V9 Pi Payment Spine Evidence: https://safegate-v185-pi-debug-ready.vercel.app/v9-pi-payment-spine-evidence.html

Boundary:

This is controlled pilot / Testnet-oriented evidence. It does not claim Mainnet payment live, production readiness, official Pi partnership, custody, or a fully audited security product.
## Boundary

This repository is a public documentation and architecture repository.

It does not include backend payment verification code, private admin logic, API keys, Supabase service role keys, receipt secrets, Pi access tokens, wallet passphrases, private debug screenshots, or production API credentials.
