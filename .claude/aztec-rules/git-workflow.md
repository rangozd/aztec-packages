# Git Workflow

## Branch Strategy

- **Primary development branch**: `next` (default PR target)
- **Production branch**: `master`
- **Merge trains**: Some teams batch PRs via `merge-train/*` branches before merging to `next`:
  - `merge-train/barretenberg` — barretenberg PRs
  - `merge-train/avm` — AVM PRs
  - `merge-train/spartan` — spartan PRs
- Never target `master` or `main` for PRs without explicit approval

## Commit Messages

Follow conventional commits: `fix:`, `feat:`, `chore:`, `refactor:`, `docs:`, `test:`

## Common Git Pitfalls

- Always `git fetch` before creating branches to avoid stale bases
- If `noir/noir-repo` shows as modified but you didn't change it, run `git submodule update noir/noir-repo`
- PRs are squashed to a single commit on merge — during development just create normal commits
