# aztec-packages

Privacy-first L2 zk-rollup on Ethereum.

All paths in this file are relative to the git root.

## Aztec Rules (IMPORTANT — read all of these)

Before starting work, read every file in `.claude/aztec-rules/`. These are cross-cutting rules that apply to all components:

- `monorepo.md` — component map, build order, cross-component rebuild instructions
- `common-mistakes.md` — build system, proof size constants, testing, and code style pitfalls
- `git-workflow.md` — branch strategy, merge trains, commit conventions
- `red-green-testing.md` — always show a failing test before fixing (red then green)
- `attribution.md` — attribute work to the git author only, no Claude co-author lines
- `session-analysis.md` — at end of task, offer to analyze the session JSONL for errors and improvements
