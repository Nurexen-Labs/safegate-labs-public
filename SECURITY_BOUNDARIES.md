# Security Boundaries

SafeGate is security-conscious but does not claim full external audit.

## Current Evidenced Controls

1. Payment finalized before access unlock
2. Receipt proof created after finalized payment
3. Merchant record created after successful flow

## Hardening Still Required

1. Expected recipient / merchant wallet validation
2. Expected amount validation
3. Order / product / payment binding
4. Fail-secure behavior on API timeout, pending, failed, canceled, ambiguous, or 502 responses
5. Merchant/API-key rate limiting
6. IDOR / record access protection
7. Supabase RLS / database policy review
8. External audit

## Private Data Rules

Never publish .env files, API keys, service role keys, receipt secrets, Pi accessToken, UID/raw debug fields, wallet passphrases, or private admin/debug screenshots.

## Fail-Secure Principle

If payment verification is ambiguous, pending, failed, canceled, timed out, or invalid: do not unlock access, do not generate receipt proof, and do not create merchant record.
