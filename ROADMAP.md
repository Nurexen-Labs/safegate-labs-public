# Roadmap

## Done

- Rail-agnostic verification core with explicit assurance levels.
- Signed, portable CommerceProof with field-level provenance.
- Free verification endpoint, publicly callable.
- Paid verification endpoint over x402 version 2 on Base Mainnet, settle-only-on-VERIFIED.
- First charged verification settled on-chain, 18 November 2026. See PILOT_EVIDENCE.md.
- Machine-readable service manifest, OpenAPI 3.1 specification and published signer key registry.
- Listed in the x402scan catalogue for machine discovery.
- Pre-payment screening partnership with NSGoods, including request-bound screening.
- Base Mainnet merchant pilot with merchant-reported delivery records.

## Next

- Second settlement route through an additional facilitator, so the resource is discoverable in
  more than one catalogue.
- Request-bound screening carried end to end into a published CommerceProof.
- Richer sanctions list health reporting inside proofs, including staleness and last-checked
  times rather than version dates alone.
- First third-party paid verification. Today there is none, and PILOT_EVIDENCE.md says so.

## Later

- Independent security review.
- Durable cross-instance quota for paid customers. The current pilot has none and the health
  endpoint reports that honestly rather than implying otherwise.
- Merchant onboarding and self-serve integration.
- Working EN/TR language support on the public site. The present switcher is decorative and is
  tracked as a known gap.

## Not planned

- Becoming a payment rail, wallet, custodian or escrow.
- Claiming independently validated delivery. SafeGate verifies evidence, not fulfillment.
