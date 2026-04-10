# aztec-packages

Privacy-first L2 zk-rollup on Ethereum. Monorepo containing the full stack: proving system, smart contract language, protocol circuits, L1 contracts, and TypeScript node/client.

## Monorepo Structure

- `barretenberg/` — C++ proving system (Honk, Chonk, ECCVM, AVM). See `barretenberg/CLAUDE.md`
- `noir/` — Noir compiler (git submodule). See `noir/noir-repo/CLAUDE.md` if it exists
- `noir-projects/` — Protocol circuits and contract libraries. See `noir-projects/aztec-nr/CLAUDE.md`
- `l1-contracts/` — Solidity L1 rollup contracts
- `yarn-project/` — TypeScript node, client, SDK, and tooling. See `yarn-project/CLAUDE.md`
- `docs/` — Developer documentation site. See `docs/CLAUDE.md`
- `spartan/` — Deployment infrastructure (Helm/Terraform). See `spartan/CLAUDE.md`

## Build Order

Dependencies flow: `barretenberg → noir → l1-contracts → yarn-project`

Use `make <target>` from repo root:
- `make yarn-project` — full build chain (everything)
- `make bb-cpp-native` — barretenberg C++ only
- `make noir` — Noir compiler
- `make l1-contracts` — Solidity contracts

For individual components, use `./bootstrap.sh` inside each directory.

## Aztec Rules (IMPORTANT — read these)

Core rules for all work in this repo live in `.claude/aztec-rules/`. These apply everywhere regardless of which component you're working in:

- **`.claude/aztec-rules/common-mistakes.md`** — Build system gotchas, proof size constants, git workflow, testing, and code style mistakes that waste tokens. Read this first.
- **`.claude/aztec-rules/red-green-testing.md`** — Always show a failing test before fixing. Red then green.
- **`.claude/aztec-rules/attribution.md`** — Attribute work to the git author only. No Claude co-author lines.
- **`.claude/aztec-rules/session-analysis.md`** — At the end of every task, offer to analyze the session JSONL for errors and improvement opportunities.

## Cross-Component Changes

When your change spans multiple components, rebuild in dependency order:

1. `barretenberg/cpp` → `cmake --preset default && cd build && ninja`
2. `barretenberg/ts` → `./bootstrap.sh` (generates TS bindings from C++)
3. `noir/` → `./bootstrap.sh` (if noir changes needed)
4. `noir-projects/` → compile contracts
5. `l1-contracts/` → `forge build`
6. `yarn-project/` → `yarn build`

After changing proof sizes in barretenberg, you MUST update constants in three places (see `.claude/aztec-rules/common-mistakes.md`).

## Git Workflow

- **Primary development branch**: `next` (default PR target)
- **Production branch**: `master`
- **Merge trains**: Some teams use `merge-train/*` branches (e.g., `merge-train/barretenberg`, `merge-train/avm`). PRs for those components target their merge-train branch, not `next`.
- Never target `master` or `main` for PRs without explicit approval
- Follow conventional commits: `fix:`, `feat:`, `chore:`, `refactor:`, `docs:`, `test:`
