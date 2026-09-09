# SafeGate — Historical Public Review Archive

> **Historical repository**
>
> This repository preserves an earlier phase of SafeGate development, including Pi/Testnet-oriented payment verification, V9–V13 trust architecture, hardening plans, public review materials, and early agent-readable trust work.
>
> It is **not the current implementation source of truth** for SafeGate.

## Current SafeGate

SafeGate has evolved from a Pi-first post-payment trust architecture into a broader, agent-first, payment-rail-agnostic and chain-agnostic commerce assurance layer.

Current positioning:

**Payment proves value moved. SafeGate proves what happened next — and makes the strength of that proof explicit.**

SafeGate is designed for:

- autonomous agent commerce
- paid APIs
- MCP tools
- digital services
- merchant and platform verification
- post-payment evidence
- portable commerce proof

Current active technical repository:

https://github.com/Nurexen-Labs/safegate-xagent-commerce-outcome

Current public JavaScript SDK:

    npm install @nurexenlabs/safegate-sdk

Current SDK release:

https://github.com/Nurexen-Labs/safegate-xagent-commerce-outcome/releases/tag/sdk-v0.3.0

Website:

https://safegatelabs.xyz

---

## Why This Repository Is Preserved

SafeGate was not built in one step.

This repository documents an earlier development period in which the project explored and hardened core ideas that remain relevant today:

- post-payment verification
- receipt and evidence creation
- fail-secure behavior
- idempotency
- replay resistance
- payment/request mismatch handling
- controlled access state
- public verification
- fee-finalization boundaries
- merchant trust records
- agent-readable trust states
- privacy-aware evidence boundaries

These materials are preserved because they show the architectural evolution of SafeGate.

They should be read as **historical engineering records**, not as the current API, SDK, MCP, deployment, or product specification.

---

## Historical Development Snapshot

The material in this repository reflects the earlier SafeGate architecture through the V9–V13 period.

Historical milestones documented here include:

- V9 Payment Spine
- V9.1 Backend Behavior Validation
- V11 hardening backlog
- V11 hardening test planning
- V11 implementation sprint scope
- V13 Controlled Hardening Scope
- V13 Public Surface Validation
- V13 Backend Policy Simulation
- V13 validation runner specification
- pilot evidence and review materials
- fee architecture work
- early agent-readable trust direction
- privacy-aware trust direction

The repository's last historical `main` snapshot before this README cleanup dates from June 2026.

---

## Historical SafeGate Question

The original architecture centered on a simple question:

**Did the promised post-payment outcome actually happen?**

That question remains part of SafeGate today.

What changed is the scope.

Earlier work concentrated heavily on Pi/Testnet payment flows, merchant trust, receipts, evidence, and controlled fulfillment.

The current SafeGate architecture generalizes that idea across payment rails, chains, APIs, agents, MCP tools, and digital services.

---

## Historical Core Flow

Earlier SafeGate work explored a flow broadly shaped like:

    payment intent
          |
          v
    backend payment verification
          |
          v
    receipt / evidence creation
          |
          v
    access or fulfillment state
          |
          v
    public verification
          |
          v
    merchant / trust record

This work contributed to the later SafeGate architecture.

The current product has evolved toward:

    payment evidence
          |
          v
    request binding
          |
          v
    durable single-consume / replay safety
          |
          v
    actual outcome observation or attestation
          |
          v
    evidence
          |
          v
    assurance level
          |
          v
    portable CommerceProof

For the current implementation, use the active technical repository rather than this archive.

---

## Historical Pi / Testnet Phase

SafeGate's earlier architecture was strongly influenced by Pi Network and Pi Testnet commerce.

Historical work included:

- Pi-oriented payment flow design
- backend verification boundaries
- payment-state gating
- receipt and evidence concepts
- access locked until verified state
- public-safe verification
- controlled pilot planning

Those records are preserved here.

However:

**This repository's historical “Pi-first” language must not be interpreted as SafeGate's current product positioning.**

Current SafeGate is payment-rail agnostic and chain agnostic.

Pi remains part of SafeGate's broader development history and potential adapter landscape, but it is not the exclusive product identity represented by the current implementation.

---

## Historical V9 Payment Spine

V9 established an early controlled payment-verification direction.

Its focus included:

- payment state verification
- backend-controlled trust boundaries
- receipt/evidence direction
- access locked before verification
- public-safe review flows

At the time, V9 was explicitly not presented as:

- Pi Mainnet settlement
- production readiness
- a formal audit

Those historical claim boundaries remain part of the archived record.

---

## Historical V9.1 Backend Behavior Validation

V9.1 explored negative and fail-secure behavior.

The architectural principle was:

**If verification is incomplete, ambiguous, mismatched, or unknown, SafeGate should not create a verified trust outcome.**

That principle remains conceptually important in the current SafeGate architecture.

---

## Historical V11 Hardening Work

This repository also contains V11 hardening materials covering areas such as:

- replay handling
- duplicate behavior
- mismatch scenarios
- failure handling
- controlled implementation scope
- test planning

Relevant historical files are preserved in the repository root.

They are retained as development evidence rather than current runtime documentation.

---

## Historical V13 Controlled Hardening

V13 focused on areas including:

- duplicate callback behavior
- idempotency
- replay resistance
- payment/invoice mismatch handling
- timeout and ambiguous verification
- durable state failure
- public verify safety
- safe error output
- access unlock regression
- fee settlement confirmation

Historical V13 public-surface and policy-simulation work is preserved here.

Important:

The original V13 statements described the state of the project at that time.

They must not be used to infer the current status of SafeGate's later Base, agent, MCP, SDK, middleware, or commerce-proof work.

---

## Historical Agent-Readable Trust Direction

At the time this archive was created, agent-readable SafeGate functionality was still described as future work.

That statement is now historical.

Current SafeGate work includes:

- an MCP / Agent Tool
- public verification contracts
- a public JavaScript SDK
- agent-oriented commerce verification
- explicit assurance semantics

Current MCP and SDK behavior is documented in:

https://github.com/Nurexen-Labs/safegate-xagent-commerce-outcome

Do not use this archive's old statements such as “no MCP endpoint” or “no agent execution” as current product claims.

---

## Historical Privacy Direction

Earlier SafeGate work described the product as privacy-aware rather than as a privacy protocol.

That distinction remains useful.

SafeGate itself is not intended to be:

- a mixer
- a private payment network
- a custody system
- an escrow system

Privacy-preserving payment rails can exist underneath SafeGate.

SafeGate's role is to provide commerce verification and evidence above or around those rails.

---

## Historical Fee Architecture

The archive contains early fee-model exploration, including concepts such as transparent verification fees and payment-state-dependent finalization.

Those materials are architectural history.

They do **not** define the current commercial model or current SDK/API pricing.

---

## Current Assurance Model

The active SafeGate implementation now distinguishes evidence strength explicitly.

### CLAIMED

The provider or source supplied the outcome assertion.

The evidence may be authenticated, but SafeGate has not independently observed or validated the underlying fulfillment.

### OBSERVED

SafeGate observed execution evidence such as a paid request, execution, response, or equivalent runtime evidence.

### VALIDATED

An independent mechanism verified the outcome.

Possible mechanisms include:

- independent re-execution
- trusted execution environment evidence
- zero-knowledge evidence
- independent validators

### THIRD_PARTY_ATTESTED

A separately identified external party contributed evidence under its own trust mechanism.

Important rule:

**Payment verification alone does not prove fulfillment.**

The current SafeGate implementation must not silently promote provider claims into independent validation.

---

## Current Product Surfaces

For current implementation details, use the active repository.

Current productization includes:

- Public Verify API
- Observed Middleware Core
- MCP / Agent Tool
- public JavaScript SDK
- Developer Quickstart
- Base Mainnet USDC verification adapter

Public SDK:

    @nurexenlabs/safegate-sdk@0.3.0

Install:

    npm install @nurexenlabs/safegate-sdk

---

## What SafeGate Is Not

Across both the historical and current architecture, SafeGate is not intended to be:

- a wallet
- a payment processor
- a custodian
- an escrow service
- a replacement for payment rails
- a system that requires users to expose private keys or seed phrases

SafeGate's role is commerce assurance and evidence.

---

## Repository Purpose

This repository is retained as a public-safe historical engineering archive.

It may contain:

- architecture documents
- validation plans
- hardening backlogs
- pilot evidence
- hackathon materials
- historical product positioning
- fee architecture discussions
- public review artifacts
- claim-boundary documentation

It must not contain:

- private keys
- seed phrases
- wallet secrets
- API secrets
- database credentials
- Vercel environment secrets
- Supabase service-role keys
- Pi app secrets
- private merchant data
- sensitive user data

---

## Important Reading Rule

When information in this repository conflicts with the active SafeGate technical repository, the active repository is the current source of truth.

Current technical source of truth:

https://github.com/Nurexen-Labs/safegate-xagent-commerce-outcome

Current public SDK:

https://www.npmjs.com/package/@nurexenlabs/safegate-sdk

Current website:

https://safegatelabs.xyz

---

## Historical Value

This archive exists because engineering history matters.

The progression from:

    Pi/Testnet payment trust
          ->
    post-payment evidence
          ->
    replay and failure hardening
          ->
    agent-readable trust
          ->
    rail-agnostic commerce assurance
          ->
    Public Verify + Middleware + MCP + SDK

shows how SafeGate's current architecture emerged.

The older work is not discarded.

It is preserved here with the correct historical context.

---

**SafeGate — assurance infrastructure for programmable commerce.**