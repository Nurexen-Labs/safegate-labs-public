# SafeGate Public Links

Every link below was checked and returned a live response on 19 November 2026.
If one of them breaks, that is a bug and we want to know.

## Start here

| What | Where |
|---|---|
| Main site | https://safegatelabs.xyz/ |
| Human quickstart, sample proof, free check | https://pay.safegatelabs.xyz/try.html |
| Developer portal | https://api.safegatelabs.xyz/ |
| Technical evidence archive | https://safegatelabs.xyz/technical-evidence-archive.html |
| Pilot hub | https://safegatelabs.xyz/pilot-hub.html |
| Contact | https://safegatelabs.xyz/contact.html |

## Verify a proof

| What | Where |
|---|---|
| Free verification (same engine, shared rate limit) | `POST https://verify.safegatelabs.xyz/api/commerceproof/verify-free` |
| Free service health, with honest disclosure | https://verify.safegatelabs.xyz/api/health |
| Paid verification, 0.05 USDC per settled call | `POST https://pay.safegatelabs.xyz/api/commerceproof/verify-paid` |
| Paid service health | https://pay.safegatelabs.xyz/api/health |

Call the paid endpoint with no payment header to receive the full 402 challenge and its terms.

## Machine-readable

| What | Where |
|---|---|
| Service manifest | https://pay.safegatelabs.xyz/.well-known/x402.json |
| Service manifest, discovery form | https://pay.safegatelabs.xyz/.well-known/x402 |
| OpenAPI 3.1, JSON | https://pay.safegatelabs.xyz/openapi.json |
| OpenAPI 3.1, YAML | https://pay.safegatelabs.xyz/commerceproof-verify-paid-openapi.yaml |
| Signer key registry, Ed25519 | https://pay.safegatelabs.xyz/.well-known/safegate-keys.json |

Download the key registry independently of any proof and pin `publicKeySha256`. Never trust the
key carried inside a proof.

## Sample data

| What | Where |
|---|---|
| Sample verify request, returns VERIFIED | https://pay.safegatelabs.xyz/sample-verify-request.json |
| Sample CommerceProof | https://pay.safegatelabs.xyz/sample-commerceproof.json |

## On-chain

| What | Where |
|---|---|
| First charged verification, 18 Nov 2026 | https://basescan.org/tx/0x7d100bfb0185db0ac7d4f6983048b96fb9b4cd3d9616a6be2e8be4c5610d4b7c |

## Partner reference

| What | Where |
|---|---|
| NSGoods published manifest | https://x402.nsgoods.org/proof/index.json |
| Check before you pay, prove after you pay | https://x402.nsgoods.org/check-before-you-pay |
