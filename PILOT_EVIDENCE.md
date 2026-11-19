# Evidence Log

Dated record of what SafeGate has actually completed. Every entry here either carries a
transaction anyone can inspect, or says plainly that it does not.

Nothing in this file is a projection.

---

## 2026-11-18 · First charged CommerceProof verification

The paid verification endpoint completed its first charged call and settled on Base.

| | |
|---|---|
| Date | 18 November 2026 |
| Network | Base Mainnet, `eip155:8453` |
| Asset | USDC |
| Protocol | x402 version 2, `exact` scheme, EIP-3009 |
| Gross settled | 0.05 USDC |
| On-chain settlement | SUCCESS |
| Payment status | CHARGED |
| Verification verdict | VERIFIED |

Transaction:
`0x7d100bfb0185db0ac7d4f6983048b96fb9b4cd3d9616a6be2e8be4c5610d4b7c`

https://basescan.org/tx/0x7d100bfb0185db0ac7d4f6983048b96fb9b4cd3d9616a6be2e8be4c5610d4b7c

The proof that was verified is published, so the same check can be repeated for free by anyone.
See LINKS.md.

**Boundary.** This was the initial settlement run with our payment facilitator during
integration, not a third-party purchase. It confirms payment settlement and the reported verdict.
It does not establish product delivery or fulfillment.

---

## 2026-11 · Listed for machine discovery

The paid resource is registered in the x402scan catalogue and publishes a machine-readable
service manifest, an OpenAPI specification and a working sample request. An agent can read the
terms, run the free check and then pay, with no human step in between.

**Boundary.** Being discoverable is not traction. It means a machine can find and call the
service, nothing more.

---

## 2026-11 · Base Mainnet merchant pilot

A fee-paying restaurant pilot records signed, merchant-reported deliveries and verifies 0.07 USDC
service-fee payments on Base Mainnet.

**Boundary.** Delivery status is merchant-reported and classified CLAIMED. Physical delivery is
not independently validated, and SafeGate does not claim otherwise.

---

## 2026-11 · Routed execution evidence

A real Base to Solana routed USDC fulfillment was completed and independently evidenced, with
first integrator revenue recorded from the routed flow.

**Boundary.** The public presentation of this surface is being agreed with the partner before
production copy changes. The evidence exists; the joint wording does not yet.

---

## 2026-11 · Pre-payment screening partnership

NSGoods performs signed pre-payment sanctions screening of the payment receiver. SafeGate
verifies that signed verdict against the manifest NSGoods publishes. The two surfaces stay
independent: NSGoods never touches the merchant payment, and SafeGate never rewrites the NSGoods
verdict.

A screening can be bound to one specific request, so a clean verdict from one transaction cannot
be reused for another.

**Boundary.** Screening is negative evidence. A clean result means no known listing matched at
that moment. It is not clearance, and it is not legal advice.

---

## 2026-06 · Pi Testnet pilot (archived)

A controlled Pi Testnet pilot was completed with five testers: five successful 0.01 TEST-PI
payments, five finalized payment states, five access unlocks, five receipt proofs and five
merchant records.

Tester identities are deliberately not published. The record is held privately.

**Boundary.** This was controlled testnet evidence. It is not mainnet settlement proof, not
unrestricted production use and not an external audit. Pi work is retained as historical adapter
evidence and is **not** the current product identity.

---

## What is not claimed anywhere in this log

- No independently validated physical or digital delivery.
- No external security audit.
- No third-party customer purchase of the paid endpoint to date.
- No mature public reputation scoring.
