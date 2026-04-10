# Monorepo Structure and Build System

All paths below are relative to the git root.

## Components

- `barretenberg/` — C++ ZK proving system (Honk, Chonk, ECCVM). See `barretenberg/CLAUDE.md` and `barretenberg/cpp/CLAUDE.md`
- `barretenberg/cpp/src/barretenberg/vm2/` — AVM (Aztec Virtual Machine) for public execution. See its CLAUDE.md
- `barretenberg/sol/` — Solidity on-chain verifier. See `barretenberg/sol/CLAUDE.md`
- `barretenberg/ts/` — TypeScript bindings for barretenberg (bb.js)
- `avm-transpiler/` — Transpiles Noir bytecode to AVM bytecode (Rust)
- `noir/` — Noir compiler (git submodule pointing to noir-lang/noir)
- `noir-projects/` — Protocol circuits and contract libraries written in Noir. See `noir-projects/aztec-nr/CLAUDE.md`
- `l1-contracts/` — Solidity L1 rollup contracts (Foundry project)
- `yarn-project/` — TypeScript monorepo: node, client SDK, sequencer, prover, p2p, and tooling. See `yarn-project/CLAUDE.md`
- `docs/` — Developer documentation site (Docusaurus). See `docs/CLAUDE.md`
- `spartan/` — Kubernetes deployment infrastructure (Helm charts + Terraform). See `spartan/CLAUDE.md`
- `bb-pilcom/` — PIL compiler for AVM relation codegen
- `ci3/` — CI infrastructure scripts

## Build Order

Dependencies flow: `barretenberg → noir → l1-contracts → yarn-project`

From the git root, use `make <target>`:
- `make fast` — builds everything needed for development
- `make yarn-project` — full TS build chain (builds bb, noir, l1-contracts first)
- `make bb-cpp-native` — barretenberg C++ native only
- `make noir` — Noir compiler
- `make l1-contracts` — Solidity contracts via Foundry

For individual components, run `./bootstrap.sh` inside each directory.

## Cross-Component Changes

When your change spans multiple components, rebuild in dependency order:

1. `barretenberg/cpp/` — `cmake --preset default && cd build && ninja`
2. `barretenberg/ts/` — `./bootstrap.sh` (generates TS bindings from C++)
3. `noir/` — `./bootstrap.sh` (if noir changes needed)
4. `noir-projects/` — compile contracts
5. `l1-contracts/` — `forge build`
6. `yarn-project/` — `yarn build` (from inside `yarn-project/`, not the git root)
