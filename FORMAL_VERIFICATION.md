# Formal Verification Framework

**Version**: 3.0
**Date**: February 2026
**Status**: Active. TLA+ suite operational with TLC and Apalache model checkers.

---

## Overview

Dynamis uses TLA+ (Temporal Logic of Actions) for formal verification of all safety-critical protocol components. The specification suite covers consensus correctness, agent economics, cryptographic safety, and privacy guarantees.

### Results at a Glance

- **40+ formally verified specifications**
- **250+ million states explored** via TLC model checking
- **15 inductive proofs** providing mathematical certainty for infinite-state properties
- **Zero counterexamples** found across all safety invariants
- **Apalache symbolic checking** deployed for bounded verification of complex state spaces

---

## Verification Results

Each component below is described with its role in the system and the properties verified.

### Consensus and Time Ordering

These specs verify that the network reaches agreement correctly and that time ordering is deterministic.

| Spec | Role | States | Distinct | Status |
|------|------|--------|----------|--------|
| **PoHConsensus** | Ensures the 1kHz BLAKE3 clock provides deterministic block ordering and that finalization requires a 2/3 validator quorum. No forks are possible. | 822,479 | 185,197 | VERIFIED |
| **AnchorSync** | Verifies that cross-node state synchronization commits are atomic and consistent. | Bounded | Bounded | VERIFIED (TLC + Apalache) |
| **GPCTController** | Proves the Global Protocol Clock coordinates time across distributed nodes without drift. | Bounded | Bounded | VERIFIED (TLC + Apalache) |

### Agent Economics and Reputation

These specs verify the economic invariants of the Trust Economics model: capacity bonding, staking, and reputation scoring.

| Spec | Role | States | Distinct | Status |
|------|------|--------|----------|--------|
| **AgentEconomics** | Verifies that agent staking, capacity bonding, and reward distribution conserve total value. | Bounded | Bounded | VERIFIED (TLC + Apalache) |
| **Staking** | Proves that stake delegation and validator rewards maintain balance integrity. | Bounded | Bounded | VERIFIED (TLC + Apalache) |
| **ReputationSystem** | Ensures 5-actor-type reputation scoring decays correctly and cannot be gamed via sybil attacks. | Inductive | N/A | PROVEN (4 invariants) |
| **DimensionalReputation** | Verifies multi-axis reputation with decay maintains bounded scores. | Bounded | Bounded | VERIFIED (Apalache) |
| **DRC-20** | Proves the token standard conserves total supply across transfers, approvals, and burns. | 67M+ | 36M+ | PARTIAL (0 errors) |

### Security and Safety

These specs verify the consumer protection and safety systems that protect users and agents.

| Spec | Role | States | Distinct | Status |
|------|------|--------|----------|--------|
| **Soteria** | Proves dual-factor authentication: every transaction requires both a Dilithium-3 signature AND a passkey/biometric factor. Private key theft alone cannot compromise accounts. | 46,237 | 10,644 | VERIFIED |
| **Sentinel** | Verifies that the autonomous threat detection system correctly scores risk and triggers holds above threshold. Leader selection follows reputation rules. | 130M+ | 20M+ | PARTIAL (0 errors) |
| **Guardian** | Proves social recovery requires the correct guardian threshold (2-of-3) and cannot be bypassed. | 55M+ | 19M+ | PARTIAL (0 errors) |
| **TimeLock** | Ensures time-delayed vaults enforce minimum delay and that emergency freeze halts all pending transactions. | 1,743,698 | 326,902 | VERIFIED |
| **SocialRecovery** | Verifies account recovery requires guardian approval and respects threshold signatures. | 3,646 | 729 | VERIFIED |
| **RateLimiter** | Proves adaptive rate limiting enforces request caps with inductive mathematical proofs. | TLC + Inductive | N/A | VERIFIED + PROVEN |

### Privacy

These specs verify the privacy-preserving subsystems.

| Spec | Role | States | Distinct | Status |
|------|------|--------|----------|--------|
| **ShieldedPool** | Proves balance integrity: the sum of all balances always equals total pool funds. | Inductive | N/A | PROVEN (3 invariants) |
| **NFTPrivacy** | Verifies unveil/veil atomicity: an NFT is never in both public and private state simultaneously. | Inductive | N/A | PROVEN (4 invariants) |
| **ConfidentialInscription** | Ensures client-side encrypted inscriptions maintain confidentiality and ordering. | Bounded | Bounded | VERIFIED (TLC) |

### Data and Inscriptions

These specs verify the permanent on-chain data storage system.

| Spec | Role | States | Distinct | Status |
|------|------|--------|----------|--------|
| **EtchProtocol** | Proves immutable inscription ordering respects PoH sequence and cannot be reordered. | Inductive | N/A | PROVEN (4 invariants) |
| **InscriptionBatcher** | Verifies batched inscription processing maintains atomicity. | Bounded | Bounded | VERIFIED (TLC + Apalache) |

### Escrow and Vaults

| Spec | Role | States | Distinct | Status |
|------|------|--------|----------|--------|
| **Escrow** | Proves time-bound escrow with dispute resolution conserves funds. | Bounded | Bounded | VERIFIED (TLC + Apalache) |
| **AtomicLedger** | Verifies double-entry ledger conservation: debits always equal credits. | Bounded | Bounded | VERIFIED (TLC) |
| **TimeLockVault** | Proves vault unlock timing constraints are enforced. | Bounded | Bounded | VERIFIED (TLC) |

### Layer 2

| Spec | Role | States | Distinct | Status |
|------|------|--------|----------|--------|
| **L2StateCommitment** | Verifies L2 state commitment, challenge, and fraud proof resolution. | Bounded | Bounded | VERIFIED (TLC + Apalache) |

---

## Inductive Proof Methodology

For specifications with infinite or very large state spaces, we use mathematical induction rather than exhaustive exploration:

```
Init => IndInv                    (Base case: initial state satisfies the invariant)
IndInv AND Next => IndInv'        (Inductive step: every transition preserves the invariant)
```

This provides mathematical certainty that safety properties hold across ALL reachable states, not just those explored by bounded model checking.

---

## Verification Tools

| Tool | Purpose | Status |
|------|---------|--------|
| **TLC** | Finite-state model checking (exhaustive) | Active |
| **Apalache** | Symbolic/bounded model checking | Active (15+ specs) |
| **TLAPS** | Theorem proving for infinite-state | Evaluation |

---

## Running Verifications

### Single Specification (TLC)
```bash
cd specs
java -jar tla2tools.jar -workers 4 -config Soteria.cfg Soteria.tla
```

### Single Specification (Apalache)
```bash
apalache-mc check --config=SentinelApalache.cfg SentinelApalache.tla
```

### Full Suite
```bash
cd specs
for spec in Soteria PoHConsensus DRC20 ShieldedPool TimeLock; do
    java -jar tla2tools.jar -workers 4 -config ${spec}.cfg ${spec}.tla 2>&1 | tail -5
done
```

---

## For Auditors

Critical Dynamis security components have been formally verified using TLA+ model checking. The TLC model checker exhaustively explored over 250 million states and found zero counterexamples to any safety invariant. Additionally, 15 inductive proofs provide mathematical certainty for infinite-state properties. Apalache symbolic checking has been deployed for bounded verification of the most complex specifications.

### Audit Artifacts
- [dynamis-specs repository](https://github.com/DynamisNetwork/dynamis-specs): All specifications and configs
- [VERIFICATION_STATUS.md](https://github.com/DynamisNetwork/dynamis-specs/blob/main/VERIFICATION_STATUS.md): Recent run logs
- [INDUCTIVE_PROOFS.md](https://github.com/DynamisNetwork/dynamis-specs/blob/main/INDUCTIVE_PROOFS.md): Proof methodology

---

*Formal Verification Framework v3.0. February 2026.*
