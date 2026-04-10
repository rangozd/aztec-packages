# Common Mistakes to Avoid

These are real errors observed across many sessions. Read carefully — they save significant tokens and time.

## Build System
- Do NOT add `-j` flag to ninja — default parallelism is optimal
- Do NOT run `yarn build` from the repo root — run from `yarn-project/`
- After changing barretenberg C++, rebuild downstream: `bb → bb/ts → noir → yarn-project`
- Check AVM build state with `grep "AVM:" barretenberg/cpp/build/CMakeCache.txt` — AVM=ON persists across builds
- Docker is NOT available in all environments (including ClaudeBox). Do not attempt Docker commands.

## Proof Size Constants
- After any change affecting proof sizes, you MUST update three places:
  1. C++ static_asserts in `barretenberg/cpp/src/barretenberg/dsl/acir_format/mock_verifier_inputs.test.cpp`
  2. Noir constants in `noir-projects/noir-protocol-circuits/crates/types/src/constants.nr`
  3. TypeScript: run `yarn remake-constants` from `yarn-project/constants/`

## Testing
- Never run multiple e2e tests in parallel — they compete for ports
- Use `--runInBand` for packages with port conflicts (e.g., `ethereum`)
- E2e tests are slow — always run specific test files, not entire suites
- For IVC tests, ensure barretenberg native is built first

## Code Style
- TypeScript: 120 character line width (not 80)
- C++: format with `clang-format-20 -i <files>` before committing
- Noir: max 120 characters per line, use `panic!()` not `assert()` for unreachable code
