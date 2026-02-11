# Dynamis Technical Documentation

**Technical documentation for the Dynamis Agentic Operating System.**

This repository contains public-facing documentation covering the economic model, formal verification status, system design, and developer onboarding for the Dynamis platform.

---

## Contents

### [Trust Economics](TRUST_ECONOMICS.md)

The economic model that powers Dynamis. Capacity Bonding replaces gas fees. Agents bond assets to reserve verification power, and reputation across five actor types rewards long-term participation. Covers supply mechanics, bonding curves, and fee reduction tiers.

### [Formal Verification](FORMAL_VERIFICATION.md)

Status and methodology for the 30+ TLA+ specifications that verify Dynamis protocol safety. Includes component-by-component descriptions of what each specification proves and summary of recent verification results.

### [Developer Guide (Preview)](DEVELOPER_GUIDE.md)

A preview of the upcoming developer experience for building on Dynamis. Covers the planned SDK, agent deployment workflow, and supported language targets (Rust, AssemblyScript, Zig to WebAssembly). The full SDK and tooling are under active development.

### [System Design](DYNAMIS_OPTIMIZED_DIGITAL_SYSTEM.md)

Technical vision document covering the mathematics (Topos Theory, Sheaf Cohomology), software architecture (Java 25, WASM), and system adaptability of the Dynamis platform.

---

## Related Repositories

| Repository | Description |
|-----------|-------------|
| [dynamis-specs](https://github.com/DynamisNetwork/dynamis-specs) | TLA+ formal specifications |
| [dynamis-papers](https://github.com/DynamisNetwork/dynamis-papers) | Research papers |

---

## License

Apache 2.0

Copyright 2026 Dynamis Network. Built by Cryptozoa.
