# Security Boundaries

SafeGate is security-conscious and has **not** had an external audit. That is stated here first
so it cannot be missed.

## Controls now in place on the paid route

1. Expected payee validation. The payee in the presented requirements must equal ours.
2. Exact amount validation. Over-payment and under-payment are both refused.
3. Scheme, network and asset locks. Anything other than the advertised combination is refused.
4. Resource binding. The resource named in the payment must be the resource being called.
5. Replay protection by authorization nonce. Re-presenting the same authorization returns the
   stored result and never charges twice.
6. Settle-only-on-VERIFIED. A failed or indeterminate verdict never reaches the settle call.
7. Durable record written before settlement is attempted. If the record cannot be advanced,
   settlement is deliberately skipped and the caller is told.
8. Fail-closed on quota. If the durable quota is unavailable, the route refuses rather than
   guessing.
9. Request size limit checked before any payment handling.
10. Signer key pinned by hash to a published registry. The key inside a proof is never trusted.
11. Request-ID identity checked across every section of a proof, including inside the third
    party's own signed bytes.

## Still required

1. External security audit.
2. Durable cross-instance quota for paid customers. The pilot does not have one, and
   `/api/health` reports `durable_quota_available: false` rather than implying otherwise.
3. Broader rate limiting beyond per-instance best effort.
4. IDOR and record-access review on merchant surfaces.
5. Database policy review.

## Fail-secure principle

If verification is ambiguous, pending, failed, cancelled, timed out or invalid: do not settle, do
not report a verdict stronger than the evidence, and do not create a record implying an outcome
that was not observed.

An unavailable sanctions list is never silently treated as clear. It is reported explicitly as
unavailable or stale.

## Honest disclosure as a control

`/api/health` is unauthenticated and free, and discloses which verifier modules are loaded and
what has not been tested. If it reports false, it is false. We would rather publish a weakness
than have someone discover we hid one.

## Private data rules

Never publish `.env` files, API keys, service-role keys, receipt secrets, access tokens, raw
debug fields, wallet passphrases, private admin or debug material, or personal data of testers,
customers or partners.

Commercial terms agreed with partners are confidential. Published prices are not.
