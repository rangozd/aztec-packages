# Common Mistakes to Avoid

These are real errors observed across many sessions. Read carefully — they save significant tokens and time.

## Git Workflow
- Default PR base branch is `next`, NOT `master`
- Barretenberg PRs target `merge-train/barretenberg`
- yarn-project PRs target `merge-train/spartan`
- AVM PRs target `merge-train/avm`
- Always `git fetch` before creating branches to avoid stale bases
- If `noir/noir-repo` shows as modified, run `git submodule update noir/noir-repo`

## Code Style
- TypeScript: 120 character line width (not 80). Use `yarn format` before committing.
- C++: format with `clang-format-20 -i <files>` before committing
- Noir: max 120 characters per line, use `panic!()` not `assert()` for unreachable code.
- Rust: run `cargo fmt` before committing.
