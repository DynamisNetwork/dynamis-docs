# Dynamis Developer Guide (Preview)

> This document previews the upcoming developer experience for building on Dynamis. The SDK, 
> CLI tooling, and public testnet are under active development.

---

## Platform Overview

Dynamis is an Agentic Operating System for coordinating and verifying autonomous AI agents. Developers write agent policies in systems languages, compile them to WebAssembly, and deploy them to the Dynamis network where they operate under topological consensus via Sheaf Neural Networks.

---

## Supported Languages

| Language | Purpose | Compilation Target |
|----------|---------|-------------------|
| **Rust** | High-performance agent policies | `wasm32-unknown-unknown` |
| **AssemblyScript** | Familiar TypeScript-like syntax | `wasm32` |
| **Zig** | Low-level control, zero allocations | `wasm32-freestanding` |
| **Java 25** | Core runtime and infrastructure | JVM (server-side) |
| **Python** | AI/ML training and integration | Native (off-chain) |

> Note: EVM/Solidity is intentionally not supported. Dynamis uses WASM as its execution target.

---

## Agent Deployment Workflow (Planned)

### 1. Write Agent Logic

```rust
use dynamis_agent::*;

#[dynamis_agent]
pub struct RiskClassifier {
    threshold: f64,
}

#[dynamis_methods]
impl RiskClassifier {
    pub fn on_inference(&self, ctx: &Context) {
        let result = ctx.oracle.query("classify_risk", &ctx.payload);
        if result.confidence > self.threshold {
            ctx.commit(result); // Writes to immutable memory
        } else {
            ctx.flag_uncertainty(result); // Triggers j-Learning
        }
    }
}
```

### 2. Compile to WASM

```bash
cargo build --target wasm32-unknown-unknown --release
```

### 3. Deploy

```bash
dynamis deploy ./target/wasm32-unknown-unknown/release/risk_classifier.wasm

# Output:
# Agent deployed: agt1abc...
# Sheaf registration: complete
# Oracle binding: holoflux-v1
```

---

## Data Encoding

Dynamis uses Bech32 encoding for all on-chain identifiers:

| Prefix | Type | Description |
|--------|------|-------------|
| `dyn1...` | User Wallet | Standard account |
| `ai1...` | AI Agent | Autonomous agent account |
| `con1...` | Contract | Smart contract address |
| `txn1...` | Transaction | Transaction hash |
| `blk1...` | Block | Block hash |
| `ins1...` | Inscription | Permanent record ID |

---

## Platform Features

### Post-Quantum Cryptography

All transactions are signed with NIST Dilithium-3 (FIPS 204). Key exchange uses ML-KEM-768. The SDK handles all cryptographic operations automatically.

### Holoflux Oracle

Agents query Foundation Models (Vertex AI, GPT-4, local ONNX) through the Holoflux Oracle, which provides cryptographically verified inference results validated against the Sheaf topology.

### Trust Economics

No gas fees. Agents bond capacity to reserve verification power. Five actor types (Validators, Wallets, Agents, Smart Contracts, Liquidity Providers) earn reputation scores that reduce costs over time.

### Immutable Agent Memory

On-chain inscriptions serve as permanent shared memory for agent swarms. Once an agent commits data, it cannot be altered or "forgotten."

### Safety Systems

- **Soteria**: Dual-factor authentication (passkey + crypto signature)
- **Guardian Network**: Social recovery via trusted agents or devices
- **Time-Lock Vault**: Suspicious transactions delayed 4-72 hours
- **Sentinel AI**: Autonomous anomaly detection with risk scoring

---

## Formal Verification

Dynamis provides 30+ TLA+ specifications as templates for verifying agent logic. See the [dynamis-specs](https://github.com/DynamisNetwork/dynamis-specs) repository for the full library.

```bash
# Verify your agent's economic invariants
java -jar tla2tools.jar -config MyAgent.cfg MyAgent.tla
```

---

## Coming Soon

- Public testnet
- CLI (`dynamis-cli`) with deploy, inspect, and monitor commands
- SDK packages for Rust, Python, TypeScript, and Java
- Agent marketplace
- Block explorer and dashboard

---

## Stay Updated

- Website: [dynamisnetwork.com](https://www.dynamisnetwork.com)
- Twitter/X: [@DynamisNetwork](https://x.com/dynamisnetwork)
- Research: [Zenodo](https://zenodo.org/records/18445406)

---

*Preview Document. Version 0.1. February 2026.*
