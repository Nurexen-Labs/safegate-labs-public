# SafeGate Labs

Payment proves money moved. SafeGate proves what happened next, and says how strong that proof is.

SafeGate is a rail-agnostic commerce verification and evidence layer for AI agents, paid APIs,
digital services and platforms. It binds a payment to the exact request it paid for, prevents
replay, observes or validates the outcome, and produces a signed, portable CommerceProof.

This repository is public documentation. It contains no backend code, no credentials and no
private infrastructure detail.

## The assurance boundary

This is the part most verification projects leave vague, so it comes first.

SafeGate verifies **evidence**. It does not witness delivery.

- `commerce_verified` is never reported as true.
- A clean sanctions screening is negative evidence only. It can push a decision toward deny,
  never toward allow.
- Every proof names its own assurance level: CLAIMED, OBSERVED, VALIDATED or THIRD-PARTY
  ATTESTED. There is no silent upgrade from one level to the next.
- SafeGate is not a wallet, bank, exchange or payment processor. It holds no custody, no
  escrow and no private keys.

## What is live today

A paid verification endpoint, callable by any x402 client with no account, no API key and no
signup.

| | |
|---|---|
| Endpoint | `POST https://pay.safegatelabs.xyz/api/commerceproof/verify-paid` |
| Protocol | x402 version 2, `exact` scheme, EIP-3009 `transferWithAuthorization` |
| Network | Base Mainnet, `eip155:8453` |
| Asset | USDC, `0x833589fcd6edb6e08f4c7c32d4f71b54bda02913` |
| Published price | 0.05 USDC per settled call |
| Charging policy | `NO_CHARGE_UNLESS_VERDICT_IS_VERIFIED` |

Call it with no payment header and it returns the 402 challenge with its full terms. A `FAILED`
or `INDETERMINATE` verdict never reaches settlement and is never charged. Re-presenting the same
payment authorization returns the stored result without a second charge.

The same verification engine runs free, subject to a shared rate limit, so you can check a proof
before you pay for one. See LINKS.md.

## What a VERIFIED verdict requires

All three, independently:

1. The SafeGate signer key matches the registry published at
   `/.well-known/safegate-keys.json`, pinned by hash. The key carried inside a proof is never
   trusted.
2. A valid NSGoods signed pre-payment screening of the payment receiver, checked against the
   manifest NSGoods itself publishes.
3. Both on-chain legs confirmed by two independent RPC providers that agree.

The layers genuinely independent of SafeGate are the NSGoods manifest and the Base chain itself.
The key registry makes the signature independent of the proof, not independent of SafeGate. We
state this rather than imply otherwise.

## Evidence

See PILOT_EVIDENCE.md for the dated record, including the first charged verification settled
on Base and the on-chain transaction that anyone can inspect.

## Boundary

This repository does not include backend verification code, private admin logic, API keys,
service-role keys, receipt secrets, access tokens, wallet passphrases, private debug material or
production credentials. See NOTICE.md and SECURITY_BOUNDARIES.md.

SafeGate Labs / Nurexen Labs.
