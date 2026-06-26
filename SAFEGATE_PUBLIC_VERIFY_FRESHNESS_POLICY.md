# SafeGate Public Verify Freshness Policy

SafeGate verifies what happened after payment.

SafeGate does not process payments.

Payment is the trigger. Trust is the product.

---

## Document Status

**Status:** Public verify freshness planning policy  
**Scope:** V9.1 backend + integrity hardening  
**Current Public Phase:** Pilot Readiness  
**Controlled Pi Testnet Payment Spine:** PASSED  
**Production Claim:** No  
**Security Certification Claim:** No  
**Third-Party Audit Claim:** No  
**Independent Cryptographic Verification Claim:** Not yet claimed  

---

## Important Boundary

This document is a policy and planning note.

It does not claim production readiness.

It does not claim formal audit completion.

It does not claim tamper-proof verification.

It does not claim that all freshness, expiry, revocation, or dispute states are already implemented.

Its purpose is to define how SafeGate public verification should avoid stale or misleading trust signals.

---

## Why This Policy Exists

Technical reviewers asked an important question:

If public verify shows `verified: true`, how fresh is that result?

A public verify result must not become misleading if the underlying trust state later changes.

Examples:

- receipt expires
- receipt is revoked
- dispute is opened
- access entitlement expires
- backend state changes
- public verify response is cached
- public verify reads stale data
- database is unavailable

This policy defines SafeGate’s intended public verify freshness model.

---

## Current Foundation

SafeGate has demonstrated a controlled Pi Testnet payment-spine execution in Pi Browser.

The controlled flow showed:

- V9 invoice creation
- initial access state locked
- Pi Browser authentication
- username + payments scope
- Pi wallet payment in Sandbox / Testnet context
- backend approval / completion flow
- backend-verified payment state
- receipt and evidence generation after verified state
- public verify confirmation
- access unlock only after backend-verified receipt

Public-safe confirmed result:

- paymentState: PAYMENT_VERIFIED_TESTNET
- accessState: ACCESS_UNLOCKED_BY_BACKEND_VERIFIED_RECEIPT
- verified: true
- minimumDisclosure: true
- publicSafe: true
- paymentIdIncluded: false
- txidIncluded: false
- secretsIncluded: false
- customerDataIncluded: false
- accessUnlocked: true

---

## Current Gap

Minimum-disclosure public verify is privacy-positive.

However, minimum-disclosure does not automatically answer freshness questions.

Reviewers may ask:

- Is public verify reading live durable state?
- Is public verify returning cached data?
- Can a verified receipt later become expired, revoked, or disputed?
- If access expires, does public verify still show active unlock?
- If backend cannot read the database, does public verify fail safe?
- If a receipt is superseded, how is that shown publicly?
- Can public verify show stale truth without context?

This policy defines the intended answer.

---

## Public Verify Goal

Public verify should provide a minimum-disclosure trust result that is:

- current enough for its stated purpose
- not misleading
- privacy-preserving
- public-safe
- fail-secure
- lifecycle-aware
- clear about active, expired, revoked, disputed, or ambiguous status

---

## Public Verify Non-Goals

Public verify should not expose:

- raw paymentId
- raw txid
- raw Pi API response
- access token
- service role key
- customer data
- private wallet data
- internal logs
- stack traces
- private deployment details

Public verify should not claim:

- production-grade finality
- Pi Mainnet settlement
- formal audit verification
- independent cryptographic proof unless implemented
- tamper-proof status unless implemented

---

## Freshness Definitions

SafeGate should distinguish between several freshness concepts.

| Concept | Meaning |
|---|---|
| createdAt | When the original invoice/trust record was created |
| issuedAt | When receipt/evidence was issued |
| verifiedAt | When backend-verified payment state was recorded |
| lastCheckedAt | When public verify last read or confirmed the record |
| updatedAt | When the trust record was last updated |
| expiresAt | When receipt or access validity expires, if used |
| revokedAt | When receipt was revoked, if used |
| disputedAt | When dispute state was added, if used |

---

## Data Source Policy

Public verify should clearly define its data source.

Preferred model:

- public verify reads from durable backend state
- public verify does not rely only on static page text
- public verify does not expose private backend data
- public verify returns minimum-disclosure fields only
- public verify fails safe if durable state cannot be read

Boundary:

If any public verify response is cached in the future, cache behavior must be documented.

---

## Live vs Cached vs Static

SafeGate should avoid ambiguity.

### Live DB-Backed Public Verify

Public verify reads current durable state and returns a current minimum-disclosure result.

This is the preferred model for trust-state verification.

### Cached Public Verify

Public verify may cache safe fields for performance, but should expose cache/freshness context if used.

Required fields may include:

- lastCheckedAt
- cacheStatus
- freshnessStatus

### Static Public Verify

A static public result is not enough for lifecycle-aware trust verification.

Static evidence may be useful for demonstration, but should not be presented as current active trust state unless clearly labeled.

---

## Recommended Public Verify Status Fields

Public verify should eventually include public-safe status fields such as:

- verified
- receiptStatus
- evidenceStatus
- accessStatus
- freshnessStatus
- minimumDisclosure
- publicSafe
- issuedAt
- verifiedAt
- lastCheckedAt
- expiresAt if implemented
- revokedAt if implemented and public-safe
- disputedAt if implemented and public-safe
- integrityStatus if implemented
- accessUnlocked

---

## Suggested Freshness Status Values

Suggested public-safe freshness values:

| Freshness Status | Meaning |
|---|---|
| FRESH | Public verify read current durable state recently |
| CACHED_VALID | Result is cached but still inside defined freshness window |
| STALE | Result may be outdated |
| UNKNOWN | Freshness cannot be determined |
| UNAVAILABLE | Durable state cannot be read |
| REVIEW_REQUIRED | State is ambiguous and requires manual review |

---

## Suggested Receipt Status Values

Suggested public-safe receipt lifecycle values:

| Receipt Status | Meaning |
|---|---|
| RECEIPT_ACTIVE | Receipt is currently active |
| RECEIPT_EXPIRED | Receipt has expired |
| RECEIPT_REVOKED | Receipt was revoked |
| RECEIPT_DISPUTED | Receipt is under dispute |
| RECEIPT_SUPERSEDED | Receipt was replaced by a newer record |
| RECEIPT_INTEGRITY_FAILED | Receipt integrity check failed |
| RECEIPT_REVIEW_REQUIRED | Receipt requires manual review |
| RECEIPT_NOT_FOUND | Receipt/evidence pair not found |
| RECEIPT_MISMATCH | Receipt/evidence pair mismatch |

---

## Suggested Access Status Values

Public verify should distinguish payment verification from access status.

Suggested access states:

| Access Status | Meaning |
|---|---|
| ACCESS_LOCKED | Access is not unlocked |
| ACCESS_UNLOCKED_ACTIVE | Access is currently unlocked |
| ACCESS_EXPIRED | Access was unlocked but later expired |
| ACCESS_REVOKED | Access was revoked |
| ACCESS_DISPUTED | Access is blocked or flagged due to dispute |
| ACCESS_REVIEW_REQUIRED | Access status requires manual review |
| ACCESS_UNKNOWN | Access status cannot be determined safely |

---

## Verified Does Not Always Mean Active Forever

A payment may have been verified at one time.

That does not always mean the related access remains active forever.

SafeGate should distinguish:

- payment verification
- receipt validity
- access entitlement
- evidence integrity
- dispute/revocation state

A public verify result should not collapse all of these into one vague `verified: true` forever.

---

## Public Verify Result Policy

When the record is valid and active:

- verified: true
- receiptStatus: RECEIPT_ACTIVE
- accessStatus: ACCESS_UNLOCKED_ACTIVE
- freshnessStatus: FRESH or CACHED_VALID
- minimumDisclosure: true
- publicSafe: true

When the record is expired:

- verified may remain historically true if payment was verified
- receiptStatus should show RECEIPT_EXPIRED
- accessStatus should show ACCESS_EXPIRED
- accessUnlocked should be false or clearly inactive
- publicSafe remains true

When the record is revoked:

- verified should not imply active trust
- receiptStatus should show RECEIPT_REVOKED
- accessStatus should show ACCESS_REVOKED
- accessUnlocked should be false
- publicSafe remains true

When the record is disputed:

- receiptStatus should show RECEIPT_DISPUTED
- accessStatus may show ACCESS_DISPUTED or ACCESS_REVIEW_REQUIRED
- public verify should avoid exposing private dispute details
- publicSafe remains true

When durable state is unavailable:

- public verify must not return fake verified true
- freshnessStatus should show UNAVAILABLE or UNKNOWN
- accessUnlocked should not be newly asserted
- safe error should be returned

---

## Public Verify Fail-Secure Rule

If SafeGate cannot determine current trust state safely, it should not assert active trust.

Fail-secure behavior means:

- no fake verified active state
- no new receipt/evidence
- no access unlock
- no sensitive error output
- safe public error or review state

---

## Unknown or Mismatched Records

For unknown receipt/evidence pairs:

Expected behavior:

- verified: false
- receiptStatus: RECEIPT_NOT_FOUND
- publicSafe: true
- no sensitive details

For mismatched receipt/evidence pairs:

Expected behavior:

- verified: false
- receiptStatus: RECEIPT_MISMATCH
- publicSafe: true
- no sensitive details
- no access unlock

---

## Expiry Policy

SafeGate should define whether receipts or access entitlements expire.

Possible models:

### Permanent Receipt, Time-Limited Access

The receipt remains as historical proof, but access expires.

### Time-Limited Receipt and Access

Both receipt and access expire after a defined period.

### Permanent Receipt With Revocation/Dispute State

The receipt remains visible but can show revoked or disputed status.

Boundary:

Do not claim expiry behavior until implemented and tested.

---

## Revocation Policy

SafeGate should define whether a receipt can be revoked.

Possible revocation reasons:

- payment later invalidated
- dispute accepted
- evidence integrity failure
- merchant/support correction
- abuse detection
- manual review decision

Public verify should show revocation status without exposing private details.

---

## Dispute Policy

SafeGate should define dispute states carefully.

A dispute may not mean the payment never happened.

It may mean the post-payment outcome is under review.

Suggested public-safe dispute behavior:

- receiptStatus: RECEIPT_DISPUTED
- accessStatus: ACCESS_REVIEW_REQUIRED or ACCESS_DISPUTED
- verified remains contextual, not active entitlement
- no private dispute details exposed

---

## Superseded Record Policy

Sometimes a record may be replaced by a corrected record.

If this happens, public verify should avoid confusion.

Possible public-safe status:

- RECEIPT_SUPERSEDED
- replacementAvailable: true or false
- no sensitive replacement identifiers unless public-safe

Boundary:

Do not implement replacement links if they expose private data.

---

## Cache Policy

If caching is used, SafeGate should define:

- cache duration
- fields eligible for caching
- fields not eligible for caching
- stale cache behavior
- cache invalidation for revoked/disputed/expired states
- public-safe cache indicators

Recommended direction:

Trust-state public verify should prefer live durable reads or very short safe cache windows.

---

## Public Verify Freshness Fields

Possible future fields:

| Field | Public-Safe Purpose |
|---|---|
| freshnessStatus | Explains whether result is fresh, cached, stale, unknown, or unavailable |
| lastCheckedAt | Shows when state was last checked |
| verifiedAt | Shows when backend verification occurred |
| issuedAt | Shows when receipt was issued |
| updatedAt | Shows last trust-state update |
| expiresAt | Shows expiry if implemented |
| receiptStatus | Shows receipt lifecycle |
| accessStatus | Shows access lifecycle |
| integrityStatus | Shows integrity check if implemented |

---

## What Public Verify Should Continue to Exclude

Public verify should continue excluding:

- paymentId
- txid
- raw Pi API response
- service role key
- access token
- user UID
- wallet address
- recipient address
- private logs
- stack trace
- customer data
- private merchant data
- internal database row details

---

## Safe Error Codes

Possible public-safe error codes:

- RECEIPT_EVIDENCE_NOT_FOUND
- RECEIPT_EVIDENCE_MISMATCH
- PUBLIC_VERIFY_UNAVAILABLE
- DURABLE_STATE_UNAVAILABLE
- FRESHNESS_UNKNOWN
- RECEIPT_EXPIRED
- RECEIPT_REVOKED
- RECEIPT_DISPUTED
- RECEIPT_INTEGRITY_FAILED
- ACCESS_EXPIRED
- ACCESS_REVOKED
- ACCESS_REVIEW_REQUIRED
- INTERNAL_SAFE_FAILURE

---

## Relationship to Receipt Integrity

Freshness and integrity are related but separate.

Freshness asks:

Is this result current?

Integrity asks:

Has this result been altered?

A future public verify model should ideally include both:

- freshnessStatus
- integrityStatus

Minimum-disclosure should still be preserved.

---

## Relationship to Backend Behavior Matrix

The Backend Behavior Evidence Matrix defines expected endpoint behavior.

This policy defines how public verify should represent current trust state without misleading reviewers or users.

Together, they answer:

- what happened
- what is current
- what is safe to show publicly
- what must remain private
- when verification must fail secure

---

## Relationship to AI Agent Readiness

AI agents should not consume vague or stale trust signals.

Before AI Agent Readiness, public verify should expose machine-readable but minimum-disclosure fields such as:

- receiptStatus
- accessStatus
- freshnessStatus
- integrityStatus
- verifiedAt
- issuedAt
- expiresAt if implemented

AI agents should not treat `verified: true` as active entitlement without checking freshness and status fields.

---

## V9.1 Freshness Test Targets

Recommended tests:

1. valid active public verify
2. unknown receipt/evidence pair
3. mismatched receipt/evidence pair
4. durable state read failure
5. expired receipt/access if expiry is implemented
6. revoked receipt if revocation is implemented
7. disputed receipt if dispute state is implemented
8. cached result behavior if caching is used
9. stale result behavior if freshness window expires
10. safe error output without private data

---

## Current Safe Language

SafeGate may say:

- minimum-disclosure public verify
- public verify freshness policy
- live/durable-state verification direction
- stale-data risk acknowledged
- expiry/revocation/dispute policy planned
- no production claim
- no audit claim

SafeGate should not say yet:

- public verify is always current under all conditions
- public verify is tamper-proof
- public verify is independently verified
- public verify is production-grade
- public verify handles all disputes or revocations
- public verify has completed formal security validation

---

## Completion Criteria

This policy can be considered ready for V9.1 planning when:

- data source model is defined
- freshness status values are defined
- receipt lifecycle values are defined
- access status values are defined
- expiry/revocation/dispute behavior is defined
- fail-secure behavior is defined
- cache policy is defined if caching is used
- public-safe fields are defined
- unsafe fields remain excluded
- safe language boundaries are preserved

Implementation completion requires separate testing and evidence.

---

## Final Freshness Rule

A SafeGate public verify result should not merely say that something was verified once.

It should eventually show, in a minimum-disclosure way, whether the verified trust record is currently active, expired, revoked, disputed, stale, unavailable, or review-required.

That is the SafeGate public verify freshness target.

---

## Final Positioning

SafeGate does not process payments.

SafeGate verifies what happened after payment.

Payment is the trigger. Trust is the product.
